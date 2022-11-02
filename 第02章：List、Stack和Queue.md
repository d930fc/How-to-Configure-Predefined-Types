# List、Stack和Queue

### 结构

##### List ADT的结构

形如A<sub>0</sub>，A<sub>1</sub>，A<sub>2</sub>，……，A<sub>N-1</sub>的ADT称为List ADT。

##### Stack ADT的结构

Stack ADT是一种操作受限制的List ADT，这种List ADT的一端称为bottom，另一端称为top，只在top进行插入和删除。

##### Queue ADT的结构

Queue ADT是一种操作受限制的List ADT，这种List ADT的一端称为rear，另一端称为front，只在front进行删除，只在rear进行插入。

### 操作

##### List ADT的操作

- `printList`：略。
- `makeEmpty`：略。
- `find`：略。
- `findKth`：略。
- `insert`：在指定的位置上增加一个元素。
- `remove`：删除指定位置上的元素。
- `遍历`：略。
- `迭代`：略。

##### Stack ADT的操作

- `push`：在top增加一个元素。
- `pop`：在top删除一个元素。

##### Queue ADT的操作

- `enqueue`：在rear增加一个元素。
- `dequeue`：在front删除一个元素。

### 应用

##### List ADT的应用

- 略

##### Stack ADT的应用

- 平衡符号
- 后缀表达式
- 中缀到后缀的转换
- 方法调用

##### Queue ADT的应用

- 排队
