## Базовые операторы

#### Оператор присваивания  =

```swift
var a = 34
print("значение переменной а: \(a)") // 34
a = 10 // переназначение с помощью оператора присваивания
print("новое значение переменной: \(a)") // 10
```

#### Арифметические операторы +, -, *, /
```swift
var addition = 56 + 4
print("результат сложения: \(subtraction)) 
// результат сложения: 60

var subtraction = 65 - 5
print("результат вычитания: \(subtraction)")
// результат вычитания: 60

var multiplication = 10 * 10
print("результат умножения: \(multiplication)")
// результат умножения: 100

var division = 20.8 / 5
print("результат деления: \(division)")
// результат деления: 4.16 <- Double
```

#### Оператор остатка от деления %
```swift
var remainder = 9 % 4
print("остаток: \(remainder)")
// остаток: 1
```

#### Унарный оператор минус и унарный плюс -a, +a
```swift
var positiveNumber = +10
print("положительное число: \(positiveNumber + positiveNumber)") // 10 + 10 = 20
// положительное число: 20

var negativeNumber = -10
print("результат сложения отрицательного и положительного числа равен: \(positiveNumber + negativeNumber)")
// 10 + -10 => 10 - 10 = 0
// результат сложения отрицательного и положительного числа равен: 0
```

#### Операторы составного присваивания +=, -=
```swift
var compoundNumber = 20
compoundNumber = compoundNumber + 1
print("compound number is \(compoundNumber)")
// compound number is 21

compoundNumber += 1
print("compound number is \(compoundNumber)")
// compound number is 22

compoundNumber -= 1 // 21 -- decrement by 1 (subtract 1)
print("compound number is \(compoundNumber)")
// compound number is 21

compoundNumber += 100 // increment by 100 (add)
```

#### Операторы сравнения ==, >, <, >=, <=, !=
```swift
var x = 5
var y = 10
print(x == x) // true
print(y > x) // true
print(y < x) // false
print(x >= x) // true
print(x != y) // true
```

#### Тернарный оператор a ? :
```swift
var resultOfTernaryOperation = 100

if resultOfTernaryOperation > 80 {
    print("число больше, чем 80")
} else { // else if false
    print("число меньше, чем 80")
}
// число больше, чем 80

resultOfTernaryOperation = 75

let message = resultOfTernaryOperation > 80 ? "число больше, чем 80" : "число меньше, чем 80"
print(message)
// число меньше, чем 80
```

#### Nil-coalescing operator ?? (Оператор объединения с nil)
```swift
let inputNumberFromUser = "fifty six"
let convertedNumber = Int(inputNumberFromUser) // optional Int

let validNumber = convertedNumber ?? 0

print("user entered \(validNumber) number")
// user entered 0 number
```

#### Закрытый диапазон ...
```swift
for number in 0...10 {
    print(number)
}
// 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

#### Полуоткрытый диапазон ..<
```swift
for number in 0..<10 {
    print(number)
}
// 1, 2, 3, 4, 5, 6, 7, 8, 9
```

#### Односторонний диапазонe [...2], [2...]
```swift
let arr = [1, 2, 3, 4, 5, 6, 7]
print(arr) // [1, 2, 3, 4, 5, 6, 7]
print(arr[...2]) // [1, 2, 3]
print(arr[4...]) // [5, 6, 7]
```

#### Логические операторы &&, ||, !a
```swift
if (5 < 7) && (4 > 2) { // True && True return True
    print("true statement")
} else {
    print("statement is false")
}
// true statement

if (5 < 7) || (4 < 2) { // True || False return True
    print("true statement")
} else {
    print("statement is false")
}
// true statement

if (5 < 7) && (4 < 2) { // True && False return False
    print("true statement")
} else {
    print("statement is false")
}
// statement is false
```

#### Оператор НЕ (!)
```swift
var isFriday = true

if !isFriday { // !true вернет false, !false вернет true
    print("TGIF!")
} else {
    print("Just another day!")
}
// Just another day!
```