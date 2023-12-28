## Tuples

#### Кортеж - это объект, который группирует значения различных типов в пределах одного составного значения. При этом есть возможность обратиться к каждому элементу кортежа напрямую, по его идентификатору (индеку). У каждого отдельного значения в составе кортежа может быть собственный тип данных, который никак не зависит от других.


#### 
```swift
let programmStatus = (200, "In Work", true)
myProgramStatus // (.0 200, .1 "In Work", 2. true)
```

#### Тип данных кортежа - это фиксированная упорядоченная последовательность имен типов данных элементов кортежа. 
```swift

```


#### Сравнение типов данных различных кортежей
```swift
let tuple1 = (200, "In Work", true)
let tuple2 = (true, "On Work", 200)
print(type(of: tuple1) == type(of: tuple2))
// false
```


#### Объявление кортежа с явно заданным типом 
```swift
let floatStatus: (Float, String, Bool) = (200.2, "In Work", true)
floatStatus // (.0 200.2, .1 "In Work", .2 true)
```


#### Инициализация значений в параметры 
```swift
let (statusCode, statusText, statusConnect) = (200, "In Work", true)
print("Код ответа - \(statusCode)") // Код ответа - 200
print("Текст ответа - \(statusText)") // Текст ответа - In Work
print("Связь с сервером - \(statusConnect)") // Связь с сервером - true
```

#### Доступ к элементам кортежа через индексы
```swift
let programmStatus = (200, "In Work", true)

print(" Код ответа — \(myProgramStatus.0)") // Код ответа - 200
print(" Текст ответа — \(myProgramStatus.1)") // Текст ответа - In Work
print(" Связь с сервером — \(myProgramStatus.2)") // Связь с сервером - true
```


#### Получение только необходимых значений кортежа
```swift
let (statusCode, _, _) = (200, "Error", false)
```

#### Доступ к элементам кортежа через имена
```swift
let statusTuple = (statusCode: 200, statusText: "In Work", statusConnect: true)

print(" Код ответа — \(statusTuple.statusCode)") // Код ответа - 200
print(" Текст ответа — \(statusTuple.statusText)") // Текст ответа - In Work
print(" Связь с сервером — \(statusTuple.statusConnect)") // Связь с сервером - true
```

#### Массовое присвоение значений
```swift
var numberOne = 1
var numberTwo = 2
var numberThree = 3

(numberOne, numberTwo, numberThree) = (4, 5, 6)
```

#### Сравнение кортежей
```swift
(1, "alpha") < (2, "beta") // true
// истина, так как 1 меньше 2.
// вторая пара элементов не учитывается
(4, "beta") < (4, "gamma") // true
// истина, так как "beta" меньше "gamma".
(3.14, "pi") == (3.14, "pi") // true
// истина, так как все соответствующие элементы идентичны»
```


#### Пример работы с tuple
```swift
let cityTemp = [
    "Москва": Int.random(in: -8...0),
    "Санкт-Петербург": Int.random(in: -5...2),
    "Уфа": Int.random(in: -10...0)
]

for (city, temp) in cityTemp {
    print("Температура в городе \(city) составляет \(temp) градусов")
}

let cityList = cityTemp.sorted(by: <)

for cityTemp in cityList {
    print("Температура в городе \(cityTemp.key) составляет \(cityTemp.value) градусов")
}

func getTemp(in city: String) -> (city: String, temp: Int) {
    let temp = Int.random(in: -40...10)
    return (city, temp)
}

let (city, temp) = getTemp(in: "Ufa")
print("Температура в городе \(city) составляет \(temp) градусов")

let tempInCity = getTemp(in: "Moscow")
print("Температура в городе \(tempInCity.city) составляет \(tempInCity.temp) градусов")
```
