# Java集合

Java集合可以分两类

* 一类是Collection接口实现的单列集合比如，ArrayList,LinkedList,HashSet,LinkedHashSet,TreeSet。
* 一类是Map接口实现的双列集合，



## Collection

Collection 是单列集合接口的父接口，该接口并没有什么特点，只是规定一些方法。

常用方法

`Collection` 是 Java 集合框架中的一个核心接口，位于 `java.util` 包中。它定义了一组操作集合的通用方法，是 `List`、`Set` 和 `Queue` 等集合接口的父接口。以下是 `Collection` 接口中常用的 API：

---

### 1. **添加元素**
- **`boolean add(E e)`**  
  向集合中添加一个元素。  
  示例：
  
  ```java
  Collection<String> list = new ArrayList<>();
  list.add("Java");
  ```
```
  
- **`boolean addAll(Collection<? extends E> c)`**  
  将另一个集合中的所有元素添加到当前集合中。  
  示例：
  
  ```java
  Collection<String> list1 = new ArrayList<>();
  list1.add("Java");
  Collection<String> list2 = new ArrayList<>();
  list2.add("Python");
  list1.addAll(list2); // list1 现在包含 ["Java", "Python"]
```

---

### 2. **删除元素**
- **`boolean remove(Object o)`**  
  从集合中删除指定元素。  
  示例：
  
  ```java
  list.remove("Java");
  ```
```
  
  若集合中有多个相同的元素，只会删除第一个，要想全部删除可以使用removeAll，或者遍历去删。
  
- **`boolean removeAll(Collection<?> c)`**  
  删除当前集合中与指定集合相同的所有元素。  
  示例：
  
  ```java
  Collection<String> list1 = new ArrayList<>();
  list1.add("Java");
  list1.add("Python");
  Collection<String> list2 = new ArrayList<>();
  list2.add("Java");
  list1.removeAll(list2); // list1 现在只包含 ["Python"]
```

- **`void clear()`**  
  清空集合中的所有元素。  
  示例：
  
  ```java
  list.clear();
  ```

---

### 3. **查询元素**
- **`boolean contains(Object o)`**  
  判断集合中是否包含指定元素。  
  示例：
  
  ```java
  boolean hasJava = list.contains("Java");
  ```
```
  
- **`boolean containsAll(Collection<?> c)`**  
  判断当前集合是否包含指定集合中的所有元素。  
  示例：
  
  ```java
  Collection<String> list1 = new ArrayList<>();
  list1.add("Java");
  list1.add("Python");
  Collection<String> list2 = new ArrayList<>();
  list2.add("Java");
  boolean containsAll = list1.containsAll(list2); // true
```

- **`boolean isEmpty()`**  
  判断集合是否为空。  
  示例：
  
  ```java
  boolean isEmpty = list.isEmpty();
  ```
```
  
- **`int size()`**  
  返回集合中元素的数量。  
  示例：
  
  ```java
  int size = list.size();
```

---

### 4. **遍历集合**
- **`Iterator<E> iterator()`**  
  返回一个迭代器，用于遍历集合中的元素。  
  示例：
  
  ```java
  Iterator<String> iterator = list.iterator();
  while (iterator.hasNext()) {
      String element = iterator.next();
      System.out.println(element);
  }
  ```
```
  
- **`forEach(Consumer<? super E> action)`**  
  使用 Lambda 表达式遍历集合。  
  示例：
  
  ```java
  list.forEach(element -> System.out.println(element));
```

- 增强for循环

```java
Collection<String> list = new ArrayList<>();
list.add("Java");
list.add("Python");
list.add("C++");

for (String language : list) {
    System.out.println(language);
}
```



---

### 5. **集合操作**
- **`boolean retainAll(Collection<?> c)`**  
  保留当前集合中与指定集合相同的元素，删除其他元素。  
  示例：
  
  ```java
  Collection<String> list1 = new ArrayList<>();
  list1.add("Java");
  list1.add("Python");
  Collection<String> list2 = new ArrayList<>();
  list2.add("Java");
  list1.retainAll(list2); // list1 现在只包含 ["Java"]
  ```
  
- **`Object[] toArray()`**  
  将集合转换为数组。  
  示例：
```java

  Object[] array = list.toArray();
