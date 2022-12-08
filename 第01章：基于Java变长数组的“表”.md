# 基于Java变长数组的“表”

形如A<sub>0</sub>，A<sub>1</sub>，A<sub>2</sub>，…，A<sub>N-1</sub>的数据结构称为表，表中不存在空闲的位置，插入操作和删除操作可以发生在表中任意位置。

```java
public class MyArrayList<AnyType> implements Iterable<AnyType> {
  public static final int DEFAULT_CAPACITY = 10;

  private int theSize;
  private AnyType[] theItems;

  public MyArrayList() {
    doClear();
  }

  public void clear() {
    doClear();
  }

  public void doClear() {
    theSize = 0;
    ensureCapacity(DEFAULT_CAPACITY);
  }

  public int size() {
    return theSize;
  }

  public boolean isEmpty() {
    return size() == 0;
  }

  public void trimToSize() {
    ensureCapacity(size());
  }

  public AnyType get(int idx) {
    if (idx < 0 || idx >= size()) throw new java.lang.ArrayIndexOutOfBoundsException();

    return theItems[idx];
  }

  public AnyType set(int idx, AnyType newVal) {
    if (idx < 0 || idx >= size()) throw new java.lang.ArrayIndexOutOfBoundsException();

    AnyType old = theItems[idx];
    theItems[idx] = newVal;
    return old;
  }

  public void ensureCapacity(int newCapacity) {
    if (newCapacity < theSize) return;

    AnyType[] old = theItems;
    theItems = (AnyType[]) new Object[newCapacity];
    for (int i = 0; i < size(); i++) theItems[i] = old[i];
  }

  public boolean add(AnyType x) {
    add(size(), x);
    return true;
  }

  public void add(int idx, AnyType x) {
    if (theItems.length == size()) ensureCapacity(size() * 2 + 1);
    for (int i = theSize; i > idx; i--) theItems[i] = theItems[i - 1];
    theItems[idx] = x;
    theSize++;
  }

  public AnyType remove(int idx) {
    AnyType removedItem = theItems[idx];
    for (int i = idx; i < size() - 1; i++) theItems[i] = theItems[i + 1];
    theSize--;
    return removedItem;
  }

  public java.util.Iterator<AnyType> iterator() {
    return new ArrayListIterator();
  }

  private class ArrayListIterator implements java.util.Iterator<AnyType> {
    private int current = 0;

    public boolean hasNext() {
      return current < size();
    }

    public AnyType next() {
      if (!hasNext()) throw new java.util.NoSuchElementException();

      return theItems[current++];
    }

    public void remove() {
      MyArrayList.this.remove(--current);
    }
  }
}
```

add方法可能要求增加容量。扩充容量的代价是非常昂贵的，因此，如果容量被扩充，那么，它就要变成原来大小的两倍，以避免不得不再次改变容量，除非大小戏剧性的增加（+1用于大小是0的情况）。

为了把精力集中在编写迭代器类的基本方面，我们不检测可能使得迭代器无效的结构上的修改，也不检测非法的迭代器remove方法。
