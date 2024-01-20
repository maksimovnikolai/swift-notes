## Бинарный поиск

```swift
// Бинарный поиск O(log2 n) - это алгоритм; на входе он получает отсортированный список элементов.
// Каждая новая итерация делит массив пополам.
// Если искомый элемент присутствует в списке, то бинарный поиск возвращает ту позицию (индекс),
// в которой он был найден.
// В противном случае бинарный поиск возвращает nil

func binarySearch<T: Comparable>(_ list: [T], item: T) -> Int? {
    
    var lowIndex = 0 // индекс первого элемента списка
    var highIndex = list.count - 1 // индекс последнего элемента списка
    
    while lowIndex <= highIndex {
        let midIndex = (lowIndex + highIndex) / 2 // средний индекс списка
        let guessValue = list[midIndex]
        
        // Возвращает индекс искомого значения
        if guessValue == item {
            return midIndex
        }
        
        if guessValue > item {
            highIndex = midIndex - 1 // меняем указатель
        } else {
            lowIndex = midIndex + 1 // меняем указатель
        }
    }
    return nil // если значение в списке отсутствует, метод вернет nil
}

// Список элементов должен быть ОТСОРТИРОВАННЫМ
let list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(binarySearch(list, item: 3) ?? "Not Found") // => 2
print(binarySearch(list, item: 12) ?? "Not Found") // => Not Found
```