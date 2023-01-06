# IDEA快捷键整理

## MarkDown

新建行：Ctrl + ENTER

单元格内另起一行（相当于 \<br>）：Ctrl + Shift + ENTER

## 循环

- 普通 for 循环：fori
- 增强 for 循环：iter + Enter
- 迭代器：itit



## 跳转

- Ctrl + ⬅ / ➡ ：文件间跳转
- Ctrl + alt + ⬅ / ➡：返回刚才操作的位置



## 注释

- 注释有空格的问题
  - Editor --> Code Style --> Java --> Code Generation，设置"Add spaces around block" 替换成"Add a space at line comment start"



## 继承

- 查看一个类的层级关系（继承有用）：Ctrl + H

- 查看接口的子类：Ctrl + Alt + B

- 查看接口的父类：Ctrl + Alt + P
- 重写 接口/父类方法：Ctrl + o(overide)

## 绝对路径复制

copy ---> Absolute Path

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171803960.png)

## 目录跳转

1. Alt + 1 ：打开 project 目录
2. Alt + 7：打开 Structure  结构

## 跳转至 开头 / 结尾

CTRL + Home

CTRL + End

## 格式化

CTRL + ALT + L

## 查看方法在哪里被调用

Ctrl + ALT + H

## SequeenceDiagram 时序图

![image-20221222174729889](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221747991.png)

设置：

- 调用的深度
- 跳过 getset方法
- 私有函数
- 构造函数
- 只显示项目本身代码

![image-20221222175010046](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221750091.png)

根据自己的需求，来查看响应方法的调用即可

## 枚举类型遍历

```java
//  获取该 filter所有的配置参数名
Enumeration<String> initParameterNames = filterConfig.getInitParameterNames();
//  枚举类型遍历
while (initParameterNames.hasMoreElements()){
    System.out.println("名字= " + initParameterNames.nextElement());
}
```

## 运算符号整理

### 移位运算符

#### 有符号

|      |      |
| ---- | ---- |
| >>   |      |
| <<   |      |

#### 无符号

|      |      |
| ---- | ---- |
| >>>  |      |
| <<<  |      |

## boolean数组初始化为 true

```java
boolean[] toFill = new boolean[100] {};
Arrays.fill(toFill, true);
```

## Arrays.copyOf（）方法

```java
import java.util.Arrays;

public class ArrayDemo {
    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5}; 
        int[] arr2 = Arrays.copyOf(arr1, 5);
        int[] arr3 = Arrays.copyOf(arr1, 10);
        for(int i = 0; i < arr2.length; i++) 
            System.out.print(arr2[i] + " "); 
        System.out.println();
        for(int i = 0; i < arr3.length; i++) 
            System.out.print(arr3[i] + " ");
    }
} 
```

运行结果如下：

```java
1 2 3 4 5 
1 2 3 4 5 0 0 0 0 0
```

## 线程

![image-20221222190340837](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221903926.png)

### 获取当前线程 id

```java
Thread.currentThread().getId();
```

### 线程 dump

- 线程 dump 的目的是转存线程快照。（快照中是当前 JVM 所有线程正在执行方法的堆栈信息）
- 通过线程 dump 来分析定位线程出现长时间停顿的原因，如：
  - 线程死锁
  - 线程死循环
  - 线程请求外部资源长时间等待等

#### 1. jstack

##### 1.1 描述

- 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源（死循环、死锁）。
- 如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。
- jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息, 如果现在运行的java程序呈现 <font color="blue">hung </font>【挂起、假死，这里的外在表现就是不报错，好像也不干活。但是也没有任何报错】的状态，jstack是非常有用的。

文档：https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jstack.html#BABGJDIF

##### 1.2 操作

转存至快照，并通过查看当前进程中占用资源异常的线程 pid，快速定位到问题线程的堆栈信息

- 查看线程堆栈信息

  ```bash
  jstack [Pid] jstack -l -F [Pid] //查看进程是否存在死锁
  ```

- 转存线程堆栈映像文件

  ```bash
  jstack [pid]> loop.txt //jstack会自动找出死锁，并把死锁信息放在文件末尾
  ```

- 查看进程中线程资源占用情况

  ```bash
  top -p [pid] -H
  ```

- pid转十六进制

  ```bash
  printf "%x" 5016 //pid转十六进制为了查看dump文件中，该线程的堆栈信息（dump文件中，线程id以16进制呈现）
  ```

##### 1.3 分析

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221853876.png)

##### 1.4 相关概念

<font color="yellow">nid</font>：线程的<font color="yellow"> 唯一标识符</font> ，是 16 进制的，通常用于定位某个线程

java.lang.Thread.State：线程的状态标识

- NEW 未启动的新线程
- RUNNABLE 可运行的线程
  - Ready
  - Running
- TimedWaiting：超时等待
- Waiting：等待状态
- Blocked：阻塞状态（一般都是在等待锁资源）
- Terminated

##### 1.5 排查思路

- 线程状态的问题优先级：BLOCKED > WAITTING
- BLOCKED 状态的线程通常在 dump 文件的末尾部分
- 当发现某线程池中出现大量线程为 WAITTING 状态，需要查看一下线程的堆栈信息，必要则进行源代码分析

#### 2. 获取当前线程 dump

在断点调试的时候，我们可以通过点击下图红色箭头指向的相机图标，获取当前线程的dump信息。

这个功能有什么用呢？我们可以通过线程名，分析当前是哪个线程执行的，在多线程环境下对代码运行分析起到辅助作用。



比如下图1， `run()`方法是通过main主线程执行的，只是方法调用，并没有启动多线程（这是我们熟知结论的实践证明）

![图片](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221921290.png)

当我们把run方法改成`start()`方法时，可以看到是线程thread0执行的。

![图片](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212221930368.png)

## 集合相关方法

