## String and Character's

### Строка — это коллекция символов.

#### Строковый литерал

```swift
let str = "Welcome to Swift fundamentals"
print(str)
// Welcome to Swift fundamentals
```

#### Многострочные строковые литералы
```swift
let multiLine = """
This is the first line
This is the second line
This is the third line
"""
print(multiLine)
// This is the first line
// This is the second line
// This is the third line
```

#### Специальные символы в строковых литералах \n
* см. документацию для других специальных (escape) символов
```swift
let specialStr = "Swift is cool.\nSwift was introduced in 2014"
print(specialStr)
// Swift is cool.
// Swift was introduced in 2014
```

#### Инициализация пустой строки с использованием литерала или синтаксиса инициализации
```swift
var anotherStringLiteral = "" // использование строкового литерала
//anotherStringLiteral = "Apple"

if anotherStringLiteral.isEmpty { // true or false
    print("anotherStringLiteral is empty")
}

var emptyStringUsingInitializationSyntax = String()
```

#### Строки являются «value type», передаются копированием.
```swift
var original = "original string"
var copy = original
print(original) // original string
print(copy) // original string

copy = "made a mistake"
print(original) // original string
print(copy) // made a mistake
```

####  Доступ к отдельным элементам (символам) строки с помощью цикла for
* цикл for работает, проходя каждый элемент заданной коллекции.
```swift
let iterateStr = "Swift allows developers to write apps for Apple devices. 🥳"
for char in iterateStr {
    print(char) // char является типом Character или String.index
}
```

####  Использование аннотации типа при определении символа
* тип данных Character позволяет хранить строковый литерал длинной в один символ
```swift
let emoji: Character = "🥳"
```

#### Конкатенация строк
* При **конкатенации** происходитт объединение нескольких строковых значений в одно с помощью оператора сложения (+) или append()
```swift
var str1 = "Hello"
var str2 = "World"
str1.append(str2)
print(str1) 
// HelloWorld
```

```swift
str1 += str2
print(str1) 
// HelloWorldWorld
```

#### Интерполяция строк 
* **интерполяция** строк — объедиение строковых литералов, переменных, констант и выражений в едином строковом литерале
```swift
print("Adding values together \(4 + 10)")
//Adding values together 14
```

#### Unicode в строковых типах данных
* в строковых литералах для определения символов можно использовать так называемые **юникод-скаляры** - специальные конструкции, состоящие из набора символов **\u{}** и заключенной между фигурными скобками кодовой точки символа в шестнадцатеричной форме 
```swift
print("unicode character is \u{1F425}") // unicode character is 🐥
print("char is 🐥") // char is 🐥
```

#### Подсчет символов
```swift
let message = "Hello"
print("There are \(message.count) in the message") 
// There are 35 in the message
```

#### Доступ и изменение строки
```swift
var statement = "JavaScript is the language of the web"
```

#### startIndex
```swift
let firstChar = statement[statement.startIndex]
print(firstChar) 
// J
```

#### // endIndex
```swift
let lastIndex = statement.index(before: statement.endIndex)
let lastChar = statement[lastIndex] // String.Index not type Int
print(lastChar) 
// b
```

#### offset // смещение
```swift
let offsetIndex = statement.index(statement.endIndex, offsetBy: -3)
let substring = statement[offsetIndex...lastIndex]
print(substring) 
// web
```

####  Вставка и удаление
#### insert()
```swift
statement.insert("*", at: lastIndex)
print(statement) 
// JavaScript is the language of the we*b
```

#### remove(at: )
```swift
statement.remove(at: lastIndex)
print(statement) 
// JavaScript is the language of the web
```

#### Substrings / Подстроки
#### Сравнение строк
```swift
let name1 = "John"
let name2 = "john"

if name1 == name2 {
    print("names are equal")
} else {
    print("names are NOT equal") 
}
// names are NOT equal
```

```swift
if name1.lowercased() == name2.lowercased() {
    print("names are equal") 
} else {
    print("names are NOT equal") 
}
// names are equal
```

#### hasPrefix()
```swift
let firstName = "John"

if firstName.hasPrefix("Jo") {
    print("Hello, world!") 
}
// Hello, world!
```

#### hasSuffix()
```swift
if verb.hasSuffix("ing") {
    print("world ends in \"ing\"") 
}
// world ends in "ing"
```