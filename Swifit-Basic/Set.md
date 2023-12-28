## Set

#### Множество - это неупорядоченная коллекция уникальных элементов. У элементов множества нет четкого порядка следования. Определенное значение элемента может существовать в нем лишь единожды, то есть каждое значение в пределах одного множества должно быть уникальным.
---

### Создание множества

#### 
```swift
var characters: Set<Character> = [] // Рекомендованный способ создания пустого множества
var numbers: Set = [1, 2, 3, 4, 5]
let mySet = Set(arrayLiteral: 5, 66, 12)
```

### Базовые свойства и методы множеств

#### - insert(_ : )
* в результате выполнения метода insert, возвращается кортеж, первый элемент которого содержит значение типа Bool? характерезующее успешность проведенной операции. Если возвращен true - элемент успешно добавлен, если false - он уже существует во множестве.
```swift
var musicStyleSet: Set<String> = []
musicStyleSet.insert("Jazz") // (inserted true, memberAfterInsert "Jazz")
musicStyleSet // {"Jazz}
```

#### - remove(_ : ), removeAll()
* уничтожает элемент с указанным значением и возвращает его значение или nil, если такого элемента не существует. 
```swift
let albums Set = ["Jazz", "Hip-Hop", "Rock"]
var removeAlbum = albums.remove("Hip-Hop")
removeAlbum // "Hip-Hop"
albums // {"Jazz", "Rock"}

albums.remove("Classic") // nil
albums.removeAll()
albums // Set([])
```


#### - contains(_ : )  => Bool
```swift
let newAlbums: Set = ["Jazz", "Hip-Hop", "Rock", "Funk"]
newAlbums.contains("Funk") // true
 newAlbums.contains("Pop") // false
```

#### - update(with_ : )
```swift
var genericSet1: Set<String> = ["John", "Sarah"]
var oldValue1 = genericSet1.update(with: "Sarah")
print(oldValue1) // Optional("Sarah")
print(genericSet1) // ["Sarah", "John"]

oldValue1 = genericSet1.update(with: "Alex")
print(oldValue1) // nil
print(genericSet1) // ["Alex", "John", "Sarah"]
```


#### - filter(Element -> Bool) -> Set<Element>
```swift
var numbers: Set = [2, 3, 1, 4, 5]
let filteredElements = numbers.filter { $0 > 3 }
print(filteredElements)
// [5, 4]
```


#### - remove(Element) -> Element?
```swift
var numbers: Set = [2, 3, 1, 4, 5]
let removedElement = numbers.remove(1)
print(removedElement ?? 0) // 1
print(numbers)
// [2, 4, 3, 5]
```

####  - removeFirst() -> Element
```swift
struct Person: Hashable {
    let name: String
    let age: Int
}

let john = Person(name: "John", age: 5)

var persons: Set = [john, john]

var numbers: Set = [2, 3, 4, 5]

let index = numbers.firstIndex(of: 5)
if let foundIndex = index {
    numbers.remove(at: foundIndex)
}
print(numbers)
// [2, 3, 4]
```

#### - enumerated()
```swift
var names: Set = ["Alex", "Bob", "Tom", "Kim"]

for (index, name) in names.enumerated() {
    print("\(name) at index \(index)")
}
// Kim at index 0
// Alex at index 1
// Tom at index 2
// Bob at index 3
```

#### - sorted - возвращает массив
```swift
let setOfNums: Set = [1, 10, 2, 5, 12, 23]
let sortedArray = setOfNums.sorted()
print(sortedArray)
// [1, 2, 5, 10, 12, 23]
```
---

### Операции с наборами
#### 
```swift
let oddDigits: Set = [1, 3, 5, 7, 9] // set с нечетными цифрами
let evenDigits: Set = [0, 2, 4, 6, 8] // set с четными цифрами
let differentDigits: Set = [3, 4, 7, 8] // set со смешанными цифрами
```

#### - intersection(_ : ) - получить все общие элементы
```swift
let inter = differentDigits.intersection(oddDigits)
print(inter)
// [7, 3]
```

#### - symmetricDifference(_ : ) - получить все непересекающиеся (не общие) элементы
```swift
let exclusive = differentDigits.symmetricDifference(oddDigits)
print(exclusive)
//[1, 5, 4, 9, 8]
```

####  - union(_ : ) - получить элементы обоих множест
```swift
let union = evenDigits.union(oddDigits)
print(union)
// [6, 5, 0, 9, 8, 4, 1, 3, 2, 7]
```


#### - subtracting(_ : ) - получить элементы, которые входят в первое множество, но не входят во второе
```swift
let subtract = differentDigits.subtracting(evenDigits)
print(subtract)
// [7, 3]
```
---

### Отношения множеств

#### 
```swift
let aSet: Set = [1, 2, 3, 4, 5]
let bSet: Set = [1, 3]
let cSet: Set = [5, 6, 7, 8]

// Множество aSet - это надмножество для bSet, так как включает в себя все элементы из bSet.
// В то же время множество bSet - это подмножество для aSet, так как все элементы bSet существуют и в aSet.
// Множества cSet и bSet являются непересекающимися, так как у них нет общих элементов, а множество aSet и cSet - пересекающиеся, так как имеют общие элементы.
// Два множества считаются эквивалентными (==), если у них один и тот же комплект элементов.
```

####  
```swift
let copyOfBSet = bSet
bSet == copyOfBSet // true
```


#### - isSubset(of:) - является ли одно множество подмножеством другого
```swift
// вернет true, даже если множества равны
bSet.isSubset(of: aSet) // true
```

#### - isSuperset(of:) - является ли множество надмножеством для другого
```swift
// вернет true, даже если множества равны
aSet.isSuperset(of: bSet) // true
```

#### - isDisjoint(with:) - существуют ли в двух множествах общие элементы, и в случае их отсутствия возвращает true
```swift
bSet.isDisjoint(with: cSet) // true
```


#### - isStrictSubset(of:) и isStrictSuperset(of:) - является множество подмножеством или надмножеством, не равным указанному множеству
```swift
bSet.isStrictSubset(of: aSet) // true
aSet.isStrictSuperset(of: bSet) // true
```