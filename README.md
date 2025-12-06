# Мои знания алгоритмов и структур данных

## Содержание

- [Мои знания алгоритмов и структур данных](#мои-знания-алгоритмов-и-структур-данных)
  - [Содержание](#содержание)
  - [Templates](#templates)
    - [Input from command line](#input-from-command-line)
      - [String](#string)
      - [Array](#array)
    - [File input / output](#file-input--output)
        - [Input](#input)
        - [Output](#output)
  - [Array](#array-1)
    - [Binary search](#binary-search)
    - [Two pointers](#two-pointers)
  - [Stack](#stack)
    - [Monotic Stack](#monotic-stack)
  - [Queue](#queue)
    - [Реализации](#реализации)
      - [Через массив](#через-массив)
      - [Через два стека](#через-два-стека)
  - [Linked list](#linked-list)
    - [Односвязный](#односвязный)
      - [Структура](#структура)
      - [Алгоритм прохода](#алгоритм-прохода)
      - [Fast slow](#fast-slow)
    - [Двусвязный](#двусвязный)
      - [Структура](#структура-1)
  - [Graph](#graph)
    - [Tree](#tree)
      - [Binary tree](#binary-tree)
        - [Структура](#структура-2)
        - [Алгоритм прохода по бинарному дереву](#алгоритм-прохода-по-бинарному-дереву)
          - [DFS](#dfs)
          - [ВFS](#вfs)
          - [ВFS послойно](#вfs-послойно)
  - [Priority Queue](#priority-queue)
    - [Heap](#heap)
      - [Binary Heap](#binary-heap)

## Templates

### Input from command line

#### String

```swift
CLI: hello
let input = readLine()!

CLI: 412
let input = Int(readLine()!)!
```

#### Array

```swift
CLI: fas rqw q
let input = readLine()!.split(separator: " ")

CLI: 12 31 5 65
let input = readLine()!.split(separator: " ").map { Int($0)! }
```

### File input / output

##### Input

Current file **input.txt**: `32 12 678`

```swift
import Foundation

let currentDirectory = FileManager.default.currentDirectoryPath
let filePath = URL(fileURLWithPath: currentDirectory).appendingPathComponent("input.txt")
let fileContent = try String(contentsOf: filePath)
let numbers = fileContent.components(separatedBy: CharacterSet.whitespacesAndNewlines).compactMap { Int($0) }
```

##### Output

```swift
let example = 978

let outputFilePath = URL(fileURLWithPath: currentDirectory).appendingPathComponent("output.txt")
try "\(example)".write(to: outputFilePath, atomically: false, encoding: .utf8)
```

## Array

### Binary search

Поиск в **отсортированном** массиве числа. Грубо говоря, используется метод [двух поинтеров](#two-pointers)

```swift
let target = 8
let nums = [ -1, 4, 6, 8, 13, 85, 359]

var left = 0, right = nums.count - 1
while left <= right {
    let mid = left + (right - left) / 2

    if arr[mid] == target {
        break
    }

    if arr[mid] < target {
        left = mid + 1
    } else {
        right = mid - 1
    }
}

print(mid) // 3
```

[Вики](https://ru.wikipedia.org/wiki/Двоичный_поиск)

### Two pointers

Метод двух бегунков, обычно это `i` и `j` или `fast` и `slow` или `left` и `right`, которые бегают по массиву по индексам или другому контейнеру.

```swift
var left = 0, right = nums.count - 1 

while left < right {
    /*
    тут можно щупать ячейки, подбираясь к центру

    [ 1, 4, 6, 8 ]
      ^        ^
     left    right
    */

    left += 1
    right -= 1
}
```

## Stack

Stack – массив, реализующий интерфейс:

```c++
append(element)
popLast() -> Element?
```

### Monotic Stack

Монотонный стек – стек, в котором все элементы выстроены по порядку (монотонны). 

В случае же если порядок нарушается, обычно элементы удаляют и начинают заново заполнение стека. Часто используются для поиска ближайшего меньшего / большего элемента слева / справа:

```swift
// стек по индексам
var stack = [Int]() 

for i in {range} {
    while let last = stack.last, {condition} {
        // тут можно обработать last

        stack.removeLast()
    }
    stack.append(i)
}
```

Поиск ближайшего:
```swift
range:
    // справа
    0..<n
    // слева
    (0..<n).reversed()

condition:
    // большего
    nums[i] >= nums[last]
    // меньшего
    nums[i] <= nums[last]
```

## Queue

Queue - структура, реализующая интерфейс:

```c++
append(element)
popFirst() -> Element? // <-- отличие от стека
```

### Реализации

#### Через массив

```swift
var queue = [Element]()
```

Сложность `popFirst` равна **O(n)** из-за необходимости сдвигать остальные элементы. Однако в большинстве кейсов этого достаточно

#### Через два стека

```swift
class Queue<Element> {
    private var inStack: [Element]
    private var outStack = [Element]()

    init(_ array: [Element]) {
        inStack = array
    }

    func append(_ element: Element) {
        inStack.append(element)
    }

    func popFirst() -> Element? {
        if outStack.isEmpty {
            while !inStack.isEmpty {
                outStack.append(inStack.removeLast())
            }
        }
        return outStack.popLast()
    }
}
```

Сложность удаления **O(1)**, кроме случая, когда `inStack` пустой

## Linked list

`Связный список` – цепочка узлов, в которой узел имеет только Данные и Указатель на другой(-ие) узел

### Односвязный

`Односвязный список` – связный список, который имеет один указатель

#### Структура
Для целочисленных (`Int`)
```swift
public class ListNode {
    public var val: Int
    public var next: ListNode?

    public init(_ val: Int, _ next: ListNode?) { 
        self.val = val
        self.next = next
    }
}
```

#### Алгоритм прохода

```swift
let list = ListNode(...)

var current = list

while current != nil {
    // фиксируем по current.val
    current = current.next
}
```

#### Fast slow
Пока один движется как `current` из прошлого примера (`slow`), другой прыгает через один узел (`fast`)
```swift
var slow: ListNode = list
var fast: ListNode = list

while fast?.next != nil {
    slow = slow?.next
    fast = fast?.next?.next
}
```

### Двусвязный

`Двусвязный список` – связный список, который имеет один указатель

#### Структура
Для целочисленных (`Int`)
```swift
public class ListNode {
    public var val: Int
    public var next: ListNode?
    public var previous: ListNode?

    public init(_ val: Int, next: ListNode?, previous: ListNode?) { 
        self.val = val
        self.next = next
        self.previous = previous
    }
}
```

## Graph

### Tree

`Дерево` – граф без циклов

#### Binary tree

`Бинарное дерево` – дерево с 2-мя ветками: Правое и Левое

##### Структура
Для целочисленных (`Int`)
```swift
public class TreeNode {
    public var val: Int
    public var left: TreeNode?
    public var right: TreeNode?
    public init(val: Int, left: TreeNode?, right: TreeNode?) {
        self.val = val
        self.left = left
        self.right = right
    }
}
```

##### Алгоритм прохода по бинарному дереву

###### DFS

```swift
var stack: [TreeNode] = [root] // дерево

while !stack.isEmpty {
    let node = stack.popLast()

    // тут можно фиксировать текущую позицию через node

    if let left = node?.left {
        stack.append(left)
    }
    if let right = node?.right {
        stack.append(right)
    }
}
```

###### ВFS

```swift
var queue = Queue<TreeNode>([root])

while !queue.isEmpty {
    let node = queue.popFirst()

    // тут можно фиксировать текущую позицию через node

    if let left = node.left {
        queue.append(left)
    }
    if let right = node.right {
        queue.append(right)
    }
}
```
###### ВFS послойно

Слой – уровень глубины дерева

```swift
var nodes: [TreeNode?] = [root]

while !queue.isEmpty {
    var childs = [TreeNode?]()        

    for node in nodes {
        if let left = node?.left {
            childs.append(left)
        }
        if let right = node?.right {
            childs.append(right)
        }
    }

    // фиксируем слой

    nodes = childs
}
```

## Priority Queue

`Priority Queue` (очередь с приоритетом) – абстрактная структура данных (ADT), которая определяет интерфейс:

```c++
push(element)

// Работа с наибольшим / наименьшим приоритетом:
pop()
peek()
```

### Heap

`Heap` – реализация `Priority Queue`

#### Binary Heap

Работает с бинарным деревом. 

Реализация с бинарным деревом через массив:

```swift
class Heap<Element> {

    private(set) var elements: [Element]
    private let areSorted: (Element, Element) -> Bool

    init(
        elements: [Element] = [], 
        areSorted: @escaping (Element, Element) -> Bool
    ) {
        self.elements = elements
        self.areSorted = areSorted

        if !elements.isEmpty {
            for i in (0...elements.count / 2 - 1).reversed() {
                siftDown(from: i)
            }
        }
    }

    // MARK: Interface

    func push(_ value: Element) {
        elements.append(value)
        siftUp(from: elements.count - 1)
    }

    func peek() -> Element? { elements.first }

    func pop() -> Element? {
        guard !elements.isEmpty else { return nil }

        guard elements.count != 1 else {
            return elements.removeLast()
        }

        elements.swapAt(0, elements.count - 1)
        let item = elements.removeLast()
        siftDown(from: 0)
        return item
    }

    // MARK: Helpers

    private func siftUp(from index: Int) {
        var child = index
        var parent = self.parent(of: child)

        while child > 0 && areSorted(elements[child], elements[parent]) {
            elements.swapAt(child, parent)
            child = parent
            parent = self.parent(of: child)
        }
    }

    private func siftDown(from index: Int) {
        var parent = index

        while true {
            let left = leftChild(of: parent)
            let right = rightChild(of: parent)

            var candidate = parent

            if left < elements.count && areSorted(elements[left], elements[candidate]) {
                candidate = left
            }
            if right < elements.count && areSorted(elements[right], elements[candidate]) {
                candidate = right
            }
            if candidate == parent { return }

            elements.swapAt(parent, candidate)
            parent = candidate
        }
    }

    private func parent(of index: Int) -> Int { 
        (index - 1) / 2
    }

    private func leftChild(of index: Int) -> Int {
        2 * index + 1
    }

    private func rightChild(of index: Int) -> Int {
        2 * index + 2
    }
}
```
