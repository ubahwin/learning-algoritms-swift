# Мои знания алгоритмов

## Содержание
* [Граф](#граф)
   - [Дерево](#дерево)
      - [Бинарное дерево](#бинарное-дерево)


## Граф

### Дерево

#### Бинарное дерево

`Бинарное дерево` - дерево с 2-мя ветками: Правое и Левое

##### Структура

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

###### В глубину (DFS)

```swift
var stack: [TreeNode] = [root] // дерево
var current: TreeNode? = root.left

while !stack.isEmpty || current != nil {
    while let node = current {
        stack.append(node)
        current = node.left // проходим по левым веткам
    }
    if let topNode = stack.popLast() {
        // тут можно фиксировать текущую позицию через topNode

        current = topNode.right // переключаемся на на правую и по новой
    }
}
```

###### В ширину (ВFS)

```swift
var queue: [TreeNode] = [root] // дерево

while !queue.isEmpty {
    let node = queue.removeFirst()

    // тут можно фиксировать текущую позицию через node

    if let left = node.left {
        queue.append(left)
    }
    if let right = node.right {
        queue.append(right)
    }
}
```
