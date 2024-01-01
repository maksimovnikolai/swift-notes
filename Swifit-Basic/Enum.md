## Перечисления


```swift
var someDay = "Monday"

func setupAlarm(for day: String) {
    if day == "Monday" || day == "Tuesday" {
        print("Today is \(day). The alarm is set at 8 am")
    } else {
        print("Today is \(day). Alarm not set")
    }
}

setupAlarm(for: someDay) // Today is Monday. The alarm is set at 8 am

someDay = "tuesday"

setupAlarm(for: someDay) // Today is tuesday. Alarm not set
``` 

``` swift 
enum Weekday {
    case monday
    case tuesday
    case wednsday
    case thursday
    case friday
    case saturday
    case sunday
}

var weekday = Weekday.monday // monday
weekday = .tuesday // tuesday
``` 

### Использование инструкции Switch
```swift
func setupAlarm(for weekday: Weekday) {
    switch weekday {
    case .monday, .tuesday:
        print("To set the alarm at 8 am")
    case .wednsday:
        print("To set the alarm at 7 am")
    case .thursday:
        print("To set the alarm at 7:30 am")
    case .friday:
        print("Yay! TGIF!")
    default:
        print("Weekend. Alarm not set")
    }
}
``` 

### Исходные значения
```swift
enum Country: String {
    case usa = "USA"
    case russia = "Russia"
    case china
}

var country = Country.china

print("case name: \(country)")
print("case value: \(country.rawValue)")

enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}

let earth = Planet.earth
print("Earth is the \(earth.rawValue) planet from the sum")
```

### Инициализация
```swift
if let possiblePlanet = Planet(rawValue: 7) {
    print("The seventh planet is \(possiblePlanet)")
}
```

### Связанные значения (ассоциированные параметры)
```swift
enum WeekdayV2 {
    case workday(message: String, time: Int)
    case weekend(message: String)
}

var anyDay = WeekdayV2.workday(message: "Set alarm to", time: 8)

func setAlarm(for weekday: WeekdayV2) {
    switch weekday {
    case let .workday(message, time):
        print("\(message) \(time) am")
    case let .weekend(message):
        print(message)
    }
}

setAlarm(for: anyDay)

anyDay = .weekend(message: "Alarm not set")

setAlarm(for: anyDay)
```