### hash（）方法

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
//	重写了 hashCode（）方法 = 100
//	所以：h = 100
//	最终结果为：100 ^ (100 >>> 16) = 100 ^ 0 = 100
```

### 根据 hash值 ---> 下标

```java
//	初始位置：15 & 100 = 4
if ((p = tab[i = (n - 1) & hash]) == null)	
//	31 & 100 = 4
//	63 & 100 = 36
```

### putVal（）方法

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {	
    Node<K,V>[] tab; Node<K,V> p; int n, i;	//	定义辅助变量
    if ((tab = table) == null || (n = tab.length) == 0)	//	初始化
        n = (tab = resize()).length;	//	n = 16（初始化容量大小为： 16）
    if ((p = tab[i = (n - 1) & hash]) == null)	//	计算 key 对应的 hash值，去计算该 key 应该在 table 表的哪一个索引位置去存放，并把这个位置的对象 赋给 辅助变量 p
        //	再判断是否为 null
        //	1）如果 p 为 null，表示还没有存放过元素，就创建一个 Node（key="java", value = PRESENT）放在该位置
        //	为什么把 key 对应的 hash 也存进去了，因为将来会去比较，如果再有的话，它会去看，如果相等会往后怼
        tab[i] = newNode(hash, key, value, null);
    else {
        //	辅助变量
        Node<K,V> e; K k;
        if (p.hash == hash &&	//	不能添加
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)	//	树
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {	//	链表【新加入的 Node，与链表中的 Node依次比较】
            for (int binCount = 0; ; ++binCount) {	
                if ((e = p.next) == null) {	//	能加的情况，还要看是否要树化，e = null
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // TREEIFY-THRESHOLD = 8【binCount 从0开始计数，当binCount = 0 时候，表示将要插入到第 2个位置；依次类推，binCount = 1 时，表示要插入到第 3个位置；所以当 binCount = 7时，表要要插入到第 9个位置。此时进行树化
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&	//	如果在循环过程中，发现有相同，就不添加了 break【e != null】只是替换 value
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;	//e = p (链表的首位置)
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;	// 更新 e.value = 新传进来的 value
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;	//	添加成功的次数
    if (++size > threshold)	//	 1 > threshold = 12 false
        resize();
    afterNodeInsertion(evict);	//	evict 默认是 true，在结点后插入 【对于 HashMap来说是空方法，目的是为了让它的子类去是实现这个方法，再做一些动作的】
    return null;
}
```

### resize（）方法

调用 Node<K, V>[] resize() 方法，初始化或加倍表大小

- 如果为 null，则按照字段阈值中保存的初始容量目标进行分配
- 否则，因为我们使用二次幂展开，每个 bin 中的元素必须保持相同的索引，或者在新表中以二次幂的偏移量移动

```java
/**
     * Initializes or doubles table size.  If null, allocates in
     * accord with initial capacity target held in field threshold.
     * Otherwise, because we are using power-of-two expansion, the
     * elements from each bin must either stay at same index, or move
     * with a power of two offset in the new table.
     *
     * @return the table
     */
//	allocates：分配
//	in accord with ：符合
//	power of two：二次幂

final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;	//	旧容量
    int oldThr = threshold;	//	旧阈值（初始为：0）
    int newCap, newThr = 0;
    if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults（初始化）
        newCap = DEFAULT_INITIAL_CAPACITY;	//	16
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) { //	新阈值：16 * 0.75 = 12 != 0【table大小有16，但是只要用了12个，就要开始扩容了】
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;		//	更新 阈值 threshold = 12
    @SuppressWarnings({"rawtypes","unchecked"})
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];	//	创建初始默认容量大小数组（16）
    table = newTab;
    if (oldTab != null) {	//	初始化时，oldTab == null，后面的代码都不执行，直接走到最后的 return
        for (int j = 0; j < oldCap; ++j) {	//	遍历旧 table 数组
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {	
                oldTab[j] = null;	//	将不为 null 的置 null
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;	//	扩容后 索引位置也会同步更新
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order 【原位置是链表的情况】
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;	//	返回初始容量大小数组
}
```

###  treeifyBin(tab, hash)方法

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)	//	n = tab.length = 64 【MIN_TREEIFY_CAPACITY=64】
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);	//	替换树  
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```

#### 当 tab.length < 64时，调用resize() 扩容

#### 当 tab.length >= 64时，调用 treeify(tab) 进行树化

```java
// For treeifyBin
TreeNode<K,V> replacementTreeNode(Node<K,V> p, Node<K,V> next) {
    return new TreeNode<>(p.hash, p.key, p.value, next);
}
```

```java
/**
         * Forms tree of the nodes linked from this node.
         * 从此节点链接的节点的表单树
         */
