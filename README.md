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

```swift
var stack: [TreeNode] = [root]
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
