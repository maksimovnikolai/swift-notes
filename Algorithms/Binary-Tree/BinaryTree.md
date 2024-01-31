
```swift
// Binary Tree - Abstract data type
// Компонентами дерева являются корневой узел, дочерний узел

// узел называется листом, если у него нет дочерних элементов

// Есть 2 способа обхода двоичного дерева
// 1. Обход в ширину — использует очередь для отслеживания посещенных узлов
// 2. Обход в глубину: по порядку, пост-заказ, предварительный порядок

class BinaryTreeNode<T> {
  var value: T // T is a generic, can hold any data type, e.g Int, String....
  var leftChild: BinaryTreeNode?
  var rightChild: BinaryTreeNode?
  init(_ value: T) {
    self.value = value
  }
}

// queue - is a FIFO data structure
// FIFO - first in first out
struct Queue<T> {
  private var elements = [T]()
  
  public var isEmpty: Bool {
    return elements.isEmpty
  }
  
  public var count: Int {
    return elements.count
  }
  
  // смотрит в начало очереди (строки)
  public var peek: T? {
    return elements.first
  }
  
  // добавить элемент в конец очереди
  public mutating func enqueue(_ item: T) {
    elements.append(item) // O(1)
  }
  
  // удаляет первый элемент из (передней) очереди
  public mutating func dequeue() -> T? {
    guard !elements.isEmpty else { return nil }
    return elements.removeFirst() // O(n) // мы оптимизируем CTA поста
  }
}


/*
          root
            1
           / \
          2   3
         / \
        4   5
*/

let rootNode = BinaryTreeNode<Int>(1)
let twoNode = BinaryTreeNode<Int>(2)
let threeNode = BinaryTreeNode<Int>(3)
let fourNode = BinaryTreeNode<Int>(4)
let fiveNode = BinaryTreeNode<Int>(5)

rootNode.leftChild = twoNode
rootNode.rightChild = threeNode
twoNode.leftChild = fourNode
twoNode.rightChild = fiveNode


/*
          root
            1
           / \
          2   3
         / \
        4   5
*/

extension BinaryTreeNode {
  func breadthFirstTraversal(visit: (BinaryTreeNode) -> ()) {
// использование очереди для отслеживания узлов по мере их посещения
// помещает в очередь всех дочерних элементов этого узла
    var queue = Queue<BinaryTreeNode>()
    visit(self) // self представляет корневой узел
    // visit(self) // в этом методе мы захватываем текущий узел, а не печатаем
    queue.enqueue(self) // добавить корневой узел в очередь
    // мы добавляем корневой узел в очередь, чтобы также посетить его дочерние элементы (левый, правый дочерний элемент)
    
    while let node = queue.dequeue() {
      // проверьте наличие левого дочернего элемента и при необходимости поставьте в очередь
      if let leftChild = node.leftChild { // using optional binding
        visit(leftChild)
        queue.enqueue(leftChild)
      }
      // проверьте правильность дочернего элемента и при необходимости поставьте в очередь
      if let rightChild = node.rightChild {
        visit(rightChild)
        queue.enqueue(rightChild)
      }
    }
  }
}


// протестировать обход в ширину

print("breadth-first traversal")
rootNode.breadthFirstTraversal { (node) in
  print(node.value, terminator: " ") // 1 2 3 4 5
}

print("\n")
```