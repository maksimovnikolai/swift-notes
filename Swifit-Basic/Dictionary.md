## Dictionary

##### Словарь - это неупорядоченная коллекция элементов, для доступа к значениям которых используются специальные индексы, называемые ключами. Каждый элемент словаря состоит из уникального ключа, указывающего на данный элемент, и значение.
---

#### Cоздание словар
```swift
var someStringDictionary = Dictionary<String, String>()// полная форма записи

var moreStringDictionary: Dictionary<String, String> = [:]

var anotherSomeDictionary = [Int: String]() // сокращенная форма записи словаря

var preferredStyleDictionary: [String: String] = [:] // Рекомендованный способ создания словаря
```

#### Создание словаря с помощью функции Dictionary(dictionaryLiteral:)
```swift
var numbers = Dictionary(dictionaryLiteral: (100, "Сто"), (200, "Двести"), (300, "Триста"))
print(numbers)
// [100: "Сто", 200: "Двести", 300: "Триста"]
```

#### Создание с помощью функции Dictionary(uniqueKeysWithValues:)
```swift
let baseCollection = [(2, 5), (3,6), (1, 4)]
let newDictionary = Dictionary(uniqueKeysWithValues: baseCollection)
print(newDictionary)
// [3: 6, 1: 4, 2: 5]
```
---
### Взаимодействие с элементами словаря

#### получить значение элемента
```swift
var usersAgeDict = ["John": 33, "Sam": 24, "Sara": 25]
var johnAge = usersAgeDict["John"]
print(johnAge)
// Optional(33)
```

#### изменение значения
```swift
usersAgeDict["John"] = 30
print(usersAgeDict)
// ["John": 30, "Sam": 24, "Sara": 25]
```

#### изменение значения с помощью функции updateValue(_ : forKey: )
```swift
var someDict = ["Max": 12, "John": 33, "Elsa": 21]

var oldValueOne = someDict.updateValue(44, forKey: "Max")
// в переменной oldValueOne записано старое измененное значение элемента
print(oldValueOne)
// Optional(12)
```

```swift
// доступ к несуществующему значению вернет nil
var oldValue2 = someDict.updateValue(50, forKey: "Nika")
// в переменной записан nil, так как элемента с таким ключом не существует
print(oldValue2)
// nil
```

####  добавить новый элемент в словарь
```swift
var usersDict = ["John": 12, "Sam": 33] // ["John": 12, "Sam": 33]
usersDict["Sara"] = 40
print(usersDict)
// ["John": 12, "Sam": 33, "Sara": 40]
```

#### удаление элемента (пары ключ-значение) присвоив ключу nil
```swift
var usersAgesDict = ["John": 12, "Sam": 33, "Elsa": 21, "Max": 12]
usersAgesDict["John"] = nil
print(usersAgesDict)
// ["Elsa": 21, "Sam": 33, "Max": 12]
```

#### удаление элемента с помощью метода removeValue(forKey:)
```swift
var usersAges2Dict = ["John": 12, "Sam": 33, "Elsa": 21, "Max": 12]
let valueSam = usersAges2Dict.removeValue(forKey: "Sam")
usersAges2Dict // ["John": 12, "Max": 12, "Elsa": 21]
print(valueSam)
// Optional(33)
```

#### уничтожить все элементы словаря с помощью конструкции [:]
```swift
var birthYears = [1991: ["John", "Ann", "Vasiliy"], 1993: ["Alex", "Boris"]]
birthYears = [:]
print(birthYears)
// [:]
```

#### если обратиться к несуществующему элементу словаря, это не приведет к ошибке. Swift вернет nil
```swift
let newDict = [1: "One", 2: "Two"]
newDict[2] // nil
```
---

### Базовые свойства и методы словарей

#### count
```swift
var someDictionary = ["One": 1, "Two": 2, "Three": 3]
someDictionary.count // 3
```

#### isEmpty
```swift
var emptyDict: [String: String] = [:]
emptyDict.isEmpty // true
```