```

- **`<T> T[] toArray(T[] a)`**  
  将集合转换为指定类型的数组。  
  示例：
  
  ```java
  String[] array = list.toArray(new String[0]);
  ```

---

### 6. **其他方法**
- **`boolean equals(Object o)`**  
  比较两个集合是否相等。  
  示例：
  ```java
  boolean isEqual = list1.equals(list2);
  ```

- **`int hashCode()`**  
  返回集合的哈希值。  
  示例：
  
  ```java
  int hashCode = list.hashCode();
  ```

---

### 总结
`Collection` 接口提供了丰富的 API，支持对集合的增删改查和遍历操作。常用的实现类包括 `ArrayList`、`HashSet` 和 `LinkedList` 等。掌握这些 API 是使用 Java 集合框架的基础。

## List

List集合特点：添加的元素是**有序的，可以重复，有索引**。

ArrayList 和LinkedList 集合的特点都是有序，可重复，有索引，为啥还有两种呢？因为底层实现不一样

`List` 接口继承自 `Collection` 接口，并在其基础上增加了一些 **特有且常用** 的 API，主要与 **索引操作** 相关。以下是 `List` 相对于 `Collection` 特有的常用 API：

---

### 1. **索引操作**
- **`E get(int index)`**  
  返回列表中指定索引位置的元素。  
  示例：
  
  ```java
  List<String> list = new ArrayList<>();
  list.add("Java");
  String element = list.get(0); // 返回 "Java"
  ```
```
  
- **`E set(int index, E element)`**  
  替换指定索引位置的元素，并返回被替换的元素。  
  示例：
  
  ```java
  String oldElement = list.set(0, "Python"); // 将索引 0 处的元素替换为 "Python"
```

- **`void add(int index, E element)`**  
  在指定索引位置插入一个元素。  
  示例：
  
  ```java
  list.add(1, "C++"); // 在索引 1 处插入 "C++"
  ```
```
  
- **`E remove(int index)`**  
  删除指定索引位置的元素，并返回被删除的元素。  
  示例：
  
  ```java
  String removedElement = list.remove(0); // 删除索引 0 处的元素
```

---

### 2. **查找元素**
- **`int indexOf(Object o)`**  
  返回指定元素在列表中 **第一次出现** 的索引，如果不存在则返回 -1。  
  示例：
  
  ```java
  int index = list.indexOf("Java"); // 返回 "Java" 的索引
  ```
```
  
- **`int lastIndexOf(Object o)`**  
  返回指定元素在列表中 **最后一次出现** 的索引，如果不存在则返回 -1。  
  示例：
  
  ```java
  int lastIndex = list.lastIndexOf("Java"); // 返回 "Java" 最后一次出现的索引
```

---

### 3. **子列表操作**
- **`List<E> subList(int fromIndex, int toIndex)`**  
  返回列表中从 `fromIndex`（包含）到 `toIndex`（不包含）的子列表。  
  示例：
  
  ```java
  List<String> subList = list.subList(1, 3); // 获取索引 1 到 2 的子列表
  ```

---

### 4. **列表迭代器**
- **`ListIterator<E> listIterator()`**  
  返回一个 **列表迭代器**，支持双向遍历和修改操作。  
  示例：
  
  ```java
  ListIterator<String> listIterator = list.listIterator();
  while (listIterator.hasNext()) {
      String element = listIterator.next();
      System.out.println(element);
  }
  ```
```
  
- **`ListIterator<E> listIterator(int index)`**  
  返回一个从指定索引开始的列表迭代器。  
  示例：
  
  ```java
  ListIterator<String> listIterator = list.listIterator(1); // 从索引 1 开始