final void treeify(Node<K,V>[] tab) {
    TreeNode<K,V> root = null;
    for (TreeNode<K,V> x = this, next; x != null; x = next) {
        next = (TreeNode<K,V>)x.next;
        x.left = x.right = null;
        if (root == null) {
            x.parent = null;
            x.red = false;
            root = x;
        }
        else {
            K k = x.key;
            int h = x.hash;
            Class<?> kc = null;
            for (TreeNode<K,V> p = root;;) {
                int dir, ph;
                K pk = p.key;
                if ((ph = p.hash) > h)
                    dir = -1;
                else if (ph < h)
                    dir = 1;
                else if ((kc == null &&
                          (kc = comparableClassFor(k)) == null) ||
                         (dir = compareComparables(kc, k, pk)) == 0)
                    dir = tieBreakOrder(k, pk);

                TreeNode<K,V> xp = p;
                if ((p = (dir <= 0) ? p.left : p.right) == null) {
                    x.parent = xp;
                    if (dir <= 0)
                        xp.left = x;
                    else
                        xp.right = x;
                    root = balanceInsertion(root, x);
                    break;
                }
            }
        }
    }
    moveRootToFront(tab, root);
}
```

### HashMap 3个方法

####  1. keySet() 方法【Set 类型】

```java
Set set1 = map.keySet();
```

#### 2. values() 方法【Collection 类型】

#### 3. entrySet() 方法【Set 类型】

##### getKey() 方法

##### getValue() 方法

由于是 Set 类型，即实现了 Itarable 接口，可以使用迭代器

- 应用迭代器遍历时候，每个 entry 向下转型

```java
Map.Entry entry = (Map.Entry)iterator.next();
System.out.println(entry.getKey() + "-" + entry.getValue());	//	动态绑定到 HashMap$Node 类的 getKey 和 getValue() 方法，对应着 Node 的 key 和 value
```

### 分析 HashSet 和 TreeSet 如何实现去重的

1. HashSet
   - haasCode() + equals()
   - 底层先通过存入对象，进行运算得到一个 hash值，得到对应索引
     - 如果发现table 表索引所在的位置，没有数据，就直接存放
     - 如果有数据，就进行 equals 比较【遍历比较】
       - 比较后不相同，就加入
       - 否则不加入
2. TreeSet
   - 如果你传入了一个 Comparator匿名对象，就使用实现的 compare方法去重
     - 方法返回0，就认为是相同的元素 / 数据，就不添加了
   - 如果你没有传入Comparator匿名对象，则以你添加的对象实现的 Comparable 接口的 compareTo 去重

```java
public V put(K key, V value) {
    Entry<K,V> t = root;
    if (t == null) {
        compare(key, key); // type (and possibly null) check

        root = new Entry<>(key, value, null);
        size = 1;
        modCount++;
        return null;
    }
    int cmp;
    Entry<K,V> parent;
    // split comparator and comparable paths
    Comparator<? super K> cpr = comparator;
    if (cpr != null) {
        do {
            parent = t;
            cmp = cpr.compare(key, t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    }
    else {	//	现在没有传入 Comparactor 对象，代码走的是这部分了
        if (key == null)
            throw new NullPointerException();
        @SuppressWarnings("unchecked")
        Comparable<? super K> k = (Comparable<? super K>) key;	//	接口类型 ---> 实现这个接口的对象【向上转型】
        do {
            parent = t;
            cmp = k.compareTo(t.key);	//	调用了字符串本身的 compareTo() 方法 【动态绑定】
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    }
    Entry<K,V> e = new Entry<>(key, value, parent);
    if (cmp < 0)
        parent.left = e;
    else
        parent.right = e;
    fixAfterInsertion(e);
    size++;
    modCount++;
    return null;
}

```

分析下面代码是否会有异常：

```java
//	因为：现在没有传入 Comparactor接口的匿名内部类（对象）
//	所以，在底层会调用 添加的对象实现的 Comparable 接口的 compareTo 去重
//	走到了上述代码
//	即：把 Person 转成 Comparable 类型
//	由于这个 Person() 类不知道有没有实现 Comparable 接口，所以可能报错！！！---> 类型转换失败 ClassCastException
TreeSet treeSet = new TreeSet();
treeSet.add(new Person());	//	报错
```

注意：第一次添加的时候，也会进行类型转换

```java
//	第一次添加的时候，就会报错，类型转换异常
@SuppressWarnings("unchecked")
final int compare(Object k1, Object k2) {
    return comparator==null ? ((Comparable<? super K>)k1).compareTo((K)k2)
        : comparator.compare((K)k1, (K)k2);
}
```

### remove() 方法

```java
//	根据 obj 的 hash 值，会先算出在 table 中的位置，所过table表中该位置为null，则删除失败
//	删除成功的话，会返回 对应的 value 值
set.remove(obj);
```



```java
public boolean remove(Object o) {
    return map.remove(o)==PRESENT;
}
```

```java
public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;	//	删除成功时，会返回被删掉的 key 对应的 value
}
```

```java
final Node<K,V> removeNode(int hash, Object key, Object value,
                           boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {	//	要删除的位置不为 null
        Node<K,V> node = null, e; K k; V v;
        if (p.hash == hash &&	//	要删除位置的 hash
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        else if ((e = p.next) != null) {	//	链表的情况
            if (p instanceof TreeNode)
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            else {
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);	//	do-while  循环
            }
        }
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode)
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            else if (node == p)
                tab[index] = node.next;
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);
            return node;
        }
    }
    return null;	//	要删除的位置为 null
}
```





### Collections 工具类

java.util.Collections

1. Collections 是一个操作 Set、List 和 Map 等集合的工具类
2. 提供了一些系列静态放法对集合元素进行<font color="yellow">排序</font>、<font color="yellow"> 查询</font> 和<font color="yellow"> 修改</font> 等操作

```java
//	对象在集合中出现的次数
public static int frequency(Collection<?> c, Object o) {
    int result = 0;
    if (o == null) {
        for (Object e : c)
            if (e == null)
                result++;
    } else {
        for (Object e : c)
            if (o.equals(e))
                result++;
    }
    return result;
}
```



- 排序操作：（均为 static 方法）

  | 方法名                                                      | 作用                                                     |
  | ----------------------------------------------------------- | -------------------------------------------------------- |
  | reverse(List)                                               | 反转 List中的元素                                        |
  | shuffle(List) 【扑克牌洗牌】                                | 对 List 集合元素进行随机排序                             |
  | sort(List)                                                  | 根据元素的自然顺序对指定 List 集合元素按升序排序         |
  | sort(List, Comparator)                                      | 根据指定的 Comparator 产生的顺序对 List 集合元素进行排序 |
  | swap(List, int, int)                                        | 将指定 list 集合中的 i 处元素和 j 处元素进行交换         |
  | Object max(Collection)                                      | 根据元素的自然顺序，返回给定集合中的最大元素             |
  | Object max(Collection, Comparator)                          | 根据 Comparator 指定的顺序，返回给定集合中的最大元素     |
  | Object min(Collection)                                      |                                                          |
  | Object min(Collection, Comparator)                          |                                                          |
  | int frequency(Collection, Object)                           | 返回指定集合中指定元素的出现次数                         |
  | void copy(List dest, List src)                              | 将 src 中的内容复制到 dest中                             |
  | boolean replaceAll(List list, Object oldVal, Object newVal) | 使用 新值替换 List 对象的所有旧值                        |

  ```java
  Collections.max(list, new Comparator() {
  
      @Override
      public int compare(Object o1, Object o2) {
          return ((String)o1).length() - ((String)o2).length();
      }
  });
  ```

  ```java
  //	为了完成一个完整拷贝，我们需要先给 dest 赋值，大小和 list.size() 一样
  for(int i = 0; i < list.size(); i++){
      dest.add("");
  }
  ```

  

 

## 开发工具类

### 1.  流的读写方法

```java
/**
 * 此类用于演示关于流的读写方法
 */
public class StreamUtils {
    /**
	 * 功能：将输入流转换成byte[] ---> 即：可以把一个文件写入（输出）到 byte[]
	 */
    public static byte[] streamToByteArray(InputStream is) throws Exception{	//	可以接受一个输入流，当然InputStream是
        //	一个抽象类，所以只要是它的子类对象
        //	都可以传进来
        ByteArrayOutputStream bos = new ByteArrayOutputStream();//创建输出流对象
        byte[] b = new byte[1024];	//	字节数组
        int len;
        while((len=is.read(b))!=-1){	//	循环读取
            bos.write(b, 0, len);	//	读取到的数据（字节数组），写入到 bos 中去
        }
        byte[] array = bos.toByteArray();	//	然后将 bos 转成一个字节数组
        bos.close();
        return array;
    }
    /**
	 * 功能：将InputStream转换成String
     * 将输入流的数据直接给你转成一个字符串
	 */

    public static String streamToString(InputStream is) throws Exception{
        BufferedReader reader = new BufferedReader(new InputStreamReader(is));
        StringBuilder builder= new StringBuilder();
        String line;
        while((line=reader.readLine())!=null){  //    当读取到 null 时，就表示结束
            builder.append(line+"\r\n");
        }
        return builder.toString();
    }
}
```

### 2. 处理各种情况的用户输入

```java
/**
	工具类的作用:
	处理各种情况的用户输入，并且能够按照程序员的需求，得到用户的控制台输入。
*/

import java.util.*;
/**

*/
public class Utility {
    //静态属性。。。
    private static Scanner scanner = new Scanner(System.in);


