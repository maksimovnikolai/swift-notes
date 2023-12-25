## Arrays / Массивы

##### **Массив - это упорядоченная коллекция однотипных элементов, для доступа к которым используются целочисленные индексы. Упорядоченной называется коллекция, в которой элементы распологаются в порядке, определенном разработчиком.** 
**Каждый элемент массива - это пара "индекс - значение".**
_**Индекс элемента массива**_ **- это целочисленное значение, используемое для доступа к значениям элемента.** 


#### Создание массива с помощью функции Array(arrayLiteral:)
```swift
let newArr = Array(arrayLiteral: 1, 2, 3)
print(newArr)
// [1, 2, 3]
```

#### Создание массива с помощью функции Array()
```swift
let newArr = Array(1...10)
print(newArr)
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### Создание массива с помощью функции Array(repeating: , count: )
```swift
let newArr = Array(repeating: "Swift", count: 5)
print(newArr)
// ["Swift", "Swift", "Swift", "Swift", "Swift"]
```

#### Создание пустого массива с типом [Int]
```swift
var integers = [Int]()
var anotherIntegers: [Int] = [] // Рекомендованный способ создания пустого массива
```

#### Создание массива путем сложения двух массивов вместе
```swift
let numbers1 = [1, 2, 3]
let numbers2 = [5, 9, 90]
let combinedArrays = numbers1 + numbers2
print(combinedArrays)
// [1, 2, 3, 5, 9, 90]
```

---
## Доступ к элементам массива

#### count 
```swift
let integersArr = [1, 11, 22, 34, 67]
print(integersArr.count)
// 5
```

#### isEmpty
```swift
var peopleNames: [String] = []
if peopleNames.isEmpty {
    print("peopleNames array is empty")
}
// peopleNames array is empty
```

#### Доступ к элементам массива по индексу
```swift
let languages = ["Swift", "Java", "Perl", "C"]
print("The first language in languages is \(languages[0])")
// The first language in languages is Swift
```


#### first / last
```swift
let values = [1, 2, 3, 4, 5]
let first = anotherLiteral.first
print(first)
// Optional(1)
```

```swift
let values: [Int] = []
if let first = values.first { // unwrapping the first?
    print(first)
} else {
    print("values is empty")
}
// values is empty
```

```swift
let values: [Int] = []
if !peopleNames.isEmpty { // !true => false
    let first = peopleNames[0]
    print(first)
} else {
    print("peopleNames is empty")
}
// peopleNames is empty
```

#### randomElement()
```swift
let randomValue = [4, 1, 2, 9, 0].randomElement()
print(randomValue)
// Optional(9)
```

#### изменение диапазона значений
```swift
var contacts = ["Bob", "Alex", "Sally", "Tom"]
contacts[1...3] = ["William", "Quincy"]
print(contacts)
// ["Bob", "William", "Quincy"]
```
---

#### Сравнение массивов ==
```swift
let integers1 = [1, 2, 3]
let integers2 = [2, 3]

integers1 == integers2 // false
integers1 != integers2 // true
```

#### Объединение элементов
```swift
let combine = [1, 2, 3] + [1, 2, 3]
print(combine)
// [1, 2, 3, 1, 2, 3]
```

#### append()
```swift
var contacts = ["Bob", "William", "Quincy"]
contacts.append("Alisa")
print(contacts)
// ["Bob", "William", "Quincy", "Alisa
```

#### append(contentsOf: )
```swift
var contacts = ["Bob", "William", "Quincy", "Alisa"]
contacts.append(contentsOf: ["John", "Eric"])
print(contacts)
// ["Bob", "William", "Quincy", "Alisa", "John", "Eric"]
```

#### insert
```swift
var contacts = ["Bob", "William", "Quincy", "Alisa", "John", "Eric"]
contacts.insert("Rachel", at: 1)
print(contacts)
// ["Bob", "Rachel", "William", "Quincy", "Alisa", "John", "Eric"]
```

#### remove(at: )
```swift
var contacts = ["Bob", "Rachel", "William", "Quincy", "Alisa", "John", "Eric"]
contacts.remove(at: contacts.count - 1)
print(contacts)
// ["Bob", "Rachel", "William", "Quincy", "Alisa", "John"]
```


#### removeLast()
```swift
var contacts = ["Bob", "Rachel", "William", "Quincy", "Alisa", "John"]
contacts.removeLast()
print(contacts)
//["Bob", "Rachel", "William", "Quincy", "Alisa"]
```

#### removeFirst()
```swift
var contacts = ["Bob", "Rachel", "William", "Quincy", "Alisa"]
contacts.removeFirst()
print(contacts)
// ["Rachel", "William", "Quincy", "Alisa"]
```

#### popLast()
```swift
var contacts = ["Rachel", "William", "Quincy", "Alisa"]
contacts.popLast()
// если массив contacts будет пуст, popLast() вернет nil, программа не упадет
print(contacts)
// ["Rachel", "William", "Quincy"]
```
#### removeAll()
```swift
var contacts = ["Rachel", "William", "Quincy"]
contacts.removeAll()
print(contacts)
// []
```

#### dropFirst()
```swift
1.
var contacts = ["Rachel", "William", "Quincy", "Alisa"]
let contactsSubsequence = contacts.dropFirst()
print(contactsSubsequence)
// ["William", "Quincy", "Alisa"]
```

```swift
2.
var contacts = ["Rachel", "William", "Quincy", "Alisa"]
let contactsSubsequence2 = contacts.dropFirst(2)
print(contactsSubsequence2)
// ["Quincy", "Alisa"]
```

#### dropLast()
```swift
1.
var contacts = ["Rachel", "William", "Quincy", "Alisa"]
let contactsSubsequence3 = contacts.dropLast()
print(contactsSubsequence3)
// ["Rachel", "William", "Quincy"]
```

```swift
2.
var contacts = ["Rachel", "William", "Quincy", "Alisa"]

