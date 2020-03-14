# ArrayList 如何实现动态扩容

> ArrayList 的本质是一个实现了 List 接口的可变数组

1. 每一个 ArrayList 的实例都有一个容量（List aL = new ArrayList()） 。
2. 该容量就是用来存储列表元素的数组大小，它至少大等于 aL.size() 。
3. 每当 aL.add(a) 或者 aL.addAll(aL2) 添加元素时，实例aL的容量会自动增长，即扩容。扩容策略的细节没有指定，除了添加一个元素有恒定的平摊时间成本。
4. 通过确保 ArrayList 实例的容量大小来进行扩容操作。

## 一、ArrayList 源码
### Ⅰ 定义了一些常量，包括初始化时默认容量、内部用于存储数据的数组
```java
private static final int DEFAULT_CAPACITY = 10;

private static final Object[] EMPTY_ELEMENTDATA = {};

private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

transient Object[] elementData; // 内部实际存储的数组

private int size; // ArrayList 实际大小
```

### Ⅱ 构造器
ArrayList 中包含 3 种构造器：`无参构造器`、`参数传入Collection的`、`设置初始容量的`
```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}

public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
    }
}
```

### Ⅲ 数组扩容
#### 3.1 add
```java
public boolean add(E e) {
    /* 1）添加元素之前，传入确保 ArrayList 实例的内部数组容量足够，
       不够时，进行扩容，创建一个更大容量的数组，赋值给 elementData。
       2）增大 size，同时将新元素添加到数组中。
    */
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}
```

#### 3.2 ensureCapacityInternal
```java
private void ensureCapacityInternal(int minCapacity) {
    // minCapacity = 原有size + 新增大小
    // 1）先调用 calculateCapacity 方法计算当前容量
    // 2）确保显性容量
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}
```

#### 3.3 calculateCapacity
```java
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    // 若数组为空，则比较默认容量与传入的minCapacity，返回较大值
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}
```

#### 3.4 ensureExplicitCapacity
```java
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // 数组长度小于要求容量不满足要求时，调用 grow 进行扩容
    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
```

#### 3.5 grow 扩容方法
```java
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    // 1）新容量扩增至数组长度的 1.5 倍
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    // 2）当扩增后的容量仍小于所需容量，则新容量大小=所需容量minCapacity
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}

public boolean addAll(Collection<? extends E> c) {
    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount
    System.arraycopy(a, 0, elementData, size, numNew);
    size += numNew;
    return numNew != 0;
}
```
