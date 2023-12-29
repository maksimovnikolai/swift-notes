## Optionals

#### Опциональные типы данных, также называемые опционалами, - это особый тип, который говорить о том, что параметр либо имеет значение определенномго типа, либо вообще не имеет никакого значения.


```swift
let possibleString = "1945"
let convertPossibleString = Int(possibleString)
print(convertPossibleString)
// Optional(1945)

let unpossibleString = "Одна тысяча сто десять"
let convertUnpossibleString = Int(unpossibleString) // nil
```
---

#### Принудительное извлечение (force unwrapping)
```swift
var optVar: Int? = 12
var intVar = 34
let result = opt! + 34 // 46
```

#### Косвенное извлечение значения (implicity unwrapping)
```swift
var name: String!
name = "Bob"

let unwrappedName: String = name
print(unwrappedName)
// Bob
```

#### Оператор объединения с nil (nil coalescing)
```swift
let optionalInt: Int? = 20
var mustHaveResult = optionalInt ?? 0 // 20
```

#### Опциональное связываание (optional binding)
```swift
var name: String?
if let newName = name {
    print("the name is \(newName)")
} else {
    print("newName is nil")
}
// newName is nil

 name = "John"

if let newName = name {
    print("the name is \(newName)")
} else {
    print("newName is nil")
}
// the name is John

```

#### Оператор досрочного выхода (guard)
```swift
func printMessage(message: String?) {
    guard let messageRetrieved = message else {
        print("no message recieved")
        return // выход из функции
    }
    print(messageRetrieved)
}

printMessage(message: nil)
// no message recieved
```

#### Опциональная последовательность (optional chaining)
```swift
class Person {
    var id: String?
    var residence: Residence?
}

class Residence {
    var address: Address?
}

class Address {
    var buildingNumber: String?
    var streetName: String?
    var apartmentNumber: String?
}

var person: Person?

if let residence = person?.residence {
    if let address = residence.address {
        if let apartmentNumber = address.apartmentNumber {
            print("The apartment number: \(apartmentNumber)")
        }
    }
}

if let apartmentNumber = person?.residence?.address?.apartmentNumber {
    print("The apartment number: \(apartmentNumber)")
}
```