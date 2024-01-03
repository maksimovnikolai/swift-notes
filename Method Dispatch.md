## Method Dispatch (Диспетчеризация методов)

#### Диспетчеризация методов - это то, как программа выбирает, какие инструкции нужно выполнить при вызове определенного метода.

#### Имя любого метода - это ссылка на адрес в памяти где хранится код функции.
---

## Виды диспетчеризации:

* **Direct Dispatch (статическая диспетечеризация)**

* **Table Dispatch (табличная / динамическая диспетечеризация)**

* **Message Dispatch (на сообщениях / самая динамическая)**
---

## Управление диспетчеризацией

* **final** - меняет диспетчеризацию класса на direct (недоступно наследование)

* **private** - меняет диспетчеризацию метода на direct (недоступно наследование)

* **static** - меняет диспетчеризацию свойств класса на direct

* **dynamic** - меняет диспетчеризацию на method dispatch

* **final @objc** - статическая дисепетчеризация

* **@nonobjc** - позволяет отключить видимость методов в NSObject'e
---

## Статическая диспетчеризация (Direct Dispatch)
* **Самая быстрая диспетчерезация**
* Полностью разруливается на стадии компиляции (компилятор изначально знает куда нужно обратиться чтобы вызвать метод)
* Запрещает наследование и переопределение методов

* Если **Struct** или **Class** имеет **extension** в котором также есть методы - это тоже Direct Dispatch
* Все **extension** работают с Direct Dispatch

####  Value type -> direct dispatch
```swift
struct Greeter {
    func hello() {
        print("hello")
    }
}

let greeter = Greeter()
greeter.hello() // Direct Dispatch
```

#### Struct Extension -> direct dispatch
```swift

struct Greeter1 {
    func hello() {
        print("hello")
    }
}

extension Greeter1 {
    func hello(name: String) {
        print("Hello \(name)")
    }
}

let greeter1 = Greeter1()
greeter1.hello(name: "world") // Direct Dispatch
```

####  Class Extension -> direct dispatch
```swift
class Greeter3 {
    func hello() {
        print("hello")
    }
}

extension Greeter3 {
    func hello(name: String) {
        print("Hello \(name)")
    }
}

let greeter3 = Greeter3()
greeter3.hello(name: "world") // Direct Dispatch
```

#### Class: NSObject Extension:
```swift
class Greeter4: NSObject {
    func hello() {
        print("hello")
    }
}

extension Greeter4 {
    func hello(name: String) {
        print("Hello \(name)")
    }
}

let greeter4 = Greeter4()
greeter4.hello(name: "world") // Direct Dispatch
```
---

## Табличная диспетчеризция (Table Dispatch)
* В данном типе диспетчеризации формируется таблица с адресами методов сущности. Она создается на этапе компиляции, а сам адрес метода ищется по таблице уже во время runtime, что сказывается на скорости работы в худшую сторону.
* Табличная диспетчеризция (динамическая) - имеет два подвида: **Virtual table** и **Witnes table**

#### Virtual Table
* используется в основном для случаев наследования, то есть для классов. Для каждого класса и для каждого его сабкласса создается виртуальная таблица с адресами методов, при этом для дочернего класса эта таблица копируется вместе с адресами. В случае когда реализация метода полностью переходит из родительского класса, адрес в таблице наследника такой же. Если какой-то метод переопределен через **override**, то адрес меняется на другую реализацию. Ниже добавляются другие методы, которые добавились в этом сабклассе.

```swift

class MainClass {
    
    func method() {
    }
    
    func method1() {
    }
}

class ChildClass: MainClass {
    
    override func method1() {
    }
    
    func method2() {
    }
    
}
```
MainClass
| 0xA00 | MainClass | 
|-------|-----------|
| 0x121 | method    |
| 0x122 | method1   |


ChildClass
| 0xB00 | ChildClass |
|-------|------------|
| 0x121 | method     |
| 0x222 | method1    |
| 0x223 | method2    |

---

#### Witness Table
* Witness Table - таблица для работы с протокалами. Для класса реализующего протокол NewProtocol создается отдельная таблица с адресами методов протокола, и при вызове метода у объекта с типом протокола NewProtocol используется Witness Table диспетчеризация. В случае реализации классом нескольких протоколов, для каждого протокола будет создаваться отдельная таблица. Отличие от Virtual Table заключается в отсутствии наследования.

