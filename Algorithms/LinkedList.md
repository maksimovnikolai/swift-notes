## Односвязный список

#### Абстрактная структура данных Node (узел)
```swift
class Node<T: Equatable>: Equatable {
 
    var value: T
    var next: Node? // Односвязный список
    
    // реализация метода протокола Equatable
    // (lhs) tail == (rhs) head
    static func == (lhs: Node<T>, rhs: Node<T>) -> Bool {
        // T && T == true
        // T && F == false
        return lhs.value == rhs.value && lhs.next == rhs.next
    }
    
    init(_ value: T) {
        self.value = value
    }
}

// реализация CustomStringConvertible для настройки описания узла
extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value) -> nil" // 12 -> nil
        }
        
        // если у нас есть соединенные узлы (node)
        return "\(value) -> \(next)"
    }
}
```
* создание экземпляров узлов

```swift
let car12 = Node<Int>(12)
let car99 = Node<Int>(99)
```

* связывание узлов (связный список с использованием связанных узлов)
```swift
car12.next = car99
```

* распечатываем текущее состояние связанного узла
чтобы напечатать подключенные узлы в читаемом виде,
класс Node реализует CustomStringConvertible
реализуем свойство (геттер) descrtiption и пишем логику печати
```swift
print(car12) // 12 -> 99 -> nil
```

## Linked List

```swift
class LinkedList<T: Equatable> {
  var head: Node<T>? // nil
  var tail: Node<T>? // nil
  
  // первый элемент в списке
  public var first: Node<T>? {
    return head
  }
  
  // последний элемент в списке
  public var last: Node<T>? {
    return tail
  }
  
  // наличие элементов в списке
  public var isEmpty: Bool {
    print("empty list")
    return head == nil // если head равен nil, то список пуст
  }
  
  // метод append — добавляет узел в конец списка.
  public func append(_ value: T) {
    // создать Node (узел)
    let newNode = Node(value)
    // сценарий 1: добавление в пустой список
    guard let lastNode = tail else {
      // пустой список
      head = newNode
      tail = head
      return
    }
    // сценарий 2: существующие узлы
    lastNode.next = newNode
    tail = newNode
  }
  
  // removeLast method - удаляет последний узел из конца списка
  public func removeLast() -> Node<T>? {
    // сценарий 1 – пустой список
    if isEmpty {
      return nil
    }
    var removedNode: Node<T>?
    if head == tail {
      // сценарий 2 — если head и tail указывают на один и тот же узел
      removedNode = head
      head = nil
      tail = nil
      return removedNode
    }
      
    // сценарий 3 — итерация, обход, обход связанного списка, начиная с головы (head)
    var currentNode = head
    
    while currentNode?.next != tail { // остановиться в узле перед хвостом (tail)
      currentNode = currentNode?.next // увеличить currentNode на 1
    }
    
    // где находится currentNode в конце цикла while
    
    // сохраните узел хвоста перед удалением последнего узла (хвоста)
    removedNode = tail
    
    // установите хвост (tail) в Node перед последним node (узлом)
    tail = currentNode
    
    currentNode?.next = nil
    
    return removedNode
  }
}
```

* **реализация протокола CustomStringConvertible**
```swift
extension LinkedList: CustomStringConvertible {
  // stored property
  // var name = 90 - НЕ КОМПИЛИРУЕТСЯ, РАСШИРЕНИЯМ НЕ ДОПУСКАЕТСЯ СОХРАНЯТЬ СВОЙСТВА
  
  // computed property
  var description: String {
    guard let head = head else {
      return "empty list"
    }
    return "\(head)"
  }
}
```

* **инициализация пустого списка, добавление и удаление элементов из списка**

```swift
let fellows = LinkedList<String>()
fellows.append("Oscar")
fellows.append("Tanya")
fellows.append("David")

print(fellows) // Oscar -> Tanya -> David -> nil

fellows.removeLast() // метод removeLast возвращает удаляемый узел

print(fellows) // Oscar -> Tanya -> nil

fellows.append("Luba")

print(fellows) // Oscar -> Tanya -> Luba -> nil

fellows.removeLast() // Luba -> nil
fellows.removeLast() // Tanya -> nil
fellows.removeLast() // Oscar -> nil
fellows.removeLast() // nil
fellows.removeLast() // nil
fellows.removeLast() // nil
```