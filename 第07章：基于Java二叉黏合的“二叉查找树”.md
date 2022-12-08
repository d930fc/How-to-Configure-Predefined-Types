# 基于Java二叉黏合的“二叉查找树”

我的左儿子和他的任意后代都不如我，我的右儿子和他的任意后代都胜过我，那么我代表二叉查找树中的任意一个节点。

```java
package tree;

public class UnderflowException extends java.lang.RuntimeException {}
```

```java
package tree;

public class BinarySearchTree<AnyType extends Comparable<? super AnyType>> {
  private static class BinaryNode<AnyType> {
    // Constructors
    BinaryNode(AnyType theElement) {
      this(theElement, null, null);
    }

    BinaryNode(AnyType theElement, BinaryNode<AnyType> lt, BinaryNode<AnyType> rt) {
      element = theElement;
      left = lt;
      right = rt;
    }

    AnyType element; // The data in the node
    BinaryNode<AnyType> left; // Left child
    BinaryNode<AnyType> right; // Right child
  }

  private BinaryNode<AnyType> root;

  public BinarySearchTree() {
    root = null;
  }

  public void makeEmpty() {
    root = null;
  }

  public boolean isEmpty() {
    return root == null;
  }

  public boolean contains(AnyType x) {
    return contains(x, root);
  }

  public AnyType findMin() {
    if (isEmpty()) throw new UnderflowException();

    return findMin(root).element;
  }

  public AnyType findMax() {
    if (isEmpty()) throw new UnderflowException();

    return findMax(root).element;
  }

  public void insert(AnyType x) {
    root = insert(x, root);
  }

  public void remove(AnyType x) {
    root = remove(x, root);
  }

  /** Print the tree contents in sorted order. */
  public void printTree() {
    if (isEmpty()) {
      System.out.println("Empty tree");
    } else {
      printTree(root);
    }
  }

  /**
   * Internal method to find an item in a subtree.
   *
   * @param x is item to search for.
   * @param t the node that roots the subtree.
   * @return true if the item is found; false otherwise.
   */
  private boolean contains(AnyType x, BinaryNode<AnyType> t) {
    if (t == null) return false;

    int compareResult = x.compareTo(t.element);

    if (compareResult < 0) {
      return contains(x, t.left);
    } else if (compareResult > 0) {
      return contains(x, t.right);
    } else {
      return true; // Match
    }
  }

  /**
   * Internal method to find the smallest item in a subtree.
   *
   * @param t the node that roots the subtree.
   * @return node containing the smallest item.
   */
  private BinaryNode<AnyType> findMin(BinaryNode<AnyType> t) {
    if (t == null) {
      return null;
    } else if (t.left == null) {
      return t;
    }

    return findMin(t.left);
  }

  /**
   * Internal method to find the largest item in a subtree.
   *
   * @param t the node that roots the subtree.
   * @return node containing the largest item.
   */
  private BinaryNode<AnyType> findMax(BinaryNode<AnyType> t) {
    if (t != null) {
      while (t.right != null) {
        t = t.right;
      }
    }

    return t;
  }

  /**
   * Internal method to insert into a subtree.
   *
   * @param x the item to insert.
   * @param t the node that roots the subtree.
   * @return the new root of the subtree.
   */
  private BinaryNode<AnyType> insert(AnyType x, BinaryNode<AnyType> t) {
    if (t == null) return new BinaryNode<>(x, null, null);

    int compareResult = x.compareTo(t.element);

    if (compareResult < 0) {
      t.left = insert(x, t.left);
    } else if (compareResult > 0) {
      t.right = insert(x, t.right);
    } else {
      ; // Duplicate; do nothing
    }
    return t;
  }

  /**
   * Internal method to remove from a subtree.
   *
   * @param x the item to remove.
   * @param t the node that roots the subtree.
   * @return the new root of the subtree.
   */
  private BinaryNode<AnyType> remove(AnyType x, BinaryNode<AnyType> t) {
    if (t == null) return t; // Item not found; do nothing

    int compareResult = x.compareTo(t.element);

    if (compareResult < 0) {
      t.left = remove(x, t.left);
    } else if (compareResult > 0) {
      t.right = remove(x, t.right);
    } else if (t.left != null && t.right != null) { // Two children
      t.element = findMin(t.right).element;
      t.right = remove(t.element, t.right);
    } else {
      t = (t.left != null) ? t.left : t.right;
    }

    return t;
  }

  /**
   * Internal method to print a subtree in sorted order.
   *
   * @param t the node that roots the subtree.
   */
  private void printTree(BinaryNode<AnyType> t) {
    if (t != null) {
      printTree(t.left);
      System.out.println(t.element);
      printTree(t.right);
    }
  }
}
```

进行插入操作的例程在概念上是简单的。为了将X插入到树T中，你可以像用contains那样沿着树查找。如果找到X，则什么也不用做（或做一些“更新”）。否则，将X插入到遍历的路径上的最后一点上。

重复元的插入可以通过在节点记录中保留一个附加域以指示发生的频率来处理。

正如许多数据结构一样，最困难的操作是remove（删除）。一旦我们发现要被删除的节点，就需要考虑几种可能的情况。

如果节点有一个儿子，则该节点可以在其父节点调整自己的链以绕过该节点后被删除。

复杂的情况是处理具有两个儿子的节点。一般的删除策略是用其右子树的最小的数据（很容易找到）代替该节点的数据并递归地删除那个节点（现在它是空的）。因为右子树中的最小的节点不可能有左儿子，所以第二次remove要容易。