```

---

### 5. **批量操作**
- **`boolean addAll(int index, Collection<? extends E> c)`**  
  在指定索引位置插入另一个集合中的所有元素。  
  示例：
  
  ```java
  List<String> list1 = new ArrayList<>();
  list1.add("Java");
  List<String> list2 = new ArrayList<>();
  list2.add("Python");
  list1.addAll(1, list2); // 在索引 1 处插入 list2 的所有元素
  ```

---

### 6. **替换所有元素**
- **`default void replaceAll(UnaryOperator<E> operator)`**  
  对列表中的每个元素执行指定操作，并替换为操作结果。  
  示例：
  
  ```java
  List<String> list = new ArrayList<>();
  list.add("Java");
  list.add("Python");
  list.replaceAll(String::toUpperCase); // 将所有元素转换为大写
  ```

---

### 7. **排序**
- **`default void sort(Comparator<? super E> c)`**  
  根据指定的比较器对列表进行排序。  
  示例：
  
  ```java
  List<String> list = new ArrayList<>();
  list.add("Java");
  list.add("Python");
  list.sort(Comparator.naturalOrder()); // 按自然顺序排序
  ```

---

### 总结
`List` 相对于 `Collection` 特有的常用 API 主要围绕 **索引操作** 和 **有序性**，包括：

- 通过索引获取、设置、插入和删除元素（`get`、`set`、`add`、`remove`）。
- 查找元素的索引（`indexOf`、`lastIndexOf`）。
- 获取子列表（`subList`）。
- 使用列表迭代器（`listIterator`）。
- 在指定位置插入集合（`addAll`）。
- 替换所有元素（`replaceAll`）。
- 排序（`sort`）。

这些 API 使得 `List` 在处理有序数据时更加灵活和强大。

## ArrayList

没有什么特有的且常用的API

## LinkedList

`LinkedList` 是基于 **双向链表** 实现的，因此它支持高效的头部和尾部操作。此外，它还实现了 `Deque` 接口，提供了双端队列和栈的操作方法。

---

### 1. **头部和尾部操作**
 （1）**`void addFirst(E e)`**
在链表头部插入元素。  
示例：
```java
LinkedList<String> list = new LinkedList<>();
list.addFirst("Java"); // 头部插入
System.out.println(list); // 输出: [Java]
```

 （2）**`void addLast(E e)`**
在链表尾部插入元素。  
示例：

```java
list.addLast("Python"); // 尾部插入
System.out.println(list); // 输出: [Java, Python]
```

 （3）**`E getFirst()`**
返回链表头部的元素。  
示例：

```java
String first = list.getFirst(); // 获取头部元素
System.out.println(first); // 输出: Java
```

（4）**`E getLast()`**
返回链表尾部的元素。  
示例：

```java
String last = list.getLast(); // 获取尾部元素
System.out.println(last); // 输出: Python
```

（5）**`E removeFirst()`**
删除并返回链表头部的元素。  
示例：

```java
String removedFirst = list.removeFirst(); // 删除头部元素
System.out.println(removedFirst); // 输出: Java
System.out.println(list); // 输出: [Python]
```

 （6）**`E removeLast()`**
删除并返回链表尾部的元素。  
示例：

```java
String removedLast = list.removeLast(); // 删除尾部元素
System.out.println(removedLast); // 输出: Python
System.out.println(list); // 输出: []
```

---

### 2. **双端队列操作**
`LinkedList` 实现了 `Deque` 接口，因此支持双端队列操作。

（1）**`boolean offerFirst(E e)`**
在链表头部插入元素（队列操作）。  
示例：
```java
list.offerFirst("Java"); // 头部插入
System.out.println(list); // 输出: [Java]
```

（2）**`boolean offerLast(E e)`**
在链表尾部插入元素（队列操作）。  
示例：
```java
list.offerLast("Python"); // 尾部插入
System.out.println(list); // 输出: [Java, Python]
```

 （3）**`E pollFirst()`**
删除并返回链表头部的元素（队列操作）。  
示例：
```java
String polledFirst = list.pollFirst(); // 删除头部元素
System.out.println(polledFirst); // 输出: Java
System.out.println(list); // 输出: [Python]
```

 （4）**`E pollLast()`**
删除并返回链表尾部的元素（队列操作）。  
示例：
```java
String polledLast = list.pollLast(); // 删除尾部元素
System.out.println(polledLast); // 输出: Python
System.out.println(list); // 输出: []
```

 （5）**`E peekFirst()`**
返回链表头部的元素，但不删除（队列操作）。  
示例：
```java
list.add("Java");
String peekedFirst = list.peekFirst(); // 查看头部元素
System.out.println(peekedFirst); // 输出: Java
```

（6）**`E peekLast()`**
返回链表尾部的元素，但不删除（队列操作）。  
示例：
```java
list.add("Python");
String peekedLast = list.peekLast(); // 查看尾部元素
System.out.println(peekedLast); // 输出: Python
```

---

### 3. **栈操作**
`LinkedList` 可以作为栈使用，支持后进先出（LIFO）的操作。

（1）**`void push(E e)`**
将元素压入栈顶（链表头部）。  
示例：
```java
list.push("Java"); // 压入栈顶
list.push("Python"); // 压入栈顶
System.out.println(list); // 输出: [Python, Java]
```

（2）**`E pop()`**
弹出栈顶元素（链表头部）。  
示例：
```java
String popped = list.pop(); // 弹出栈顶元素
System.out.println(popped); // 输出: Python
System.out.println(list); // 输出: [Java]
```

 （3）**`E peek()`**
返回栈顶元素（链表头部），但不删除。  
示例：

```java
String peeked = list.peek(); // 查看栈顶元素
System.out.println(peeked); // 输出: Java
```

---

### 4. **逆序迭代器**
（1）**`Iterator<E> descendingIterator()`**
返回一个逆序迭代器，用于从尾部向头部遍历链表。  
示例：

```java
list.add("Java");
list.add("Python");
list.add("C++");

