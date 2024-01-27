
## Сортировка выбором O(n2)

* **Алгоритм "сортировки выбором" поможет перейти к алгоритму "Быстрой сортировки"**

```swift
// Вспомогательная функция для поиска наименьшего элемента массива
func findSmallestIndex<T: Comparable>(_ array: [T]) -> Int {
    var smallest = array[0] // <- для хранения наименьшего значения
    var smallestIndex = 0 // <- для хранения индекса наименьшего значения
    
    for i in 1..<array.count {
        if array[i] < smallest {
            smallest = array[i]
            smallestIndex = i
        }
    }
    return smallestIndex
}
```

* **Selection Sort / Сортировка Выбором**
```swift
func selectionSort<T: Comparable>(_ array: [T]) -> [T] {
    
    // Нам не нужны никакие вычисления, если длина массива равна 1.
    guard array.count > 1 else { return array }
    
    var newArray = [T]()
    
    var mutableArray = array
    
    for _ in 0..<mutableArray.count {
        // Находит наименьший элемент массива и добавляет его в новый массив.
        let smallestIndex = findSmallestIndex(mutableArray)
        newArray.append(mutableArray.remove(at: smallestIndex))
    }
    return newArray
}

print(selectionSort([5, 3, 6, 2, 10])) // => [2, 3, 5, 6, 10]
```