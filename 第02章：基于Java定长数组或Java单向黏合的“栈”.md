# 基于Java定长数组或Java单向黏合的“栈”

### 栈

插入操作和删除操作只能固定发生在表的一端的表称为栈，该端称为栈顶。插入称为push，删除称为pop，测试称为top。

### MyArrayStack类

另一种实现方法避免了链而且可能是更流行的解决方案。由于模仿ArrayList的add操作，因此相应的实现方法非常简单。与每个栈相关联的操作是theArray和topOfStack，对于空栈它是-1（这就是空栈初始化的做法）。为将某个元素x推入栈中，我们使topOfStack增1然后置theArray[topOfStack]=x。为了弹出栈元素，我们置返回值为theArray[topOfStack]然后使topOfStack减1。

### MyLinkedStack类

栈的第一种实现方法使使用单链表。通过在表的前端插入来实现push，通过删除表前端元素实现pop。
