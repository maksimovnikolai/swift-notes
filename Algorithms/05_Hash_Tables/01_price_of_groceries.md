
```swift
var book = [String: Double]()
// яблоко стоит 67 центов
book["apple"] = 0.67
// молоко стоит $1.49
book["milk"] = 1.49
book["avacado"] = 1.49
print(book) // => ["avacado": 1.49, "apple": 0.67000000000000004, "milk": 1.49]

// Вопрос: Почему «яблоко» равно 0,67000000000000004 вместо 0,67?
// Ответ: Double не может точно сохранить значение 0,67. Swift использует (как и многие другие языки) двоичные числа с плавающей запятой в соответствии со стандартом IEEE 754.
// Эта тема не связана с алгоритмами, но вы можете поиграть с .description и .debugDescription для обходных путей.
print(book["apple"]?.description ?? "Not exist") // => 0.67
print(book["apple"]?.debugDescription ?? "Not exist") // => 0.67000000000000004
```