## Grand Central Dispatch

* **GCD** - это низкоуровневый API, набор функций и инструментов, предоставляемых Apple для создания и управления многопоточностью в приложениях.
---

#### Queue (очередь)
* **Queue** - представляет собой сущность, выполняющую задачи, поступающие на вход, на одном или множестве потоков.
Очередь работает по принципу **FIFO** - первая задача в очереди будет первой направлена на выполнение на потоке.

##### Очереди делятся на два типа:
* **serial** (последовательная) - выполняет задачи последовательно (поочередно). Пока первая задача в очереди не будет выполнена, поток не приступит к выполнению следующей.


```swift
// создание пользовательской последовательной очереди 
let serialQueue = DispatchQueue(label: "serialTest")
```
* **concurrent** (параллельная) - выполняет задачи параллельно. Задачи, поступающие в параллельную очередь могут выполняться одновременно на разных потоках.

```swift
// создание пользовательской параллельной очереди 
    let concurrentQueue = DispatchQueue(label: "concurrentTest", attributes: .concurrent)
```

__Помимо создания собственных очередей, можно получить доступ к Queue из глобального пула системных очередей. Данный пул содержит очереди уже созданные и используемые системой. Для использования такой очереди необходимо вызвать статический метод global()__

```swift
let globalUserInteractiveQueue   = DispatchQueue.global(qos: .userInteractive)
let globalUserInitiatedQueue     = DispatchQueue.global(qos: .userInitiated)
let globalUnspecifiedQueue       = DispatchQueue.global(qos: .unspecified)
let globalDefaultQueue           = DispatchQueue.global(qos: .default)
let globalBackgroundQueue        = DispatchQueue.global(qos: .background)
```
* Все очереди из пула системных очередей являются **Сoncurrent** очередями.
* Глобальные очереди являются системными, а значит их создает сама система и использует для выполнения системных задач. Таким образом данные очереди являются by default загруженными, что сказывается на эффективности выполнения задач. Старайтесь не отдавать таким очередям тяжелые задачи.

__Помимо глобальных очередей, GCD дает возможность обратиться к главной очереди main. Рассмотрим пример обращения к системным очередям:__

```swift
let mainQueue = DispatchQueue.main
```
* Главная очередь main является serial очередью. Для выполнения задач главная очередь использует главный поток.
* Все задачи, связанные с обновлением UI необходимо выполнять на главной очереди или на главном потоке. В ином случае приложение выбросит рантайм ошибку, что приведет к крешу приложения, и вот почему:
На запуске приложения UIApplication инициализирует Main RunLoop на главном потоке, который в свою очередь обрабатывает все UI активности в приложении. Обновление UIView не происходит сразу после изменения этой view. View будет перерисована в конце текущей итерации RunLoop. Этот механизм гарантирует, что приложение успеет обработать все изменения view, и применить их одновременно (перерисовать). Данный процесс называется View Drawing Cycle. Предположим, мы обновляем какой-либо UI элемент в фоновом потоке. Пользователь переводит девайс в альбомную ориентацию и системе необходимо перерисовать все view с учетом новых размеров девайса. В силу того, что каждый тред работает на своем собственном RunLoop, view, которые мы изменяем в фоновом потоке не смогут обновиться одновременно со всеми остальными view. Резюмируя, мы получаем рассинхрон на уровне Main RunLoop и фонового RunLoop. Но даже это не приводит к крашу, по большому счету мы бы просто получили рассинхрон обновления разных view на экране. Проблема лежит глубже и связана напрямую с процессом рендеринга.
---

#### Context Switch
Concurrent очередь достигает возможность параллельно выполнять задачи благодаря множеству потоков, на которых она выполняет эти самые задачи. У всего есть своя цена и concurrent queue не исключение, процесс переключения между потоками является одним из самых ресурсозатратных в многопоточной среде, а имя ему context switch.

**Переключение контекста (англ. context switch)** — в многозадачных ОС и средах - процесс прекращения выполнения процессором одной задачи (процесса, потока, нити) с сохранением всей необходимой информации и состояния, необходимых для последующего продолжения с прерванного места, и восстановления и загрузки состояния задачи, к выполнению которой переходит процессор.

Не смотря на то, что context switch оптимизирован на уровне ОС, он все равно требует больших вычислительных ресурсов. Эти ресурсы в основном тратятся на сохранение контекста текущего процесса (что на самом деле задействовано в переключении контекста, зависит от архитектуры, операционной системы и количества совместно используемых ресурсов). В отличии от concurrent, serial очередь использует единственный поток, таким образом выполнение задач в очереди не приводит к context switch.

---