    /**
     * 功能：读取键盘输入的一个菜单选项，值：1——5的范围
     * @return 1——5
     */
    public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1, false);//包含一个字符的字符串
            c = str.charAt(0);//将字符串转换成字符char类型
            if (c != '1' && c != '2' && 
                c != '3' && c != '4' && c != '5') {
                System.out.print("选择错误，请重新输入：");
            } else break;
        }
        return c;
    }

    /**
	 * 功能：读取键盘输入的一个字符
	 * @return 一个字符
	 */
    public static char readChar() {
        String str = readKeyBoard(1, false);//就是一个字符
        return str.charAt(0);
    }
    /**
     * 功能：读取键盘输入的一个字符，如果直接按回车，则返回指定的默认值；否则返回输入的那个字符
     * @param defaultValue 指定的默认值
     * @return 默认值或输入的字符
     */

    public static char readChar(char defaultValue) {
        String str = readKeyBoard(1, true);//要么是空字符串，要么是一个字符
        return (str.length() == 0) ? defaultValue : str.charAt(0);
    }

    /**
     * 功能：读取键盘输入的整型，长度小于2位
     * @return 整数
     */
    public static int readInt() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(2, false);//一个整数，长度<=2位
            try {
                n = Integer.parseInt(str);//将字符串转换成整数
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
    /**
     * 功能：读取键盘输入的 整数或默认值，如果直接回车，则返回默认值，否则返回输入的整数
     * @param defaultValue 指定的默认值
     * @return 整数或默认值
     */
    public static int readInt(int defaultValue) {
        int n;
        for (; ; ) {
            String str = readKeyBoard(10, true);
            if (str.equals("")) {
                return defaultValue;
            }

            //异常处理...
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串
     * @param limit 限制的长度
     * @return 指定长度的字符串
     */

    public static String readString(int limit) {
        return readKeyBoard(limit, false);
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串或默认值，如果直接回车，返回默认值，否则返回字符串
     * @param limit 限制的长度
     * @param defaultValue 指定的默认值
     * @return 指定长度的字符串
     */

    public static String readString(int limit, String defaultValue) {
        String str = readKeyBoard(limit, true);
        return str.equals("")? defaultValue : str;
    }


    /**
	 * 功能：读取键盘输入的确认选项，Y或N
	 * 将小的功能，封装到一个方法中.
	 * @return Y或N
	 */
    public static char readConfirmSelection() {
        System.out.println("请输入你的选择(Y/N)");
        char c;
        for (; ; ) {//无限循环
            //在这里，将接受到字符，转成了大写字母
            //y => Y n=>N
            String str = readKeyBoard(1, false).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }

    /**
     * 功能： 读取一个字符串
     * @param limit 读取的长度
     * @param blankReturn 如果为true ,表示 可以读空字符串。 
     * 					  如果为false表示 不能读空字符串。
     * 			
	 *	如果输入为空，或者输入大于limit的长度，就会提示重新输入。
     * @return
     */
    private static String readKeyBoard(int limit, boolean blankReturn) {

        //定义了字符串
        String line = "";

        //scanner.hasNextLine() 判断有没有下一行
        while (scanner.hasNextLine()) {
            line = scanner.nextLine();//读取这一行

            //如果line.length=0, 即用户没有输入任何内容，直接回车
            if (line.length() == 0) {
                if (blankReturn) return line;//如果blankReturn=true,可以返回空串
                else continue; //如果blankReturn=false,不接受空串，必须输入内容
            }

            //如果用户输入的内容大于了 limit，就提示重写输入  
            //如果用户如的内容 >0 <= limit ,我就接受
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不能大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}
```

### 3. 统计Web应用Servlet访问次数（ServletContext）  

```java
public class WebUtils {
    //  这个方法就是对访问次数的累积，同时返回次数
    public static Integer visitCount(ServletContext servletContext) {
        //  从 servletContext 中获取 visit_count 属性 k - v
        Object visit_count = servletContext.getAttribute("visit_count");
        //  判断 visit_count 是否为 null
        if (visit_count == null) {  //  说明是第一次访问网站
            servletContext.setAttribute("visit_count", 1);
            visit_count = 1;
        }else { //  第二次或者以后
            //  取出 visit_count 属性值 + 1
            visit_count = Integer.parseInt(visit_count + "") + 1;
            //    再放回到 servletContent中
            servletContext.setAttribute("visit_count", visit_count);
        }
        return Integer.parseInt(visit_count + "");
    }
}
```

将来想要统计哪个Web应用 Servlet访问的次数，只需要访问这个方法即可

### 4. Cookie 处理

```java
//	查找
public class CookieUtils {

    //  编写一个方法，返回指定名字的 cookie 值
    public static Cookie readCookieByName(String name, Cookie[] cookies){
        //  判断传入的参数是否正确
        if (name == null || "".equals(name) || cookies == null || cookies.length == 0){
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())){ //  如果名字相同
                return cookie;
            }
        }
        //  如果遍历完了之后，还是没有，就返回 null
        return null;
    }
}
```

```java
//	修改
public class UpdateCookie {
    public static Cookie updateCookieByName(String name, Cookie[] cookies){
        if (name == null || "".equals(name) || cookies == null || cookies.length == 0){
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                cookie.setValue("hiLLQ");
                return cookie;
            }
        }
        return null;
    }
}

//	注意：如果希望我们的浏览器本地的 cookie 也修改，则需要使用
if (cookie != null){
	response.addCookie(cookie);
}
```

### 5. 将一个字符串数字，转成 int

```java
public static int parseInt(String str, int defaultVal){
    try {
        return Integer.parseInt(str);
    } catch (NumberFormatException e) {
        System.out.println(str + "格式不对，转换失败");
    }
    return  defaultVal;
}
```

### 6. 判断一个文件是否是 html

```java
public static boolean isHtml(String uri){
    return uri.endsWith(".html");
}
```

### 7. 根据文件名来读取文件 ---> String

```java
//  根据文件名来读取该文件 ---> String
public static String readHtml(String filename) {
    String path = WebUtils.class.getResource("/").getPath();
    StringBuilder stringBuilder = new StringBuilder();
    try {
        BufferedReader br = new BufferedReader(new FileReader(path + filename));
        String buf = "";
        while ((buf = br.readLine()) != null) {
            stringBuilder.append(buf);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
    return stringBuilder.toString();
}
```

## 字符串相关操作

### 1. 字符串拼接

先占位

```java
String mes = HspResponse.respHeader + "<h1>num1+num2=sum</h1>";
```

然后将其中一个删掉，并在左右两侧添加逗号

```java
String mes = HspResponse.respHeader + "<h1>"+num1+"+num2=sum</h1>";
```

```java
String mes = HspResponse.respHeader + "<h1>"+num1+"+"+num2+"=sum</h1>";
```

```java
String mes = HspResponse.respHeader + "<h1>" + num1 + " +" +num2 + " = " + sum + "</h1>";
```



### 2. 判断字符串结尾是否是特定字串

```java
return uri.endWith(".html");
```

### 3. 读取子串

```java
uri.substring(1);
```

### 4. String.format() 格式化输出

```java
//  思路：讲结果组装到一个字符串中，方便我们在下一个页面显示
String resInfo = String.format("%s %s %s = %s", num1, symbol, num2, res);
request.setAttribute("resInfo", resInfo);
```

### 5. 标题不超过15个字

```java
if (title.length() > 15){
    title = title.substring(0, 15) + "...";
}
```

### 6. Object ---> String

```java
Object visit_count = servletContext.getAttribute("visit_count");
visit_count = Integer.parseInt(visit_count+"") + 1;
servletContext.setAttribute("visit_count", integer);	//	类似 hashMap 修改（替换）
```



## 显示编译后的文件夹

target 目录查看与隐藏

show Excluded Files



## Package 和 Directory

![image-20221124205924621](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211242059073.png)

只有 Sources Root 目录才可以创建 package，其它只能创建目录 directory

## Return

1. 没有返回值的方法中：如果触发了 return，那么后面的语句就都不执行了

## Debug技巧

- 快捷键修改 Appearance & Behavior ---> Keymap，然后搜索 debug
- 改为 Alt + D
- F9（Resume Program）：断点跳转

## 复制过来的文件 IDEA 找不到

Build ---> Rebuild Project

## 并行执行

模拟多个用户登录

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211291949568.png)

Edit Configurations

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211291949731.png)

勾选 Allow parallerl run

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211291950606.png)

## 异常捕获

- Ctrl + Alt + T：try-catch-finally 捕获异常



## 插件

1. LeetCode插件

   - 下载 LeetCode Editor插件

   - File ---> Settings ---> Tools ---> leetcode plugin

   - 参数说明：

     Custom code template: 开启使用自定义模板，否则使用默认生成格式 CodeFileName: 生成文件的名称，默认为题目标题 

     CodeTemplate: 生成题目代码的内容，默认为题目描述和题目代码 

     TemplateConstant： 模板常用变量

     ${question.title}：题目标题，例如:两数之和
     $
   
     {question.titleSlug}：题目标记，例如:two-sum
     ${question.frontendQuestionId}：题目编号，例如:1
   
     ${question.content}：题目描述内容
     $

     {question.code}：题目代码部分
     $!velocityTool.camelCaseName(str)：一个函数，用来将字符串转化为驼峰样式
   
   - 配置
   
     - CodeFileName这个里面填的就是以后自动生成类的类名
   
       ```shell
       $!velocityTool.camelCaseName(${question.titleSlug}) 
       ```
   
     - CodeTemplate：自动生成的代码格式
   
       ```java
       package leetcode.editor.cn;
       
       ${question.content}
       public class $!velocityTool.camelCaseName(${question.titleSlug}){
       	public static void main(String[] args) {
       		Solution solution = new $!velocityTool.camelCaseName(${question.titleSlug})().new Solution();
       		
       	}
       ${question.code}
       }
       ```
   
       ![image-20221108171714048](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211081717109.png)

​	新版的谷歌浏览器根本就抓不到 Cookie，微软Edge倒是可以抓到，但是没用

​	关于插件的问题，是由于 LeetCode域名更换了，是需要升级到 8.1版本，但是我的IDEA版本匹配不上，就先不折腾了。

​	最后解决方法：更改临时配置文件

- 修改临时变量

  - 打开 C:\Users\李隆齐\AppData\Roaming\JetBrains\IntelliJIdea2020.1\options 

  - 将里面的`leetcode-cn.com`字段替换成`leetcode.cn`

## 背景图片设置

使用 Alt + B 进行设置

![image-20221120111805687](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211201118799.png)



## JUnit 单元测试框架

- @Test：Alt + Enter
- 先写方法，再加上@Test



## UML图相关快捷键

- 查看源码：F4

- 在UML图上方可以查看 ：
  - 字段
  - 构造器
  - 方法
- 使用空格来搜索并添加类
- Show Dependencies：体现在局部变量，方法的参数，或者对静态方法的调用

## 虚拟机

### 1. 本地客户端与虚拟机交互

查看服务器端口连接

```ba
netstat -anutp
```

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211292037888.png)

虽然客户端和服务器都哟 User，但是路径不同（交互的时候会带着路径）

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211292038789.png)

所以，客户端发送给服务器端时，服务器无法识别！！！

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202211292040652.png)

而左边强制转换的 User，则是当前目录的，所以目录要改为和客户端一致才可以。

## Maven 

### 一、IDEA创建项目

1. 利用 maven 配置 web 项目

   Archetype（模板原型）

   - cocoon-22-archetype-webapp![image-20221110153017416](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101530458.png)

   - maven-archetype-quickstart![image-20221110153035109](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101530150.png)
   - maven-archetype-webapp![image-20221110153116451](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101531491.png)

   https://maven.apache.org/archetypes/maven-archetype-webapp/

   1. project
   2. |-- pom.xml
   3. `-- src
   4. ​    `-- main
   5. ​        `-- webapp
   6. ​            |-- WEB-INF
   7. ​            |   `-- web.xml
   8. ​            `-- index.jsp

   ### Usage

   To generate a new project from this archetype, type:

   mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4![image-20221110154811820](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101548872.png)

2. Artifact Coordinates【<font color="blue">GroupId</font> 和 <font color="blue">ArtifactId</font> 统称为<font color="red">坐标</font>：可以唯一标识一个项目】

   - GroupId：公司信息
     - 一般是 com.公司名称

   - ArtifactId：项目名称
     - 通常就是项目名

3. 指定 maven

   	- 这里使用 IDEA自带的 maven: D:/Program Files (x86)/JetBrains/IntelliJ IDEA 2020.1.2/plugins/maven/lib/maven3

4. User setting file

   - Maven配置文件（默认是国外的仓库）---> 在 settings.xml 中配置 maven 镜像

5. Local repository

   - 本地仓库：从 maven 仓库下载的包，放到哪里



### 二、配置阿里云镜像

![image-20221109204435132](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211092044343.png)

IDEA中自带的 settings.xml

```bash
D:/Program Files (x86)/JetBrains/IntelliJ IDEA 2020.1.2/plugins/maven/lib/maven3
```

![image-20221109204724753](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211092047814.png)

将这个 settings.xml 文件复制过来

```bash
C:\Users\李隆齐\.m2\settings.xml
```

修改配置

https://developer.aliyun.com/mvn/guide

```xml
 <mirrors>
		<mirror>
			<!-- 指定镜像ID（可以自己改名） -->
			<id>nexus-aliyun</id>
			<!-- 匹配中央仓库（阿里云的仓库名称，不可以自己起名，必须这么写）-->
			<mirrorOf>central</mirrorOf>
			<!-- 指定镜像名称（可以自己改名）-->
			<name>Nexus aliyun</name>
			<!-- 指定镜像路径（镜像地址）-->
			<url>https://maven.aliyun.com/nexus/content/repositories/central</url>
		</mirror>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
  </mirrors>
```

### 三、使用 maven 引入 jar 包

在pom.xml中配置依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.wangyi</groupId>
  <artifactId>aclq_tomcat</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>aclq_tomcat Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <!--
        老韩解读
        1.  引入 servlet-api.jar，为了开发 servlet
        2.  dependency 标签是标识要引入一个包
        3.  groupId：包的开发公司相关信息
        4.  artifactId：项目名  javax.servlet-api
            补充：groupId + artifactId 是以目录形式
        5.  version 版本
        6.  scope 表示引入的包的作用范围
        7.  provided：表示 tomcat 本身有jar 包，这里你引入的 jar 包，在编译，测试是有效，但是在打包发布的时候，不要带上这个 jar 包
        8.  下载的包在你指定的目录： C:\Users\李隆齐\.m2\repository
        9.  可以去修改我们要下载包的位置
        10. 我们可以去指定 maven 仓库，即：配置maven镜像
      -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>aclq_tomcat</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

下载的包在你指定的目录 C:\Users\李隆齐\.m2\repository

![image-20221110003531470](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211100035531.png)

Reload All Maven Projects

![image-20221110003857269](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211100038310.png)

现在已经下载到本地了，那么该怎么证明你的 Java 项目 在用 它呢？

![image-20221110004337252](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211100043313.png)

![image-20221110004359342](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211100043386.png)

### 四、实现效果

在maven里面开发 servlet，需要新建项目

1. main ---> new ---> Directory ---> java

2. 刷新下 Maven，Crete New Servlet ![image-20221110161513517](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101615556.png)

3. 静态资源放在 webapp 目录下

4. 在 llq 下新建一个包 utils，将 string ---> int

   ```java
   package com.aclq.utils;
   
   /**
    * @Author: Ronnie LEE
    * @Date: 2022/11/10 - 11 - 10 - 11:40
    * @Description: com.llq.utils
    * @version: 1.0
    */
   public class WebUtils {
       /**
        * 将一个字符串数字，转成int，如果转换失败，就返回自己传入的 defaultVal
        * @param str
        * @param defaultVal
        * @return
        */
       public static int parseInt(String str, int defaultVal){
           try {
               return Integer.parseInt(str);
           } catch (NumberFormatException e) {
               System.out.println(str + "格式不对，转换失败");
           }
           return  defaultVal;
       }
   }
   
   ```

   

5. 配置 Tomcat

   - 注意：新配置 tomcat 时，选择 ➕ 进行添加

     ![image-20221110144326971](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101443015.png)

   - add Configuration ---> Tomcat Server ---> Local ---> F:\apache-tomcat-8.0.50
   - Deployment ---> Artifact ---> llq_tomcat:war exploded
   - Application context只保留：/![image-20221110115947669](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/LeetCode/202211101159750.png)

   - 配置热加载





## Java_EE

### 1. 给浏览器返回信息

乱码问题解决：

```java
request.setCharacterEncoding("utf-8");   
 
response.setContentType("text/html;charset=utf-8");
```



```java
//  给浏览器返回信息
response.setContentType("text/html;charset=utf-8");
PrintWriter writer = response.getWriter();
writer.println("<h1>完成读取 cookie信息任务</h1>");
writer.flush();
writer.close();
```

### 2. login.html 登录界面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
<form action="/cs/loginCheck">
    <h1>用户登录界面</h1>
    用户名：<input type="text" name="username"></br></br>
    密码：<input type="password" name="password"></br></br>
    <input type="submit" value="登录">
	评论：<br><textarea name="review" style="width: 300px; height: 100px;"></textarea>
</form>
</body>
</html>
```

- 当 type="text" 时，name 的值就是：key，我们输入的值为：value
- 当 type="password" 时，输入的为星号
  - key: password
  - value：我们输入的值

- 当 type="summit"时，我们只能点击提交（不能输入），所以只有 value（显示的文字）

### 3. DOM绑定

#### 3.1 静态绑定

通过html 标签的 <font color="yellow">事件属性 </font>直接赋于事件响应后的代码，这种方式叫静态注册

#### 3.2 动态绑定

widows.onload() 作用

- window.onload() 方法用于在网页加载完毕后立刻执行的操作，即当 HTML 文档加载完毕后，立刻执行某个方法。
- window.onload() 通常用于 元素，在页面完全载入后(包括图片、css文件等等)执行脚本代码。

##### 为什么使用 windows.onload()?

因为 JavaScript 中的函数方法需要<在 HTML 文档渲染完成后才可以使用，如果没有渲染完成，此时的 <font color="yellow">DOM 树是不完整 </font>的，这样在调用一些 JavaScript 代码时就可能报出"undefined"错误。

我们都知道页面的代码顺序是 <font color="red">从上往下</font> 进行加载，很多时候我们要对页面中的某一个模块进行操作，这时候我们常常使用javascript代码来进行操作。为了能够保证操作的模块或对象在js代码之前就加载了，我们不得不把js代码放在页面的底端。

但是我们在设计页面的时候，为了把js代码放在一起，或者一个让页面更加简洁的位置，那就有可能出现代码中操作的对象未被加载的情况，那么我们该如何去解决呢？这时候 <font color="yellow">window.onload</font> 就被有了存在的意义了。

##### windows.onload 是什么?

window.onload是一个 <font color="yellow">事件</font>，在文档加载完成后能立即触发，并且能够为该事件注册事件处理函数。将要对对象或者模块进行操作的代码存放在处理函数中。即：window.onload =function (){这里写操作的代码};

##### 例子

###### 1. 对对象操作，但是对象未被加载，操作失败

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">

    document.getElementById("s").style.color = "green";

    </script>
</head>
<body>

<span id="s">ACLQ！！！</span>
</body>
</html>
```

![image-20221228110202955](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281102080.png)

###### 2. 使用 windows.onload() 方法处理

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function(){
            document.getElementById("s").style.color = "red";
        }

    </script>
</head>
<body>

<span id="s"> ACLQ！！！</span>
</body>
</html>
```

![image-20221228110729412](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281107466.png)

### 4. 将 login.html 改造成 servlet

因为 login.html 没有办法动态地去获取 value 值，所以要改造成 Servlet

- UserUIServlet：负责界面
- Login：信息校验
  - 接收用户输入
  - 判断该用户是否合法
  - 并返回提示信息

##### UserUISerlvet：负责界面

```java
/**
 * 界面
 */
public class UserUIServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("userUI 被访问! ");
        //  得到 writer
        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println("<!DOCTYPE html>\n" +
                "<html lang=\"en\">\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "    <title>登录</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<form action=\"/cs/login\">\n" +
                "    <h1>用户登录界面</h1>\n" +
                "    用户名：<input type=\"text\" name=\"username\"></br></br>\n" +
                "    密码：<input type=\"text\" name=\"password\"></br></br>\n" +
                "    <input type=\"submit\" value=\"登录\">\n" +
                "</form>\n" +
                "</body>\n" +
                "</html>");
        writer.flush();
        writer.close();
    }


    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}
```

##### LoginServlet 验证回送 Cookie

```java
//	验证，并给 浏览器传送 cookie
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //   1. 判断是否登录成功?
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();
        if (username != null && password != null){
            if ("llq".equals(username) && ("123456".equals(password))){
                //  将登录信息成功的用户名，以 cookie 形式，保存到浏览器

                Cookie cookie = new Cookie("username", "llq");
                cookie.setMaxAge(60 * 60 * 24 * 3); //  3 天后失效
                response.addCookie(cookie);
                //  给浏览器返回信息
                writer.println("<h1>恭喜你，登录成功</h1>");
            }else {
                //  给浏览器返回信息
                writer.println("<h1>用户或者密码信息不对，请重试！</h1>");
            }
            writer.flush();
            writer.close();
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}

```

然后，浏览器再次访问登录页面 UI 的时候，要读取 cookie

正常value格式：
```java
value = "xxx";
```

如果直接这么写，相当于双引号没有了

```java
value=xxx
```

```java
"    用户名：<input type=\"text\" name=\"username\"></br></br>\n" +
```

要改为：

```java
"    用户名：<input type=\"text\" value=\"" + value + "\"name=\"username\"></br></br>\n" +
```

```java
\"：引号转义符
```

![image-20221204200147456](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212042001548.png)

在一堆混乱的字符中插入一个变量：

- 中间截断：插入2个粉色引号
- 由于 value = "×××" 形式
- 使用 / " 进行转义

##### UserUIServlet：浏览器再次访问登录页面 UI 的时候，要读取 cookie

```java
public class UserUIServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("userUI 被访问! ");

        //  读取从浏览器发送来的 cookie
        Cookie[] cookies = request.getCookies();
        Cookie username = CookieUtils.readCookieByName("username", cookies);
        String value = "";
        if (username != null){
            value = username.getValue(); //  取出登录成功的名字
            System.out.println(value);
        }


        //  得到 writer
        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println("<!DOCTYPE html>\n" +
                "<html lang=\"en\">\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "    <title>登录</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<form action=\"/cs/login\">\n" +
                "    <h1>用户登录界面</h1>\n" +
                "    用户名：<input type=\"text\" value=\"" + value + "\"name=\"username\"></br></br>\n" +
                "    密码：<input type=\"text\" name=\"password\"></br></br>\n" +
                "    <input type=\"submit\" value=\"登录\">\n" +
                "</form>\n" +
                "</body>\n" +
                "</html>");
        writer.flush();
        writer.close();

        RequestDispatcher requestDispatcher = request.getRequestDispatcher("/cs/login");
        requestDispatcher.forward(request, response);
    }


    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}

```



### 5. Servlet

#### 动态绑定(方法有，属性没有)

注意：

1. 当调用对象的<font color="yellow">方法</font>时，该方法会和对象的 内存地址 / <font color="yellow">运行类型</font> 绑定
2. 当调用对象的属性时，没有动态绑定机制，哪里声明，哪里使用

![image-20221217152242582](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171522684.png)

#### 5.1 ServletConfig

![image-20221217170132335](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171701439.png)

```java
   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //	这个 servletConfig 对象是由 Tomcat创建的
        ServletConfig servletConfig = getServletConfig();
        String username = servletConfig.getInitParameter("username");
        String pwd = servletConfig.getInitParameter("pwd");
        System.out.println("初始化参数 user = " + username);
        System.out.println("初始化参数 pwd = " + pwd);
    }
