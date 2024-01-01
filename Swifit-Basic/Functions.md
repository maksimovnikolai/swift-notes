## Functions

### Функция:

* группирует програмный код в единый контейнер
* имеет совбственное имя, с помощью которого может быть многократно вызвана с возможностью передачи аргументов
* создает отдельную область видимости внутри себя, в результате чего все созданные в теле функции параметры недоступны извне
* может принимать входные параметры
может возвращать значение как результат исполнения сгруппирированного в ней кода
* имеет собственный функциональный тип данных
* может быть записана в параметр (переменную или константу) и в таком виде передана
 
 ### Пример
 ```
    func nameOfFunction() {
        some code
    }
  ```
 

 ```swift
func addingTwoNumbers() {
    let a = 3
    let b = 5
    let c = a + b
    print(c) 
}

let result = addingTwoNumbers
addingTwoNumbers()
result() // 8
 ```
### Функции с возвращаемыми значениями

 ```
    func nameOfFunction() -> Data Type {
        some code
        return some value
    }
 ```
```swift
func addingTwoNumbers() -> Int {
    let a = 3
    let b = 5
    return a + b
}

var result = addingTwoNumbers()
 ```

 ### Функции с параметрами

 ```
    func name(argumentOne parameterOne: Data Type, argumentTwo parameterTwo: Data Type) {
        some code
    }
 ```
* **Функция с параметрами без аргументов**
```swift
func addingTwoNumbers(a: Int, b: Int) -> Int {
    a + b
}

result = addingTwoNumbers(a: 5, b: 8)
print(result) // 13
```

* **Функция с параметрами и аргументами**
```swift
1.
func addingTwoNumbers(number a: Int, andNumber b: Int) -> Int {
    a + b
}

addingTwoNumbers(number: 4, andNumber: 6) // 10
```

```swift
2.
func addingTwoNumbers(_ a: Int, _ b: Int) -> Int {
    a + b
}

addingTwoNumbers(5, 8) // 13
```

```swift
3.
func addingTwoNumbers(_ a: Int, and b: Int) -> Int {
    a + b
}

addingTwoNumbers(5, and: 8) // 13
```
---

### Вариативные параметры

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total = 0.0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}

arithmeticMean(2, 0, 3.15, 0.0, 48) // => Double(10,63)
```

---

### Функции как замыкания

* **Отбор чисел меньше заданного значения**

```swift
func filterLessThanValue(value: Int, numbers: [Int]) -> [Int] {

    var filteredSetOfNumbers: [Int] = []

    for number in numbers {
        if number < value {
            filteredSetOfNumbers.append(number)
        }
    }
    return filteredSetOfNumbers
}

let numbers = [5, 8, 20, 13, 1, 4, 3, 6]

filterLessThanValue(value: 5, numbers: numbers) // [1, 4, 3]
```

* **Отбор чисел больше заданного значения**

```swift
func filterGreaterThanValue(value: Int, numbers: [Int]) -> [Int] {

    var filteredSetOfNumbers: [Int] = []

    for number in numbers {
        if number > value {
            filteredSetOfNumbers.append(number)
        }
    }
    return filteredSetOfNumbers
}

filterGreaterThanValue(value: 5, numbers: numbers) // [8, 20, 13, 6]
```
* **Функция для отбора чисел, относительно заданного значения**

```swift
func filterWithPredicateClosure(value: Int, numbers: [Int], closure: (Int, Int) -> Bool) -> [Int] {
    var filterNumbers: [Int] = []

    for number in numbers {
        if closure(number, value) {
            filterNumbers.append(number)
        }
    }
    return filterNumbers
}
```

* **Функция для отбора чисел меньше заданного значения**
```swift
func lessThenValue(number: Int, value: Int) -> Bool {
    number < value
}

let closure = lessThenValue
lessThenValue(number: 8, value: 5) // false
closure(3, 5) // true
```

* **Функция для отбора чисел больше указанного значения**

```swift
func greaterThenValue(number: Int, value: Int) -> Bool {
    number > value
}
```
* **Отбор чисел меньше указанного значения**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: lessThenValue
)
```
* **Отбор чисел больше указанного значения**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: greaterThenValue
)
```
---

### Замыкающие выражения
* **Замыкающие выражения - это безымянные функции, которые написаны в облегченном синтаксисе, которые могут захватывать значения из окружающего контекста**

```
{ (параметры) -> тип результата in
    тело замыкающего выражения
}
```
#### Использование замыкания в качестве аргумента

* **Отбор чисел меньше указанного значения**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { (number: Int, value: Int) -> Bool in
        return number > value
    }
)
```
* **Вывод типа из контекста**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { (number, value) in
        return number < value
    }
)

filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { (number, value) in
        return number > value
    }
)
```
* **Неявные возвращаемые значения из замыканий с одним выражением**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { (number, value) in number < value }
)

filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { (number, value) in number > value }
)
```

* **Сокращенные имена параметров**

```swift
filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { $0 < $1 }
)

filterWithPredicateClosure(
    value: 5,
    numbers: numbers,
    closure: { $0 > $1 }
)
```
* **Последующее замыкание**

```swift
filterWithPredicateClosure(value: 5, numbers: numbers) { $0 < $1 }
filterWithPredicateClosure(value: 5, numbers: numbers) { $0 > $1 }
```

* **Операторные функции**
```swift
filterWithPredicateClosure(value: 5, numbers: numbers, closure: <)
filterWithPredicateClosure(value: 5, numbers: numbers, closure: >)
```