Iterator<String> descendingIterator = list.descendingIterator();
while (descendingIterator.hasNext()) {
    System.out.println(descendingIterator.next());
}
// 输出:
// C++
// Python
// Java
```

---

### 5. **其他方法**
（1）**`boolean removeFirstOccurrence(Object o)`**
删除链表中第一次出现的指定元素。  
示例：

```java
list.add("Java");
list.add("Python");
list.add("Java");
list.removeFirstOccurrence("Java"); // 删除第一个 "Java"
System.out.println(list); // 输出: [Python, Java]
```

（2）**`boolean removeLastOccurrence(Object o)`**
删除链表中最后一次出现的指定元素。  
示例：
```java
list.add("Java");
list.add("Python");
list.add("Java");
list.removeLastOccurrence("Java"); // 删除最后一个 "Java"
System.out.println(list); // 输出: [Java, Python]
```

---

### 总结
`LinkedList` 的特有 API 主要围绕 **头部和尾部操作**、**双端队列操作**、**栈操作** 和 **逆序迭代器**。这些方法使得 `LinkedList` 在以下场景中非常高效：

- 需要频繁在头部或尾部插入/删除元素。
- 需要实现队列或栈的功能。
- 需要逆序遍历链表。





## ArrayList和LinkedList的区别

ArrayList 和LinkedList，两者都是有序，可重复，有索引，为什么还设计成两个？ 

答： `ArrayList` 和 `LinkedList` 虽然都是 `List` 接口的实现类，具有 **有序、可重复、有索引** 的特性，但它们的 **底层实现** 和 **性能特点** 完全不同。设计成两个类是为了满足不同的**使用场景和性能需求**。以下是它们的区别和设计原因：

### 1. **底层实现**

+ **`ArrayList`**：
  + 基于 **动态数组** 实现。
  + 内存中是连续存储的。
  + 支持快速随机访问（通过索引）。
+ **`LinkedList`**：
  + 基于 **双向链表** 实现。
  + 内存中是分散存储的，每个元素通过指针连接。
  + 不支持快速随机访问（**支持索引，为啥不能快速访问呢？**答：LinkedList由双向链表实现，在内存中是分散的，所以不能通过向像数组那样通过索引快速查找，LinkedList的索引，是基于索引，循环链表实现的），但插入和删除效率高。

------

### 2. **性能对比**

| 操作                    | `ArrayList`                      | `LinkedList`                     |
| :---------------------- | :------------------------------- | :------------------------------- |
| **随机访问（get/set）** | **O(1)**：通过索引直接访问元素。 | **O(n)**：需要从头或尾遍历链表。 |
| **插入/删除（头部）**   | **O(n)**：需要移动后续元素。     | **O(1)**：只需修改指针。         |
| **插入/删除（尾部）**   | **O(1)**（如果不需要扩容）。     | **O(1)**：只需修改指针。         |
| **插入/删除（中间）**   | **O(n)**：需要移动后续元素。     | **O(n)**：需要遍历到指定位置。   |
| **内存占用**            | 较少：只存储元素。               | 较多：每个元素需要额外存储指针。 |

------

### 3. **设计原因**

 （1）**`ArrayList` 的设计目的**

+ **快速随机访问**：`ArrayList` 的底层是数组，支持通过索引直接访问元素，时间复杂度为 O(1)。
+ **内存连续**：数组的内存是连续的，适合需要频繁读取数据的场景。
+ **适合场景**：
  + 需要频繁访问元素（例如通过索引）。
  + 数据量较大，但插入和删除操作较少。

 （2）**`LinkedList` 的设计目的**

+ **高效插入和删除**：`LinkedList` 的底层是链表，插入和删除操作只需修改指针，时间复杂度为 O(1)。
+ **内存分散**：链表的内存是分散的，适合需要频繁插入和删除的场景。
+ **适合场景**：
  + 需要频繁插入和删除元素（例如实现队列或栈）。
  + 数据量较小，或者对内存占用不敏感。

------

### 4. **使用场景示例**

（1）**`ArrayList` 的典型场景**

+ **读取密集型操作**：

  ```java
