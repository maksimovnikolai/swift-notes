## Связный список

#### Абстрактная структура данных Node (узел)
```swift
class Node<T: Equatable>: Equatable {
 
    var value: T
    var next: Node? // Односвязный список
//  var previous: Node? // Двусвязный список
    
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
// car99.previous = car12 <->
```

* распечатываем текущее состояние связанного узла
чтобы напечатать подключенные узлы в читаемом виде,
класс Node реализует CustomStringConvertible
реализуем свойство (геттер) descrtiption и пишем логику печати
```swift
print(car12) // 12 -> 99 -> nil
```