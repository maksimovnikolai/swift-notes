
```swift
// Очереди: это абстрактный тип данных, представляющий собой структуру FIFO (это означает: первый добавленный объект — это первый объект, который будет удален из структуры данных)

// методы очереди:

// постановка в очередь: добавление элемента в конец очереди

// удаление из очереди: удалить элемент из начала очереди

// свойства: count, isEmpty, просмотр

struct Queue<T> {
// реализация очереди с использованием массива
  private var elements = [T]()
  // private var linkedList = LinkedList<T>()
  // private var stack = [Int]()
  
  public var isEmpty: Bool {
    return elements.isEmpty
  }
  
  public var count: Int {
    return elements.count
  }
  
  // возвращает элементы в начале очереди, не удаляя их

  public var peek: T? {
    return elements.first
  }
  
  // add item to elements
  public mutating func enqueue(_ item: T) {
    elements.append(item)
  }
  
  // удалить элемент из передней части массива элементов
  public mutating func dequeue() -> T? {
    guard !isEmpty else { return nil }
    return elements.removeFirst()
  }
}

var queue = Queue<String>()
queue.enqueue("Mel")
queue.enqueue("Kelby")
queue.enqueue("Oscar")

print("\(queue.peek ?? "") is at the front of the line")

queue.dequeue()

print("fellows left in line are \(queue)")

queue.enqueue("Eric")

print("there are \(queue.count) fellows on line")

// iterate through a queue structure

var queueCopy = queue

while let value = queueCopy.dequeue() {
  print("fellow: \(value)")
}

print("there are \(queueCopy.count) fellows left in queueCopy")
```