List<String> list = new ArrayList<>();
  list.add("A");
  list.add("B");
  list.add("C");
  String element = list.get(1); // 快速访问索引 1 的元素
  ```

+ **数据存储**：

  

  ```java
List<Integer> numbers = new ArrayList<>();
  for (int i = 0; i < 1000000; i++) {
      numbers.add(i); // 添加大量数据
  }
  ```

 （2）**`LinkedList` 的典型场景**

+ **频繁插入和删除**：

  ```java
  List<String> list = new LinkedList<>();
  list.add("A");
  list.add("B");
  list.add(1, "C"); // 在索引 1 处插入元素
  list.remove(0); // 删除索引 0 处的元素
  ```

+ **实现队列或栈**：


  ```java
  Queue<String> queue = new LinkedList<>();
  queue.offer("A");
  queue.offer("B");
  String first = queue.poll(); // 移除并返回队首元素
  ```

------

### 5. **总结**

| 特性              | `ArrayList`    | `LinkedList`        |
| :---------------- | :------------- | :------------------ |
| **底层实现**      | 动态数组       | 双向链表            |
| **随机访问性能**  | 快（O(1)）     | 慢（O(n)）          |
| **插入/删除性能** | 慢（O(n)）     | 快（O(1)）          |
| **内存占用**      | 较少           | 较多                |
| **适合场景**      | 读取密集型操作 | 插入/删除密集型操作 |

+ **`ArrayList`**：适合需要频繁读取数据的场景，例如数据存储、随机访问。
+ **`LinkedList`**：适合需要频繁插入和删除的场景，例如实现队列、栈或中间插入操作。

设计成两个类是为了让开发者根据具体需求选择最合适的实现，从而优化性能。



## Set

Set集合特点：无序：输入的顺序，输出是无序的（LinkedHashSet有序,TreeSet 按照大小默认升序排序），**不重复**，无索引

Set的子类 HashSet, LinkedHashSet,TreeSet.

Set接口并没有特定的常用API，全是继承子Collection接口。

---

## HashSet

继承子Colletion---->Set.没有特定常用的API。基本上都是使用Collection的API.

`HashSet` 是 Java 集合框架中的一个常用类，它的核心特性是 **无序** 和 **不重复**。这两个特性是由其底层实现和设计原理决定的。以下是详细解析：

---

### 1. **为什么 `HashSet` 是无序的？**

`HashSet` 的无序性是由其底层数据结构 **哈希表** 决定的。

**哈希表的存储机制**

- `HashSet` 的底层是一个 `HashMap`，元素存储在 `HashMap` 的键中。
- 当添加元素时，`HashSet` 会通过哈希函数计算元素的哈希值，然后根据哈希值将元素分配到哈希表的某个位置（桶）。
- 哈希函数的设计目标是 **均匀分布**，因此元素的存储位置与插入顺序无关。

**遍历顺序**

- 当遍历 `HashSet` 时，元素的顺序取决于哈希表中的存储顺序，而不是插入顺序。
- 由于哈希表的存储位置是通过哈希值计算的，因此遍历顺序是 **不可预测的**。

**示例**

```java
Set<String> set = new HashSet<>();
set.add("Java");
set.add("Python");
set.add("C++");
System.out.println(set); // 输出顺序可能与插入顺序不同
```

---

### 2. **为什么 `HashSet` 是不重复的？**

`HashSet` 的不重复性是由其底层实现和 `equals`、`hashCode` 方法决定的。

**`HashMap` 的键唯一性**

- `HashSet` 的底层是 `HashMap`，而 `HashMap` 的键是唯一的。
- 当添加元素时，`HashSet` 会调用元素的 `hashCode` 方法计算哈希值，然后通过 `equals` 方法判断是否已存在相同的元素。

**`equals` 和 `hashCode` 方法**

- **`hashCode` 方法**：用于计算元素的哈希值，决定元素在哈希表中的存储位置。
- **`equals` 方法**：用于判断两个元素是否相等。
- 如果两个元素的 `hashCode` 相同且 `equals` 返回 `true`，则认为它们是重复的。

**添加元素的流程**

1. 使用无参构造器，创建一个默认16的数组，默认负载因子0.75

1. 计算元素的哈希值。
2. 根据哈希值找到对应的桶。
3. 如果桶为空，则直接添加元素。
4. 如果桶不为空，则遍历桶中的元素，调用 `equals` 方法判断是否已存在相同的元素。
   - 如果存在相同的元素，则返回 `false`，表示添加失败。
   - 如果不存在相同的元素，则添加元素。

**示例**

```java
Set<String> set = new HashSet<>();
set.add("Java"); // 返回 true
set.add("Java"); // 返回 false，因为元素已存在
```

tips: 如果HashSet 存的是对象，我们的业务可能需要对对象去重直接使用HashSet可能会出现问题，因为去重的判断是根据hashcode以及equls判断的，两个内容相同的对象，我们人认为是相同的，但是hashcode是根据地址算的。所以要重写hashcode以及equls方法。

---

### 3. **`HashSet` 的无序性和不重复性的实现原理**

**无序性的实现原理**

- 元素的存储位置由哈希值决定，而哈希值与插入顺序无关。
- 哈希表的存储结构（数组 + 链表/红黑树）决定了遍历顺序与插入顺序不一致。

**不重复性的实现原理**

- 通过 `hashCode` 和 `equals` 方法判断元素是否重复。
- 如果两个元素的 `hashCode` 相同且 `equals` 返回 `true`，则认为它们是重复的。

---

### 4. **如何保证 `HashSet` 的有序性？**

如果需要有序的 `Set`，可以使用以下实现类：

- **`LinkedHashSet`**：基于哈希表和链表实现，元素按插入顺序排序。
- **`TreeSet`**：基于红黑树实现，元素按自然顺序或自定义顺序排序。

**`LinkedHashSet` 示例**

```java
Set<String> set = new LinkedHashSet<>();
set.add("Java");
set.add("Python");
set.add("C++");
System.out.println(set); // 输出: [Java, Python, C++]
```

**`TreeSet` 示例**

```java
Set<String> set = new TreeSet<>();
set.add("Java");
set.add("Python");
set.add("C++");
System.out.println(set); // 输出: [C++, Java, Python]
```

---

### 5. **总结**

- **`HashSet` 的无序性**：由哈希表的存储机制决定，元素的存储位置与插入顺序无关。
- **`HashSet` 的不重复性**：由 `hashCode` 和 `equals` 方法保证，如果两个元素的 `hashCode` 相同且 `equals` 返回 `true`，则认为它们是重复的。
- 如果需要有序的 `Set`，可以使用 `LinkedHashSet` 或 `TreeSet`。



## LinkedHashSet

没有特定的API，继承字Collection--->Set

有序（输出按着输入的顺序），不重复，无索引。

和HashSet 的实现一样，只不过多了一个双向列表去维护插入的顺序。





## TreeSet

`TreeSet` 是 `Set` 接口的一个实现，基于红黑树（红黑树是一种自平衡的二叉查找树）。它的元素是**有序**的，并且不允许重复元素。除了继承自 `Set` 的常用方法外，`TreeSet` 还提供了以下特定的 API：

1. **first()**
    返回 `TreeSet` 中的第一个（最小）元素。

   ```java
   E first = treeSet.first();
   ```

2. **last()**
    返回 `TreeSet` 中的最后一个（最大）元素。

   ```java
   E last = treeSet.last();
   ```

3. **ceiling(E e)**
    返回大于或等于给定元素的最小元素。如果没有这样的元素，返回 `null`。

   ```java
   E ceiling = treeSet.ceiling("apple");
   ```

4. **floor(E e)**
    返回小于或等于给定元素的最大元素。如果没有这样的元素，返回 `null`。

   ```java
   E floor = treeSet.floor("banana");
   ```

5. **higher(E e)**
    返回严格大于给定元素的最小元素。如果没有这样的元素，返回 `null`。

   ```java
   E higher = treeSet.higher("apple");
   ```

6. **lower(E e)**
    返回严格小于给定元素的最大元素。如果没有这样的元素，返回 `null`。

   ```java
   E lower = treeSet.lower("banana");
   ```

7. **pollFirst()**
    返回并移除 `TreeSet` 中的第一个元素。

   ```java
   E removedFirst = treeSet.pollFirst();
   ```

8. **pollLast()**
    返回并移除 `TreeSet` 中的最后一个元素。

   ```java
   E removedLast = treeSet.pollLast();
   ```

9. **comparator()**
    返回 `TreeSet` 使用的排序比较器（如果没有自定义排序，则返回 `null`，表示使用自然排序）。

   ```java
   Comparator<? super E> comparator = treeSet.comparator();
   ```