```



![image-20221217150758505](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171507628.png)

1. Servlet程序默认是第1次访问的时候创建

2. ServletConfig 在 Servlet 程序创建时，就创建一个对应的 ServeltConfig 对象

   ![image-20221217150926248](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171509405.png)

 看到，父类 GenericServlet中有 getServletConfig() 方法

![image-20221217151026158](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212171510346.png)

```java
//	GenericServlet中的 ServletConfig config字段，用于保存 tomcat创建的 ServletConfig 对象
private transient ServletConfig config;
```

```java
//	getServletConfig() 方法，获取 ServletConfig config 字段
public ServletConfig getServletConfig() {
    return this.config;	//	返回当前类的 config 属性【属性没有动态绑定】
}
```

前面说了，当浏览器访问时 Serlvet时

- tomcat通过反射机制，实例化Serlvet
- 再创建一个对应的 ServeltConfig 对象

##### init（ServletConfig config）方法

通过该方法，将 Tomcat 创建的 ServletConfig 对象赋值给 GenericServlet 的 config 属性

```java
public void init(ServletConfig config) throws ServletException {
    this.config = config;
    this.init();
}
```

最后 通过 getServletConfig() 方法，获取 config 字段 ===> 就是 Tomcat 创建的 ServletConfig 对象

- 体现了 <font color="yellow">封装性</font>。

注意，如果在 Servlet 中重写了 init(ServletConfig config)方法的话，要调用 super.init(ServletConfig config)方法，实现将 Tomcat创建的ServletConfig对象赋值给 GenericServlet 中的config字段，否则通过 getServletConfig() 方法获取到的 config字段就一直为 null。

```java
//	将 Tomcat 创建的 ServletConfig 对象给到了 GenreciServlet 里面的 config 属性
@Override
public void init(ServletConfig config) throws ServletException {
    super.init(config);	//	中间桥梁作用
}
```

#### 5.2 ServletContext

![image-20221217235429764](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212172354898.png)

1. ServletContext 是一个接口，它表示 Servlet 上下文对象

2. 一个 web 工程，只有一个 ServletContext 对象实例

3. ServletContext 对象是在 web 工程启动的时候创建，在 web工程停止时销毁

4. ServletContext对象获取方式：

   - ServletConfig.getServeltContext() 方法获得对 ServletContext 对象的引用

   - 通过 this.getServletContext() 来获得其对象的引用

     ![image-20221217235635496](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212172356624.png)

   ![image-20221218001523685](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212180015959.png)

5. 由于一个 Web应用中的所有 Servlet 共享同一个 ServletContext 对象

   - 因此 Servlet 对象之间可以通过 ServletContext 对象来实现多个 Servlet 间通讯

   - ServletContext对象通常也被称之为域对象

     ![image-20221217235928547](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212172359714.png)

##### ServletContext 可以做什么

```java
 ServletContext servletContext = getServletContext();