#### Sync / Async / AsyncAfter / Concurrent Perform

__Существует два основных способа взаимодействия с очередями. Данные способы подразумевают под собой методы, в которые будут передаваться задачи в виде замыканий.__

* **Sync** - метод, позволяющий **выполнять задачи СИНХРОННО** по отношению к вызывающей очереди.

```swift
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

task1()
task2()
serialQueue.sync(execute: task3)
task4()
task5()

// задача task3 выполняется на очереди serialQueue, 
// в то время как основной поток ожидает ее выполнения.

// task1 - task2 - pause - task4 - task5   - основной поток
//                 task3                   - serialQueue
```

* **Async** - метод, позволяющий **выполнять задачи АСИНХРОННО** по отношению к текущей очереди.

```swift
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

task1()
task2()
serialQueue.async(execute: task3)
task4()
task5()

// задача task3 все так же выполняется на очереди serialQueue, 
// но при этом main не дожидается ее выполнения и продолжает свою работу асинхронно.

// task1 - task2 - task4 - task5   
//             task3
```

* **AsyncAfter** - метод, позволяющий отложить асинхронное выполнение задачи на определенное время. Данный метод идентичен async, за исключением аргумента deadline.

```swift
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

task1()
task2()
serialQueue.asyncAfter(deadline: .now() + 3) { task3() }
task4()
task5()

// .now + 3 - откладываем выполнение задачи в очреди на 3 секунды 

// task1 - task2 - task4 - task5   
//         .now(3) task3
```

* **Concurrent Perform** 

```swift
class func concurrentPerform(iterations: Int, execute work: (Int) -> Void)

// iterations - количество раз, которое необходимо выполнить блок
// work - блок для параллельного выполнения. Этот блок не имеет 
// возвращаемого значения и принимает следующий параметр:
//       итерация - текущий индекс итерации
```

Concurrent Perform выполняется параллельно на главной очереди, что при большом количестве итераций, будет ступорить ее работу. 

```swift
DispatchQueue.concurrentPerform(iterations: 20000) { indexOfIteration in
    print("\(indexOfIteration) times") // Main Queue
}
```

Чтобы увести работу на вспомогательные очереди с главной, можно сделать так:

```swift
       // Создать глобальную очередь с приоритетом utility
let globalQueue = DispatchQueue.global(qos: .utility)
        
globalQueue.async {
    // Вызываем concurrentPerform на глобальной очереди
    DispatchQueue.concurrentPerform(iterations: 20000) { indexOfIteration in
    print("\(indexOfIteration) times")
    }
}
```

---

#### Управление очередью

```swift
let inactiveQueue = DispatchQueue(label: "myQueue", attributes: [.concurrent, .initiallyInactive])
        
// Помещаем задачу в очередь
inactiveQueue.async {
   print("Готово!")
}
        
print("Очередь еще на запущена!")
inactiveQueue.activate() // запускаем очередь (очередь становится активной)
print("Очередь АКТИВНА!")
inactiveQueue.suspend() // приостанавливаем очередь (ставим на пузу)
print("Очередь поставлена на паузу")
inactiveQueue.resume() // возобновляем выполнение задач на очереди
        
// Консоль:
//              Очередь еще на запущена!
//              Очередь АКТИВНА!
//              Очередь поставлена на паузу
//              Готово!
```


---

#### Dispatch Work Item
__Фреймворк Dispatch позволяет ставить в очередь на выполнение не только замыкания, но и объекты типа DispatchWorkItem.__

* **DispatchWorkItem** – класс, являющийся абстракцией над выполняемой задачей, который предоставляет ряд полезных методов. Например метод notify, позволяющий уведомить какую-либо очередь о выполнении задачи и следом выполнить какую-либо работу на уведомленной очереди.

__notify__
```swift
// Создаем очередь
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

// Создаем DispatchWorkItem и передаем в него замыкание (задачу)
let workItem = DispatchWorkItem {
    print("DispatchWorkItem task")
}

 // Передаем в метод notify очередь, на которой необходимо будет выполнить задачу,
 // после выполнения данного DispatchWorkItem
workItem.notify(queue: DispatchQueue.main) {
    print("DispatchWorkItem completed")
}

// Выполняем DispatchWorkItem на очереди serialQueue
serialQueue.async(execute: workItem)

// Консоль:
// DispatchWorkItem task
// DispatchWorkItem completed
```

Попробуем реализовать данную логику без использования DispatchWorkItem:

```swift
// Создаем очередь
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

serialQueue.async {
    print("task")

    DispatchQueue.main.async {
        print("completed")
    }
}

// Консоль:
// task
// completed
```
Сравнивая данные примеры видно, что DispatchWorkItem позволяет нам более явно задать логику, без использования вложенных друг в друга замыканий и хаотичных вызовов методов async / sync.


Помимо notify, DispatchWorkItem дает нам возможность отменять задачу с помощью метода cancel. Важно понимать, что задачу можно отменить только в том случае, если она на момент отмены ожидает в очереди. Если поток уже начал выполнять задачу, она не будет отменена. Рассмотрим пример реализации метода cancel:
__cancel__

```swift
// Создаем очередь
let serialQueue = DispatchQueue(label: "serialTest") // последовательная очередь

// Создаем DispatchWorkItem и передаем в него замыкание (задачу)
let workItem = DispatchWorkItem {
    print("DispatchWorkItem task")
}
        
// Усыпляем serialQueue на 1 секунду и сразу возвращаем управление
serialQueue.async {
    print("zzzZZZ")
    sleep(1)
    print("Awaked")
}
        
// Ставим workItem в очередь serialQueue и сразу возвращаем управление
serialQueue.async(execute: workItem)
        
// Отменяем workItem
 workItem.cancel()

 // Пока serialQueue будет спать, мы успеем отменить workItem, тем самым удалив его из очереди serialQueue.

// Консоль:
// zzzZZZ
// Awaked
```
---

#### Semaphore

* **Semaphore** – базовый инструмент синхронизации в GCD. Semaphore позволяет ограничить количество потоков, которые могут единовременно обращаться к очереди. Для этого необходимо передать количество потоков в инициализатор класса DispatchSemaphore:

```swift
let queue = DispatchQueue(label: "concurrentTest", attributes: .concurrent)

let semaphore = DispatchSemaphore(value: 2)
// - выстраивает потоки в очередь
// value(2) - может проходить 2 потока

queue.async {
    semaphore.wait() // декремент (у value отнимается 1) и теперь семафор может принять еще один поток
    sleep(2)
    print("method 1")
    semaphore.signal() // инкремент добавляем к value + 1 - готов принимать еще 2 потока
}

queue.async {
    semaphore.wait() // - 1
    sleep(2)
    print("method 2")
    semaphore.signal() // + 1
}

queue.async {
    semaphore.wait() // - 1
    sleep(2)
    print("method 3")
    semaphore.signal() // + 1
}
```

__пример №2__
```swift
let queue = DispatchQueue(label: "concurrentTest", attributes: .concurrent)

let semaphore = DispatchSemaphore(value: 1)

// concurrentPerform - выполняет метод заданное количество итераций в 
//                     нескольких очередях сразу (и в главной и в параллельных)
// iterations - количество итераций (выполнений метода)
DispatchQueue.concurrentPerform(iterations: 10) { id in
    
    semaphore.wait(timeout: DispatchTime.distantFuture)
    
    sleep(1)
    
    print("Block", String(id))
    
    semaphore.signal()
}
// Консоль:
//      Block 0
//      Block 2
//      Block 3
//      Block 1
//      Block 4
//      Block 5
//      Block 6
//      Block 7
//      Block 8
//      Block 9
```

__пример №3__
```swift
class SemaphoreTest {
    
    private let semaphore = DispatchSemaphore(value: 1)
    
    private var array = [Int]()
    
    private func methodWork(_ id: Int) {
        
        semaphore.wait() // -1
        
        array.append(id)
        print("test array", array.count)
        
        sleep(2)
        
        semaphore.signal() // + 1
    }
    
    func startAllThread() {
        DispatchQueue.global().async {
            self.methodWork(111)
        }
        
        DispatchQueue.global().async {
            self.methodWork(231)
        }
        
        DispatchQueue.global().async {
            self.methodWork(532)
        }
        
        DispatchQueue.global().async {
            self.methodWork(743)
        }
    }
}

let semaphoreTest = SemaphoreTest()

semaphoreTest.startAllThread()
```
Методы signal и wait работают по принципу инкрементирования / декрементирования внутреннего каунтера семафора (аналогично рекурсивному mutex). Это означает, что поток будет разблокирован только тогда, когда каунтер равен значению value, которое мы передаем в инициализатор.

---

#### Dispatch group

* DispatchGroup – объект, позволяющий объединить задачи в группу и синхронизировать их поведение. Группа позволяет присоединить к ней несколько задачь или DispatchWorkItem и запланировать их асинхронное выполнение на одной или нескольких очередях. Когда все задачи в группе будут выполнены, группа уведомит об этом какую-либо очередь и выполнит на ней completion handler. Так же группа позволяет нам дождаться выполнения задач в группе синхронно, без использования уведомления

