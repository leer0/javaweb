## java集合类

#### 一、Collection接口和它的Set、List子接口

Collection接口和它的子接口以及实现类的结构如下图所示

![](C:\Users\leer0\Pictures\Collection集合体系继承树.png)

**List**表示的是**有序**且元素**可重复**的容器。

**Set**表示的是**无序**且元素**不可重复**的容器，容器内没有索引。

##### 1、List接口的实现类

1.1、ArrayList类

**实现原理：**

ArrayList底层是由**数组**实现的存储。实现动态数组存储的方式是每新添加一个元素，首先检测数组是否已经满了。当数组的大小不够用时，就去直接创建一个更大的数组，这个更大的数组相较原先的数组增加的长度可以在创建ArrayList对象时传入，该长度也是原始数组的长度，**所以每次扩容相当于扩大到原先数组的两倍长度**。若未设置则使用默认值。然后再将原先的数组数据拷贝到新创建的数组中。

**特点：**

1、查询效率高

2、增删效率低（每当删除或增加元素时，都要拷贝多个元素来实现元素位置移动）

3、线程不安全



**方法：**

size():返回元素的个数

indexOf(“a”):返回指定元素a第一次出现时的索引

lastIndexOf("A"):返回指定元素a最后一次出现时的索引

remove(o):删除第一个与匹配的元素



1.2、LinkedList类

**实现原理：**

LinkedList底层使用双向链表实现存储。

**特点：**

1、查询效率低

2、增删效率高

3、线程不安全

**方法：**

add(E):boolean ——在链表末尾添加新元素



1.3、Vector类

Vector底层使用数组实现存储，相关的方法增加了同步检查，如indexOf方法增加了synchronized同步标记。因此具有**线程安全**但**效率低**的特点，实际使用的情况不多。



**建议：**

1、需要线程安全时使用Vector

2、不存在线程安全问题，并且查找较多时使用ArrayList

3、不存在线程安全问题，并且增删较多时使用LinkedList



##### 2、Set接口的实现类



1、HashSet

HashSet底层使用Map实现。

2、TreeSet

TreeSet底层实际是用TreeMap实现，内部维持了一个简化的TreeMap，通过key来实现存储Set的元素。TreeSet需要对存储的元素进行排序，因此需要多元素对应的类**实现Compareable接口**，然后根据compareTo()方法比较元素之间的大小。



#### 二、Map接口

1、Map接口的实现类

1.1、HashMap类

HashMap底层实现使用了哈希表，**哈希表的基本结构是数组+链表，当链表长度大于8时，链表就转换为红黑树，**这样就又大大提高了链表的查询效率。

**JDK 1.8 HashMap 采用位桶 + 链表 + 红黑树实现。（当链表长度超过阈值 “8” 时，将链表转换为红黑树）**

**java中规定：两个内容相同(equals()返回值为true)的对象必须具有相等的hashcode。**

![](C:\Users\leer0\Pictures\Hash存储键值对的过程.PNG)

位桶数组扩容：capacity 是数组（bucket）的大小，loadFactor 是数组的最大填满比例。当数组中的节点（entry）数目 >capacity∗loadFactor 时，就需要扩容，调整数组的大小为当前的 2 倍，以提高 HashMap 的 hash 效率。



1.2、TreeMap类

TreeMap是典型的红黑树(**一种自平衡二叉查找树\排序树**)的实现，存储的元素按照key递增的方式排序。



1.3、HashMap和HashTable的区别

1、HashMap：线程不安全，效率高。允许key或value为null

2、HashTable：线程安全，效率低。不允许key或value为null

#### 三、遍历各个容器的方法：

##### 1、使用迭代器

```java
//List和Map的便利步骤相同
List<String> list = new ArrayList<>();
list.add("aa");
list.add("ab");
list.add("ac");
for(Iterate<String> iter = list.iterator();iter.hasNext();){
    String temp = iter.next();
    iter.remove();//如果遍历时要删除集合中的元素，建议使用这种方式
    System.out.println(temp);
}

//第一种Map的遍历方式
Map<String,Object> map = new HashMap<>();
map.put("01","aa");
map.put("02","ab");
map.put("03","ac");
Set<Entry<String,Object>> ss = map.entrySet();
for(Iterator<Entry<String,Object>> iter = ss.iterator();iter.hasNext();){
    Entry<String,Object> temp = iter.Next();
    System.out.println(temp.getKey()+"---"+temp.getValue());
}
//第二种Map的遍历方式
Set<String> keySet = map.keySet();
for(Iterator iter = keySet.Iterator();iter.hasNext();){
    String key = iter.next();
    System.out.println(key+"---"+map.get(key));
}
```

##### 2、其他类型的便利方式

```java
//增强for循环，Set同理
for(String temp:list){
    System.out.println(temp);
}
```