```

1. 获取 web.xml 中配置的上下文参数 <font color="yellow"> context-param</font> 【信息和 <font color="red"> 整个web 应用</font> 相关，而不是属于某个 Servlet】

   ```java
   //  2. 获取 website
   String website = servletContext.getInitParameter("website");
   String company = servletContext.getInitParameter("company");
   System.out.println("website= " + website + "\n" + "company= " + company);
   ```

2. 获取当前的<font color="yellow"> 工程路径</font> ，格式：/ 工程路径 ===> 比如：/serlvet

   ```java
   //  3. 获取项目的工程路径
   String contextPath = servletContext.getContextPath();
   System.out.println("项目路径：" + contextPath);  //  /servlet
   ```

3. 获取工程部署后在服务器硬盘上的<font color="yellow"> 绝对路径</font>（更好地去定位资源），比如：

   ```java
   //  4. 获取项目发布后真正的工作路径
   //  / 表示我们的项目（发布后）的根路径
   //  E:\Tom\servlet\out\artifacts\servlet_war_exploded
   String realPath = servletContext.getRealPath("/");
   System.out.println("项目发布后的结对路径= " + realPath);
   ```

   ![image-20221218000955494](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212180009634.png)

4. 像 Map 一样存取数据，多个 Servlet 共享数据

### 6. cookie处理

#### 查找

```java
//	查找
public class CookieUtils {

