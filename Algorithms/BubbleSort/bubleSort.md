
```swift
func bubbleSort(_ arr: inout [Int]) { 
  guard arr.count >= 2 else { return }
  for _ in 0..<arr.count { // O(n)
    for j in 1..<arr.count { // O(n)
      if arr[j] < arr[j - 1] {

        let temp = arr[j]
        arr[j] = arr[j - 1]
        arr[j - 1] = temp
      }
    }
  }
}
var list = [10, 2, -8, 4]
bubbleSort(&list)
print(list)
```