#### keys - все ключи словаря
```swift
let keysDict = ["one": 1, "two": 2, "three": 3]
let allKeys = keysDict.keys // Dictionary.Keys(["one", "two", "three"])
print(allKeys) 
// ["two", "three", "one"]
```

#### values - все значения словаря
```swift
let valuesDict = ["one": 1, "two": 2, "three": 3]
let allValues = valuesDict.values // Dictionary.Values([3, 1, 2])
print(allValues) 
// [1, 3, 2]
```

#### преобразование Dictionary.Keys(["one", "two", "three"]) в Set
```swift
let keySet = Set(allKeys)
keySet // {"one", "two", "three"}
```

#### преобразование Dictionary.Values([3, 1, 2]) в Array
```swift
let valuesArray = Array(allValues)
valuesArray // [1, 2, 3]
```
---

### Поиск Элементов

#### containts
```swift
let gradesDict = ["Rachel": 90, "Alex": 85, "Kim": 95, "Tom": 92]
let studentExist = gradesDict.contains { (student, grade) -> Bool in
    student == "Kim"
}

if studentExist {
    print("Kim's grade has been entered.")
} else {
    print("Student needs to redo the exam.")
}
// Kim's grade has been entered.
```


#### first; firstIndex((key, value) -> Bool) -> Index?
```swift
let peoplesDict = ["Rachel": 90, "Alex": 85, "Kim": 95, "Tom": 92]
let index = peoplesDict.firstIndex { $0.value == 95 }

if let foundIndex = index {
    print(peoplesDict[foundIndex])
}
// (key: "Kim", value: 95)
```

#### min (((key, value), (key, value)) -> Bool) -> (key, value)?
#### max (((key, value), (key, value)) -> Bool) -> (key, value)?
```swift
let devDict = ["Rachel": 90, "Alex": 85, "Kim": 95, "Tom": 92]
let maxElement = devDict.max { $0.value > $1.value}
print(maxElement)
// Optional((key: "Alex", value: 85))
```
---

### Преобразование словаря

####  mapValues((value) -> T) -> Dictionary<Key, T>
```swift
let numbersDict = [1: 2, 2: 3, 3: 4]
let valuesSquared = numbersDict.mapValues { $0 * $0 }
print(valuesSquared)
// [3: 16, 2: 9, 1: 4]
```

#### map((key, value) -> T) -> [T]
```swift
struct Person {
    let name: String
    let age: Int
}

let personDict = ["Rachel": 34, "Henry": 29, "Gabriel": 45, "Tracy": 41]
let people = personDict.map { Person(name: $0.key, age: $0.value) }

people.forEach { print("\($0.name) is \($0.age) old.")}
// Gabriel is 45 old.
// Henry is 29 old.
// Tracy is 41 old.
// Rachel is 34 old.
```

#### sorted(((key, value), (key, value)) -> Bool) -> [(key, value)]
```swift
let sortedPeople = personDict.sorted { $0.value > $1.value }
print(sortedPeople)
// [(key: "Gabriel", value: 45), (key: "Tracy", value: 41), (key: "Rachel", value: 34), (key: "Henry", value: 29)]

for (name, age) in sortedPeople {
    print("\(name) is \(age)")
}
// Gabriel is 45
// Tracy is 41
// Rachel is 34
// Henry is 29
```

#### shuffled() -> [(key, value)]
```swift
let person2Dict = ["Rachel": 34, "Henry": 29, "Gabriel": 45, "Tracy": 41]
let shuffledElements = person2Dict.shuffled()
print(shuffledElements)
// [(key: "Tracy", value: 41), (key: "Gabriel", value: 45), (key: "Henry", value: 29), (key: "Rachel", value: 34)]
```

#### zip(_ : _ : )
```swift
let nearestStarNames = ["Proxima Centauri", "Alpha Centauri A", "Alpha Centauri B"]
let nearestStarDistances = [4.24, 4.37, 4.37]
let starDistanceDict = Dictionary(uniqueKeysWithValues: zip(nearestStarNames, nearestStarDistances))
print(starDistanceDict)
// ["Proxima Centauri": 4.24, "Alpha Centauri A": 4.37, "Alpha Centauri B": 4.37]
```