    //  编写一个方法，返回指定名字的 cookie 值
    public static Cookie readCookieByName(String name, Cookie[] cookies){
        //  判断传入的参数是否正确
        if (name == null || "".equals(name) || cookies == null || cookies.length == 0){
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())){ //  如果名字相同
                return cookie;
            }
        }
        //  如果遍历完了之后，还是没有，就返回 null
        return null;
    }
}
```

#### 修改

```java
//	修改
public class UpdateCookie {
    public static Cookie updateCookieByName(String name, Cookie[] cookies){
        if (name == null || "".equals(name) || cookies == null || cookies.length == 0){
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                cookie.setValue("hiLLQ");
                return cookie;
            }
        }
        return null;
    }
}

//	注意：如果希望我们的浏览器本地的 cookie 也修改，则需要使用
if (cookie != null){
    response.addCookie(cookie);
}
```

#### 中文处理

存放中文 cookie，默认报错，可以通过 url 编码和解码来解决，不建议存放中文 cookie 信息

```java
public class EncoderCookie extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("encoder 被调用");
        Cookie cookie = new Cookie("name", "爱新觉罗LQ");
        response.addCookie(cookie);
        //  给浏览器返回信息
        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println("<h1>设置中文 cookie 成功</h1>");
        writer.flush();
        writer.close();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}

