# 基于Java变长数组的“表”

### 表

形如A<sub>0</sub>，A<sub>1</sub>，A<sub>2</sub>，…，A<sub>N-1</sub>的数据结构称为表，表中每一个元素都有一个序号。

### MyArrayList类

```java

public class MyArrayList<AnyType> implements Iterable<AnyType>
{
    /**
     * Construct an empty ArrayList.
     */
    public MyArrayList( )
    {
        doClear( );
    }
    
    /**
     * Returns the number of items in this collection.
     * @return the number of items in this collection.
     */
    public int size( )
    {
        return theSize;
    }
    
    /**
     * Returns true if this collection is empty.
     * @return true if this collection is empty.
     */ 
    public boolean isEmpty( )
    {
        return size( ) == 0;
    }
    
    /**
     * Returns the item at position idx.
     * @param idx the index to search in.
     * @throws ArrayIndexOutOfBoundsException if index is out of range.
     */
    public AnyType get( int idx )
    {
        if( idx < 0 || idx >= size( ) )
            throw new ArrayIndexOutOfBoundsException( "Index " + idx + "; size " + size( ) );
        return theItems[ idx ];    
    }
        
    /**
     * Changes the item at position idx.
     * @param idx the index to change.
     * @param newVal the new value.
     * @return the old value.
     * @throws ArrayIndexOutOfBoundsException if index is out of range.
     */
    public AnyType set( int idx, AnyType newVal )
    {
        if( idx < 0 || idx >= size( ) )
            throw new ArrayIndexOutOfBoundsException( "Index " + idx + "; size " + size( ) );
        AnyType old = theItems[ idx ];    
        theItems[ idx ] = newVal;
        
        return old;    
    }

    @SuppressWarnings("unchecked")
    public void ensureCapacity( int newCapacity )
    {
        if( newCapacity < theSize )
            return;

        AnyType [ ] old = theItems;
        theItems = (AnyType []) new Object[ newCapacity ];
        for( int i = 0; i < size( ); i++ )
            theItems[ i ] = old[ i ];
    }
    
    /**
     * Adds an item to this collection, at the end.
     * @param x any object.
     * @return true.
     */
    public boolean add( AnyType x )
    {
    add( size( ), x );
        return true;            
    }
    
    /**
     * Adds an item to this collection, at the specified index.
     * @param x any object.
     * @return true.
     */
    public void add( int idx, AnyType x )
    {
        if( theItems.length == size( ) )
            ensureCapacity( size( ) * 2 + 1 );

        for( int i = theSize; i > idx; i-- )
            theItems[ i ] = theItems[ i - 1 ];

        theItems[ idx ] = x;
        theSize++;  
    }
      
    /**
     * Removes an item from this collection.
     * @param idx the index of the object.
     * @return the item was removed from the collection.
     */
    public AnyType remove( int idx )
    {
        AnyType removedItem = theItems[ idx ];
        
        for( int i = idx; i < size( ) - 1; i++ )
            theItems[ i ] = theItems[ i + 1 ];
        theSize--;    
        
        return removedItem;
    }
    
    /**
     * Change the size of this collection to zero.
     */
    public void clear( )
    {
        doClear( );
    }
    
    private void doClear( )
    {
        theSize = 0;
        ensureCapacity( DEFAULT_CAPACITY );
    }
    
    /**
     * Obtains an Iterator object used to traverse the collection.
     * @return an iterator positioned prior to the first element.
     */
    public java.util.Iterator<AnyType> iterator( )
    {
        return new ArrayListIterator( );
    }

    /**
     * Returns a String representation of this collection.
     */
    public String toString( )
    {
            StringBuilder sb = new StringBuilder( "[ " );

            for( AnyType x : this )
                sb.append( x + " " );
            sb.append( "]" );

            return new String( sb );
    }
    
    /**
     * This is the implementation of the ArrayListIterator.
     * It maintains a notion of a current position and of
     * course the implicit reference to the MyArrayList.
     */
    private class ArrayListIterator implements java.util.Iterator<AnyType>
    {
        private int current = 0;
        private boolean okToRemove = false;
        
        public boolean hasNext( )
        {
            return current < size( );
        }
        
        
        public AnyType next( )
        {
            if( !hasNext( ) ) 
                throw new java.util.NoSuchElementException( ); 
                  
            okToRemove = true;    
            return theItems[ current++ ];
        }
        
        public void remove( )
        {
            if( !okToRemove )
                throw new IllegalStateException( );
                
            MyArrayList.this.remove( --current );
            okToRemove = false;
        }
    }
    
    private static final int DEFAULT_CAPACITY = 10;
    
    private AnyType [ ] theItems;
    private int theSize;
}

class TestArrayList
{
    public static void main( String [ ] args )
    {
        MyArrayList<Integer> lst = new MyArrayList<>( );

        for( int i = 0; i < 10; i++ )
                lst.add( i );
        for( int i = 20; i < 30; i++ )
                lst.add( 0, i );

        lst.remove( 0 );
        lst.remove( lst.size( ) - 1 );

        System.out.println( lst );
    }
}
```

add方法可能要求增加容量。扩充容量的代价是非常昂贵的，因此，如果容量被扩充，那么，它就要变成原来大小的两倍，以避免不得不再次改变容量，除非大小戏剧性的增加（+1用于大小是0的情况）。

为了把精力集中在编写迭代器类的基本方面，我们不检测可能使得迭代器无效的结构上的修改，也不检测非法的迭代器remove方法。