let contactsSubsequence4 = contacts.dropLast(2)
print(contactsSubsequence4)
// ["Rachel", "William"]
```
---
## Итерации по массиву

#### Доступ только к элементу
```swift
var cities = ["New York", "Stockholm", "Boston", "Los Angeles"]
for city in cities { // for someElement in a Collection
    print(city)
}
// New York
// Stockholm
// Boston
// Los Angeles
```

#### Доступ к индексу, а также к элементу
```swift
var cities = ["New York", "Stockholm", "Boston", "Los Angeles"]
for city in cities {
    if city == "Boston"{
        print("city is \"Boston\"")
    }
}
// city is "Boston"
```

#### Получить доступ к индексу Бостона
```swift
var cities = ["New York", "Stockholm", "Boston", "Los Angeles"]
for (index, city) in cities.enumerated() {
    if city == "Boston"{
        print("city is \"Boston\" found at index: \(index)")
    }
}
// city is "Boston" found at index: 2
```
---
## Поиск элементов

#### contains()
```swift
var cities = ["New York", "Stockholm", "Boston", "Los Angeles"]
if cities.contains("Miami") { // contains() return true or false
    print("Sunshine state")
} else {
    print("city is not in array")
}
// city is not in array
```

#### firstIndex(of: )
```swift
var cities = ["New York", "Stockholm", "Boston", "Los Angeles"]
if let index = cities.firstIndex(of: "Los Angeles") {
    print("found city an index \(index)")
} else {
    print("city does not exist in cities array")
}
// found city an index 3
```

#### lastIndex(of: )
```swift
var cities = ["New York", "Stockholm", "Los Angeles", "Boston"]
if let index = cities.lastIndex(of: "Los Angeles") {
    print("found city an index \(index)")
} else {
    print("city does not exist in cities array")
}
// found city an index 2
```

#### min()
```swift
1.
let ages = [4, 1, 89, 3, 2, 56]
let minAge = ages.min()
print(minAge)
// Optional(1)
```
```swift
2.
let ages1 = [Int]()
let minAge1 = ages1.min()
print(minAge1)
// nil
```

#### max()
```swift
1.
var ages = [4, 1, 89, 3, 2, 56]
let maxAge = ages.max() ?? 0
print(maxAge)
// 89
```
```swift
2.
var ages = [4, 1, 89, 3, 2, 56]
let maxAge = ages.max() ?? 0
print(maxAge)
// вернет значение по умолчанию - 0
```
---
## Selecting elements

#### prefix()
```swift
var ages = [4, 1, 89, 3, 2, 56]
let prefixAges = ages.prefix(3)
print(prefixAges)
// [4, 1, 89]
```

#### suffix()
```swift
var ages = [4, 1, 89, 3, 2, 56]
let suffixAges = ages.suffix(2)
print(suffixAges)
// [2, 56]
```
---
## // Функции Высшего Порядка

#### map()
```swift
let numbers = [1, 2, 3, 4, 5]
let squares = numbers.map { $0 * $0 }
print(squares)
// [1, 4, 9, 16, 25]
```

#### filter(_ : )
```swift
let numbers = [1, 4, 10, 15]
let even = numbers.filter { $0 % 2 == 0 }
print(even)
// [4, 10]
```

#### mapValues()
```swift
let numbers = [1, 2, 3, 4, 5]
let squares = numbers.map { $0 * $0 }
print(squares)
// [1, 4, 9, 16, 25]
```



#### flatMap()
```swift
let matrix = [[1, 2], [3, 4], [5, 6]]
print(matrix)
for arr in matrix { // каждый элемент matrix представляет массив
    print(arr) // [1, 2] [3, 4], [5, 6]
    for num in arr {
        print(num) // 1 2 3 4 5 6
    }
}
```
```swift
let flattedArr = matrix.flatMap { $0 }
print(flattedArr)
// flatMap возвращает плоский массив - [1, 2, 3, 4, 5, 6]
```

#### compactMap()
```swift
let grades = [nil, 78, nil, 89, 99, 59, nil]
print(grades)
// [nil, Optional(78), nil, Optional(89), Optional(99), Optional(59), nil]