```

![image-20221205105031591](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212051050175.png)

##### URL 编码

- 如果直接存放中文的 cookie，会报错

  ```java
  //	报错
  HTTP Status 500 - Control character in cookie value or attribute.
  ```

- 解决方法：将中文 编成 URL 编码

  ```java
  // URLEncoder 工具类
  ```

  将 中文 转成一个 基于 utf-8 的 url 编码

  ```java
  String aclq = URLEncoder.encode("爱新觉罗LQ", "utf-8");
  Cookie cookie = new Cookie("name", aclq);
  ```

- 编码后，再保存即可

  ![image-20221205110105358](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212051101459.png)

```bash
Set-Cookie: name=%E7%88%B1%E6%96%B0%E8%A7%89%E7%BD%97LQ
```

使用工具可以来进行解码

![image-20221205110313543](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212051103654.png)

##### URL 解码

在读取 中文 cookie 时，进行解码

```java
public class ReadCookie2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("read2 被调用");
        Cookie[] cookies = request.getCookies();
        Cookie name = CookieUtils.readCookieByName("name", cookies);
        String value = name.getValue();
        //  解码
        String decode = URLDecoder.decode(value, "utf-8");
        System.out.println("解码后的 value= " + decode);

        //  给浏览器返回信息
        response.setContentType("text/html;charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println("<h1>完成读取中文 cookie信息 解码成功</h1>");
        writer.flush();
        writer.close();

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}
```

### 7. jsp简易计算器（JS + 正则表达式）

用户可以提交数据，并完成校验

通过 id 来获取元素，再通过 .value 取出对应值

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jsp版本计算器</title>
    <%--使用 js + 正则表达式完成数据校验--%>
    <script type="text/javascript">
        function check() {
            //  得到 num1 和 num2的值
            let num1 = document.getElementById("num1").value;
            let num2 = document.getElementById("num2").value;

            //  验证：正则表达式
            let reg = /^[-]?([1-9]\d*|0)$/;
            if (!reg.test(num1)){ // 如果不满足验证体哦阿健
                alert("num1 不是一个整数");
                return false;   //  我就不提交了
            }
            if (!reg.test(num2)){
                alert("num2 不是一个整数");
                return false;
            }
            return true;
        }
    </script>
</head>
<body>
<h1>jsp版本-计算器</h1>
<form action="/jsp/cal_server.jsp" onsubmit="return check()">
    num1：<input type="text" id="num1" name="num1"><br/><br/>
    num2: <input type="text" id="num2" name="num2"><br/><br/>

    运算符号:
    <select name="symbol">
        <option>--选择--</option>
        <option selected value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
     </select><br/><br/>
     <input type="submit" value="计算">
</form>

</body>
</html>
```

### 8. EL表达式 （替代 jsp表达式<%=表达式%>）

本质：语法糖

- EL表达式在 输出 null 时，输出的是 ""
- jsp 表达式脚本输出 null 时，输出的是 "null" 字符串

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>el表达式快速入门</title>
</head>
<body>
<%--
    解读：
    1.  如果 name 是 null，request.getAttribute() 返回的是 null 字符串 【显示有些难看】
    2.  如果 name 是 ${name} 返回的是 ""
    //  进一步理解：相当于
    <%request.getAttribute("name") == null? "" : request.getAttribute("name")%>
--%>

<%
    request.setAttribute("name", "爱新觉罗LQ");
%>
<h1>jsp 表达式脚本</h1>
名字= <%=request.getAttribute("name")%><br>
<h1>el 表达式</h1>
名字= ${name}<br>
</body>
</html>
```

作业优化：

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 李隆齐
  Date: 2022/12/8
  Version: 1.0
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jsp版本计算器</title>
    <%--使用 js + 正则表达式完成数据校验--%>
    <script type="text/javascript">
        function check() {
            //  得到 num1 和 num2的值
            let num1 = document.getElementById("num1").value;
            let num2 = document.getElementById("num2").value;

            //  验证：正则表达式
            let reg = /^[-]?([1-9]\d*|0)$/;
            if (!reg.test(num1)){ // 如果不满足验证体哦阿健
                alert("num1 不是一个整数");
                return false;   //  我就不提交了
            }
            if (!reg.test(num2)){
                alert("num2 不是一个整数");
                return false;
            }
            return true;
        }
    </script>
</head>
<body>

<h1>jsp版本-计算器</h1>
<form action="/jsp/calServlet" onsubmit="return check()">
    num1：<input type="text" id="num1" name="num1">${num1_error}<br/><br/>
    num2: <input type="text" id="num2" name="num2">${num2_error}<br/><br/>

    运算符号:${symbol_error}
    <select name="symbol">
        <option>--选择--</option>
        <option selected value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
     </select><br/><br/>
     <input type="submit" value="计算">
</form>

</body>
</html>
```



### 9. 通过 Reploy 来清掉 Session

![image-20221215005517077](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212150055179.png)

![image-20221215005545177](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212150055252.png)

### 10. jstl（替代jsp的代码脚本<%%>） 实现表格展示

使用 forEach 循环获取

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>展示</title>
</head>
<body>
<h1>展示</h1>
<table border="1px" width="400px">
    <tr>
        <td>id</td>
        <td>name</td>
        <td>skill</td>
    </tr>
    <c:forEach items="${requestScope.monster}" var="guaiwu">
        <c:if test="${guaiwu.id > 100}">
            <tr>
                <td>${guaiwu.id}</td>
                <td>${guaiwu.name}</td>
                <td>${guaiwu.job}</td>
            </tr>
        </c:if>
    </c:forEach>
</table>
</body>
</html>
```



