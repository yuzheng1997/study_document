# java集合

###### 1.集合类存放于java.util包中，主要有set,list(包含Queue)和map

	1. Collection: Collection是集合List,Set,Queue的最基本的接口
 	2. Iterator： 迭代器，可以通过迭代器遍历集合中的数据
 	3. Map: 是映射表的基础接口

##### 2. ArrayList(数组)

​	ArrayList是最常用的List实现类，使用数组实现，允许对原始快速访问，缺点是每个元素不能有间隔，当数组大小不满足时需要增加存储能力，就要将已经有数组的数据复制到新的存储空间中。当ArrayList中间位置插入或者删除元素时，需要对数组进行复制，移动，代价较高。因此适合随机查找和遍历，不适合增删。

###### 3. Vector

​	和ArrayList一样，使用数组实现，同步的，线程安全的

###### 4. LinkList

​	使用链表结构存储数据，很适合数据的动态插入和删除，随机访问和遍历较慢，提供了List接口中没有定义的方法，专门操作表头和表尾元素，可以当作堆栈，队列和双向队列使用。

###### 5. Set

​	Set注重独一无二的性质，该体系集合用于存储无序元素，值不能重复，对象的相等性本质是对象的hashCode判断，如果想让两个对象视为相等就必须覆盖Object的hashCode方法和equals方法。

###### 6. HashSet

​	哈希表存放的哈希值。HashSet存储元素的顺序并不是按照存入时的顺序而是按照哈希值来存的所以取数据也是按照哈希值取，元素的哈希值可以通过元素的hashcode方法获取。HashSet首先判断两个元素的哈希值，如果哈希值一样就会比较equals方法，如果equals为真，HashSet就视为元素相同。不为真就不说一个元素。

###### 7. TreeSet

	1. TreeSet是使用二叉树的原理对新add的对象按指定的顺序排列（升序或降序），每增加一个对象都会排序，将对象插入的二叉树指定的位置。
 	2.  Integer和String 对象都可以进行默认的TreeSet排序，而自定义类的对象是不可以的，自定义的类必须实现Comparable接口，并覆写相应的compareTo函数，才可以正常使用
 	3. 在覆写compare函数时，要返回相应的值才能使用TreeSet按照一定的规则来排序
 	4. 比较对象与指定的对象顺序，小于返回负整数，等于返回0，大于返回正整数

###### 8. LinkHashSet

​	对于LinkedHashSet而言，它继承于HashSet又基于LinkedHashMap来实现的，LinkedhashSet底层使用LinkedHashMap来保存所有元素，继承与HashSet，其方法操作与HashSet相同。

###### 9. HashMap（数组+链表+红黑树）

​	HashMap根据hashCode存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但便利顺序却是不确定的，HashMap最多只允许一条记录的键为null，允许多条记录的value为null。hashMap不是线程安全的，如果需要满足线程安全，可以用Collections的synchronizedMap方法使hashMap具有线程安全能力，或者使用ConcurrentHashMap

###### 10. HashMap  java8 的实现

​	java8对HashMap进行了修改，最大的不同就是利用了红黑树，所以其由数组+链表+红黑树组成。

​	java7中，我们查找的时候，先根据哈市值快速定位到数组的下标。但之后需要顺着链表一个个比较下去才能找到我们需要的。java8中当链表超过8个以后，会将链表转换为红黑树。

###### 11. ConcurrentHashMap

​	ConcurrentHashMap和HashMap思路差不多，但ConcurrentHashMap支持并发，所以稍微复杂一些整个C哦那currentHashMap由一个个Segment组成，Segment代表“部分”或者“一段”，所以很多地方会将其描述为分段锁。

###### 12. ConcurrentHashMap线程安全

​	简单理解就是，ConcurrentHashMap是一个Segment数组，Segment通过继承ReentrantLock来加锁，所以每次需要加锁的都是一个segment。

###### 13. HashTable

​	不建议在新的代码中使用，线程安全，并发性不如ConcurrentHashMap。

###### 14. TreeMap（可排序）

​	TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按照键值升序排序，也可指定排序的比较器，使用Iterator遍历TreeMap时，得到的记录是排过序的。

​	使用TreeMap时，key必须实现Comparable接口或在构造时传入自定义Comparable，否则运行时抛出ClassCastException。

###### 15. LinkHashMap（记录插入顺序）

​	LinkHashMap是HashMap的一个子类，但保存了插入顺序，在用Iterator遍历时，也是有序的。