```swift
// Создаем очередь
let serialQueue = DispatchQueue(label: "serialTest")

// Создаем 2 DispatchWorkItem
let workItem1 = DispatchWorkItem {
    print("workItem1: zzzZZZ")
    sleep(1)
    print("workItem1: awaked")
}

let workItem2 = DispatchWorkItem {
    print("workItem2: zzzZZZ")
    sleep(3)
    print("workItem2: awaked")
}

// Создаем группу
let group = DispatchGroup()

// Добавляем workItem к группе, планируем его выполнение на очереди
// serialQueue и сразу возвращаем управление
serialQueue.async(group: group, execute: workItem1)
serialQueue.async(group: group, execute: workItem2)

// Устанавливаем уведомление.
// Замыкание будет выполнено на главной очереди сразу после того,
// как все задачи в группе будут выполнены
group.notify(queue: .main) {
    print("All tasks on group completed")
}
// Консоль:

// workItem1: zzzZZZ
// workItem1: awaked
// workItem2: zzzZZZ
// workItem2: awaked
// All tasks on group completed
```

Рассмотрим, как добиться такого же поведения, но уже использую enter и leave вместо уведомления:

```swift
// Создаем параллельную очередь
let concurrentQueue = DispatchQueue(label: "serialTest", attributes: .concurrent)

// Создаем группу
let group = DispatchGroup()

// Создаем DispatchWorkItem
let workItem1 = DispatchWorkItem {
    print("workItem1: zzzZZZ")
    sleep(3)
    print("workItem1: awaked")
    
    // Покидаем группу
    group.leave()
}

let workItem2 = DispatchWorkItem {
    print("workItem2: zzzZZZ")
    sleep(3)
    print("workItem2: awaked")
    
    // Покидаем группу
    group.leave()
}

// Входим в группу
group.enter()
// Вызываем
concurrentQueue.async(execute: workItem1)

group.enter()
concurrentQueue.async(execute: workItem2)

// Ожидаем пока все задачи в группе закончат свое выполнение
group.wait()
print("All tasks on group completed")


// Консоль:

// workItem1: zzzZZZ
// workItem1: awaked
// workItem2: zzzZZZ
// workItem2: awaked
// All tasks on group completed
```
В данном случае не нужно добавлять задачи в группу (в аргумент group метода async). Вместо этого мы вызываем метод группы enter, тем самым указывая явно, что задача вошла в группу, а в конце выполнения задачи вызываем метод leave, тем самым явно указывая, что задача завершила свое выполнение. Таким образом очередь в которой был вызван wait (в нашем случае главная очередь), будет ожидать до тех пор, пока все задачи в группе не завершат свое выполнение и не вызовут метод leave.

---

#### Dispatch barrier

Dispatch barrier – механизм синхронизации задач в очереди. Для того, чтобы добавить барьер, необходимо передать соответствующий флаг в метода async:

```swift
// Создаем параллельную очередь
let concurrentQueue = DispatchQueue(label: "concurrentQueue", attributes: .concurrent)

// Помечаем асинхронный вызов флагом .barrier
concurrentQueue.async(flags: .barrier) {
    // ...
}
```

```
Concurrent Queue

            [Task3] |                  |           [Task7]
        [Task2]     |  Barrier Task4   |       [  Task6   ]
    [Task1]         |                  |    [Task5]

----------------------------------------------------------> Time
```

```swift
class SafetyArray<T> {
    private var array = [T]()
    private let queue = DispatchQueue(label: "concurrentTest", attributes: .concurrent)
    
    func append(_ value: T) {
        queue.async(flags: .barrier) {
            self.array.append(value)
        }
    }
    
    var valueArray: [T] {
        var result = [T]()
        queue.sync {
            result = self.array
        }
        return result
    }
}

let safetyArray = SafetyArray<Int>()

DispatchQueue.concurrentPerform(iterations: 10) { index in
    safetyArray.append(index)
}

print("safetyArray", safetyArray.valueArray)
print("safetyArrayCount", safetyArray.valueArray.count)

// Консоль
// safetyArray [1, 0, 2, 3, 4, 5, 6, 7, 8, 9]
// safetyArrayCount 10
```

Когда мы добавляем барьер в параллельную очередь, она откладывает выполнение задачи, помеченной барьером (и все остальные, которые поступят в очередь во время выполнения такой задачи), до тех пор, пока все предыдущие задачи не будут выполнены. После того, как все предудщие задачи будут выполнены, очередь выполнит задачу, помеченную барьером самостоятельно. Как только задача с барьером будет выполнена, очередь вернется к своему нормальному режиму работы.