var validGrades = grades.compactMap { $0 }
print(validGrades)
// [78, 89, 99, 59]
```

#### reduce(_ : _ : )
```swift
var validGrades = [78, 89, 99, 59]
var sum = 0
for num in validGrades {
    sum += num
}
print("reduce of validGrades: 78 + 89 + 99 + 59 = \(sum)")
// sum of validGrades: 78 + 89 + 99 + 59 = 325

let sumOfGrades = validGrades.reduce(0, +)
print(sumOfGrades)
// 325
```

#### zip(_ : _ : )
```swift
let collectionOne = [1, 2, 3]
let collectionTwo = [4, 5, 6]
let zipSequence = zip(collectionOne, collectionTwo)

let arr = Array(zipSequence)
print(arr)
// [(1, 4), (2, 5), (3, 6)]

let dict = Dictionary(uniqueKeysWithValues: zipSequence)
print(dict)
// [1: 4, 2: 5, 3: 6]
```

---
## Изменение порядка элементов массива


#### sort()
```swift
var validGrades = [78, 89, 99, 59]
validGrades.sort()
print(validGrades)
// [59, 78, 89, 99]
```


#### sorted() -> [Element]
```swift
var validGrades = [78, 89, 99, 59]
let sortedGrades = validGrades.sorted { $0 > $1 }
// > descending bug -> small 10, 9, 8
// < ascending small -> big 8, 9, 10
print(sortedGrades)
// [99, 89, 78, 59]
```

#### reverse()
```swift
var someNumbers = [1, 2, 3, 4, 5]
someNumbers.reverse()
print(someNumbers)
// [5, 4, 3, 2, 1]
```


#### reversed() -> ReversedCollection<Array<Element>>
```swift
let newReverse: [Int] = someNumbers.reversed()
print(someNumbers) // [5, 4, 3, 2, 1]
print(newReverse) // [1, 2, 3, 4, 5]
```


#### shuffle()
```swift
var someNumbers = [1, 2, 3, 4, 5]
someNumbers.shuffle()
print(someNumbers)
// [4, 3, 1, 2, 5]
```


#### shuffled() -> [Element]
```swift
var someNumbers = [1, 2, 3, 4, 5]
let shuffledElements = someNumbers.shuffled()
print(shuffledElements)
// [1, 4, 3, 2, 5]
```

#### swapAt()
```swift
var arrangedNumbers = [1, 2, 3, 4, 5]
arrangedNumbers.swapAt(0, 1)
print(arrangedNumbers)
// [2, 1, 3, 4, 5]
```

#### partition(by: (Element) -> Bool) -> Int
```swift
var someNumbers = [4, 1, 8, 23, 10, 2, 1, 11, 90, 9]
let pivotIndex = someNumbers.partition { $0 > 9 }
print(someNumbers)
// [4, 1, 8, 1, 2, 10, 23, 11, 90]

print(pivotIndex)
// 5

let valuesLessThan10 = someNumbers[..<pivotIndex]
let valuesGreaterThan9 = someNumbers[pivotIndex...]
print(valuesLessThan10) // [4, 1, 8, 9, 1, 2]
print(valuesGreaterThan9) // [10, 11, 90, 23]
```
---
## Разделение и объединение элементов

#### split()
```swift
let str = "John is iOS Developer"
let strArr = str.split(separator: " ")
print(strArr)
// ["John", "is", "iOS", "Developer"]
```

#### joined()
```swift
1.
let stringArr = ["Bob", "John", "Sally"]
let newStr = stringArr.joined(separator: ", ")
print(newStr)
// Bob, John, Sally
```
```swift
2.
let stringArr = ["Bob", "John", "Sally"]
let newStr = stringArr.joined(separator: " - ")
print(newStr)
// Bob - John - Sally
```