```swift
protocol NewProtocol {
    
    func protocolMethod()
}

class FirstClass: NewProtocol {
    
    func protocolMethod() {
        print("first class method")
    }
}

class SecondClass: NewProtocol {
    
    func protocolMethod() {
        print("second class method")
    }
}
```

FirstClass
| 0xxCA0 | NewProtocol    | 
|--------|----------------|
| 0xxB10 | protocolMethod |

SecondClass
| 0xxCA0 | NewProtocol    | 
|--------|----------------|
| 0xxB20 | protocolMethod |

**Witnes table - реализует полиморфизм, но не реализует наследование**

---

## Диспетчеризация на сообщениях (Message Dispatch)

* Message Dispatch — это самый динамичный вид диспетчеризации, который в основном используется в связке с Objective-C. Message Dispatch работает полностью в runtime. На примере ниже видим, что тоже присутствуют родительский и дочерний классы, но родительский класс при этом наследуется от NSObject. Для работы Message Dispatch, методу добавляется модификатор @objc dynamic. Для каждого класса тоже есть таблицы, но они, в отличии от Table Dispatch, не копируются при наследовании, в таблице дочернего класса есть ссылка super на родительский класс. При вызове метода у объекта дочернего класса, сначала идет поиск адреса у самого класса. Если не адрес не найден, то поиск продолжается у родительского класса, и так далее до NSObject.
Еще одно отличие от Table Dispatch – все таблицы в этом виде диспетчеризации формируются в runtime, что значительно снижает скорость определения имплементации метода и делает его самым медленным.


| 0xA00   | NSObject   | 
|:--------|-----------:|

| 0xB00 | ParentClass  | 
|:--------|-----------:|
| super(NSObject)      | 
| 0x121 | method       |
| 0x122 | method1      |


| 0xC00 | ChildClass   | 
|:-------|------------:|
| super(ParentClass)   |
| 0x222 | method1      |
| 0x223 | method2      |

---

## Еще несколько примеров

#### 1
```swift
protocol ChatProvider {
    func sendMessage()
    func openChat()
}

extension ChatProvider {
    func sendMessage() {
        print("Message")
    }
}

class Messenger: ChatProvider {
    func sendMessage() {}
    func openChat() {}
}

let messenger = Messenger()
messenger.sendMessage() // Virtual Table
messenger.openChat() // Virtual Table
```

#### 2
```swift
protocol ChatProvider2 {
    func sendMessage()
    func openChat()
}

extension ChatProvider2 {
    func sendMessage() {
        print("Message")
    }
}

class Messenger2: ChatProvider2 {
    func openChat() {}
}

let messenger2 = Messenger2()
messenger.sendMessage() // Static Table
messenger.openChat() // Virtual Table
```

#### 3
```swift
// пример 3 ---------------------------

protocol ChatProvider3 {
    func sendMessage()
    func openChat()
}

extension ChatProvider3 {
    func sendMessage() {
        print("Message")
    }
}

class Messenger3: ChatProvider3 {
    func openChat() {}
}

let messenger3: ChatProvider3 = Messenger3()
messenger.sendMessage() // Witness Table
messenger.openChat() // Witness Table
```

####  4
```swift
@objc protocol ChatProvider4 {
    func sendMessage()
    func openChat()
}

class Messenger4: NSObject {

}

extension Messenger4: ChatProvider4 {
    func openChat() {
        print("openChat")
    }

    func sendMessage() {

    }
}

let messenger4 = Messenger4()
messenger4.sendMessage() // Message Dispatch
messenger4.openChat() // Message Dispatch
```

#### 5
```swift
class Greeter8: NSObject {
    func hello() {
        print("Hello")
    }
}

extension Greeter8 {
    func goodBuy() {
        print("GoodBuy!")
    }
}

class ChatGreeter8: Greeter8 {
    override func hello() {
        print("Hi")
    }

    override func goodBuy() { // <- нельзя переопределить
         print("Bue-bue!")
    }
}

let chatGreeter8 = ChatGreeter8()
chatGreeter8.hello()
chatGreeter8.goodBuy()

// Данный код не скомпилируется.
// Расширение класса, в котором обьявлен метод goodBuy(),
// включает статическую диспетчеризацию,
// а значит его нельзя переопределить
```

#### 6
```swift
protocol TheProtocol {}

extension TheProtocol {
    func method() {
        print("in TheProtocol")
    }
}

struct TheStruct: TheProtocol {
    func method() {
        print("in TheStruct")
    }
}

let theStruct = TheStruct()
let theProtocol: TheProtocol = theStruct

theStruct.method() // virtual table
theProtocol.method() // static dispatch
```