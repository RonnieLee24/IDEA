# 	HSP【算法】

## 1. 二分查找（非递归）BS

- 有序
- 运行时间为对数时间  $log_{2}n$

数组：

```bash
{1, 3, 8, 10, 11, 67, 100}
```

```java
package com.alq.binarySerarch;

public class binarySearchDemo {
    public static void main(String[] args) {
        int[] arr = {1, 3, 8, 10, 11, 67, 100};

        int index = binarySearch(arr, 24);
        System.out.println(index);

    }

    public static int binarySearch(int[] arr, int target){
        int l = 0;
        int r = arr.length;
        while (l <= r){
            int mid = l + (r - l) / 2;
            if (arr[mid] < target){
                l = mid+1;
            }else if (arr[mid] > target){
                r = mid - 1;
            }else {
                return mid;
            }
        }

        return -1;
    }
}
```



## 2. 分治算法 (Divide-and-Conquer Algorithm) DAC

应用场景：

- 快速排序
- 归并排序
- 傅里叶变化
- 二分搜索
- 大整数乘法
- 最接近点对问题
- 循环赛程表
- 汉诺塔

分治法在每一层递归上都有 3 个 步骤：

1. 分解：将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题
2. 解决：若子问题规模较小，而容易被解决则直接解决，否则递归地解各个子问题
3. 合并：将各个子问题的解合并为原问题的解

经典案例：汉诺塔（Hanoi）

思路分析：

1. 如果只有一个盘：A --> C

2. 如果 n > 2，我们总是可以看成是 2 个盘子

   1. (n - 1)个盘子【一个整体】

   2. max

   - 将 (n- 1) A --> B

   - 将 max A --> C

   - 将 B 的所有盘：B --> C

```java
package com.alq.divideAndConquer;

public class HanoiDemo {
    public static void main(String[] args) {
        Hanoi(5, 'A', 'B', 'C');
    }
    /**
     * @param n 个数
     * @param a 起点
     * @param b 桥梁
     * @param c 终点
     */
    public static void Hanoi(int n, char a, char b, char c){
        //  1. n = 1, A --> C
        //  2. n > 1, A 中看作是2个盘子：1）上面的 n - 1 个 2）max
        //      2.1 将 (n - 1) A --> B
        //      2.2 将 max A --> C
        //      2.4 (n- 1) B ---> C
        if (n == 1){
            System.out.println(a + "-->" + c);
            return;
        }
        //  2.1
        Hanoi(n - 1, a, c, b);  //  a 通过 c 移动到 b
        //  2.2
        System.out.println(a + "-->" + c);  //  将 max 从 A 移动到 C
        //  2.3
        Hanoi(n - 1, b, a, c);
    }
}
```

## 3. 动态规划 (Dynamic programming)  DP

### 3.1 递归与递推

#### 1. 递归【未知 ---> 已知】

- 将未知的问题缩小
- 直到已知的问题
- 便开始返回值
- 最后解决我们的问题

#### 2. 递推【已知 ---> 未知】

- 由已知的问题
- 向前推位置
- 一步步推算到我们最后要解决的问题上

兔子问题：

假定一对大兔子每月能生对小兔子，每对新生的小兔子

- 经过一个月可以长成一对大兔子具备繁殖能力
- 如果不发生死亡，且每次均生下一雌一雄

问：最初有一对小兔子，1 年后共有多少对兔子？

| 月份 | 0（初始态） | 1    | 2    | 3    | 4    | 5    | 6    | 7    | ……   |
| ---- | ----------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 小   | 1           | 0    | 1    | 1    | 2    | 3    | 5    | 8    | ……   |
| 大   | 0           | 1    | 1    | 2    | 3    | 5    | 8    | 13   | ……   |
| 总   | 1           | 1    | 2    | 3    | 5    | 8    | 13   | 21   | ……   |

2个要点：

![mmq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241349581.png)

##### Fibonacci数列

$$
f(n)=\left\{\begin{matrix}
1 & n = 0 & \\ 
1 & n = 1 & \\
f(n-1)+f(n-2) & n >1
\end{matrix}\right.
$$

递归

```java
public void Fibonacci(int n){
    //	base case
    if(n == 0 || n == 1){
        return 1;
    }
    return Fibonacci(n-1) + Fibonacci(n-2);
}
```

递推

用已知推未知，所以我们直接定义一个数组，用循环来完成

```java
public class ditui {
    public static void main(String[] args) {
        int[] arr = new int[100];
        arr[0] = 1;
        arr[1] = 1;
        for (int i = 2; i < arr.length; i++) {
            arr[i] = arr[i - 1] + arr[i - 2];
        }
        
        Scanner scanner = new Scanner(System.in);
        System.out.print("输入月份（1-100）：");
        String next = scanner.next();
        int i = Integer.parseInt(next);
        System.out.println("第 " + i + " 月 兔子对数为：" + arr[i]);
    }
}
```

##### 三角形最小路径和（120）

关注点：

- 顶部到底边
- 路径上数字之和最小
  - 左下
  - 右下
- [i, j] 的下一个相邻节点为：[i + 1, j]，[i + 1, j + 1]

![image-20230424154055818](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241540874.png)

![image-20230424160135030](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241601120.png)

![image-20230424160324447](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241603539.png)

```java
//	思路
//	a(i, j) 第 i 行，第 j 列数
//	s(i, j) 从 a(i, j )到底边的最大路径值
```

$$
S(i,j)=\left\{\begin{matrix}
a(i, j) & i=n & \\ 
a(i,j) + Math.max(S(i+1,j)), S(i+1,j+1)) & i< n  & 
\end{matrix}\right.
$$

但是这种算法：每个节点会大量地重复访问，时间复杂度为 $O(2^{n})$

![image-20230424160612030](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241606090.png)

我们使用记忆数组：进行算法优化

##### 记忆数组

时间复杂度： $O(2^{n})$ --->  $O(n^{2})$【最后复杂度就是遍历 2维数组了】

记忆化递归：

- 每访问一个节点，就把它记录下来
- 新开一个数组 record(i, j)
  - 如果没有记录，就记录
  - 否则，直接取出

```java
class Solution {
	//	定义一个 记忆数组 memory
	Integer[][] memory;
    public int minimumTotal(List<List<Integer>> triangle) {
    	memory = new Integer[triangle.size()][triangle.size()];
		//	开始递归（将大三角划分为：顶点 + 2个子三角）
		return dfs(triangle,1, 1);	//	从第一个数，开始迭代

    }
	//	i: 行数 1 -【triangle.size()】
	//	j：列数 1 - [triangle.get[i].size()]
    public int dfs(List<List<Integer>> triangle, int i, int j){
		//	base case
		if (i == triangle.size()){	//	i 从 1 开始到达底边的时候
			return triangle.get(i - 1).get(j - 1);
		}
		if (memory[i][j] != null){
			return memory[i][j];
		}
		// 开始递归（将大三角划分为：顶点 + 2个子三角）
		return memory[i][j]=triangle.get(i - 1).get(j - 1) + Math.min(dfs(triangle, i + 1, j), dfs(triangle,i + 1, j + 1));
	}
}
```

递推方法：

三角形第 n 行有 n 个元素

| 实现方式 | 说明                                         |
| -------- | -------------------------------------------- |
| 递归     | 自顶向下（抽象问题到边界）【未知 ---> 已知】 |
| 递推     | 自底向上（边界到抽象）【已知 ---> 未知】     |

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int[][] dp;
		int n = triangle.size();
		dp = new int[n + 1][n + 1];    //	多1个空间是为了让初始值满足状态转移方程（数组默认值为：0）
		//	开始递推（自低向上，由已知推算未知）
		for (int i = n - 1; i >= 0; i--) {    //	从末行开始 (i, j)代表未知【第 i 行，第 j 列】，取值的时候要取 (i - 1)(j - 1)]
			for (int j = 0; j <= i; j++) {
				dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
			}
		}
		return dp[0][0];    //	返回第一行第一个
    }
}
```

### 3.2 DP

- 每个大问题的子问题都是最优的，所以才可以直接记录下来
- 在下次寻找子问题的最优解时，直接使用

与分治算法不同的是：

- 适合 dp 请求的问题，经分解得到的子问题往往不是互相独立的
- 即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行进一步求解

#### 1. 最优子结构

原问题的最优解包含子问题的最优解

#### 2. 状态转移方程

- 写状态转移方程的程序，一般以递推的方式实现
- 虽然时间复杂度和记忆化搜索一样
- 而且在循环中会计算一些无意义的节点
- 但是递推免去了调用时进栈出栈的操作，而且避免了在递归时层数过多时栈溢出的情况

$$
f(i,j)=max\left \{ f(i-1,j) ,f(i-1,j-w(i))+v(i)\right \}
$$

#### 3. 手办问题

| 手办名     | 价格 | 喜欢程度 |
| ---------- | ---- | -------- |
| 兵长       | 10   | 24       |
| 蕾姆       | 4    | 9        |
| 小埋       | 4    | 9        |
| 和泉纱雾   | 5    | 10       |
| 空条承太郎 | 3    | 2        |

你现在 有 <font color="red">**13**</font> 元

##### 3.1 贪心算法

- 算出每个物品的性价比
- 优先买性价比高的物品

| 手办名     | 价格                           | 喜欢程度 | 性价比 |
| ---------- | ------------------------------ | -------- | ------ |
| 兵长       | <font color="yellow">10</font> | 24       | 2.4    |
| 蕾姆       | 4                              | 9        | 2.25   |
| 小埋       | 4                              | 9        | 2.25   |
| 和泉纱雾   | 5                              | 10       | 2      |
| 空条承太郎 | <font color="yellow">3</font>  | 2        | 0.67   |

最终的喜欢程度为：24 + 2 = 26

##### 3.2 0 - 1 背包问题

| 背包问题                         | 说明             |
| -------------------------------- | ---------------- |
| 0-1 背包                         | 每个物品最多一个 |
| 完全背包 ==> 可以转化成 0-1 背包 | 每种物品无限件   |

手办是一个整体

- 你不能只买🐻和大腿，按照比例给钱，所以不能用贪心来解决

###### 普通递归实现

```bash
# f(i, j)：当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值

# max【不买这个商品的价值，买这个商品的价值】
```

$$
f(i,j)=max\left \{ f(i-1,j) ,f(i-1,j-w(i))+v(i)\right \}
$$

```java
public class jianchi {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  baseCase
        if (i == 0){    //  当没有物品，价值为 0
            return 0;
        }
        if (j < price[i - 1]){  //  买不起当前物品，就无需比较
            return f(i - 1, j);
        }
        //  不买第 n 个物品
        //  买第 n 个物品，总钱数就会减少，接下来就用 j - price[i - 1]钱考虑剩下的 i - 1个物品喽
        return Math.max(f(i - 1, j), f(i - 1, j - price[i - 1]) + value[i - 1]);	//	下标从 0 开始
    }
}
```

###### 递推（记忆化数组）

那么，这个 0/1 背包问题是否满足 <font color="yellow">最优子结构 </font>呢???

能不能用<font color="yellow"> 记忆化搜索</font> 来优化这一算法???

| 已知条件                               |                |
| -------------------------------------- | -------------- |
| $n$ 个物品                             | $T$ 元钱       |
| $W_{i}$ 价格                           | $V_{i}$ 喜欢值 |
| $X_{i}\epsilon \left \{ 0,1 \right \}$ | 是否买         |

这样问题可以转化为：
$$
max\sum_{i=1}^{n}V_{i}X_{i}\left ( \sum_{i=1}^{n}W_{i}X_{i}\leqslant T \right )
$$
证明：上述公式是具有 <font color="red">**最优子结构** </font>的。

- 设 $\left ( y_{1},y_{2}……y_{n} \right )$ 是 $max\sum_{i=1}^{n}V_{i}X_{i}$ 最优解
- 那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 是子问题 $max\sum_{i=2}^{n}V_{i}X_{i}\left ( \sum_{i=2}^{n}W_{i}X_{i}\leqslant T- W_{1}X_{1}\right )$
- 使用<font color="yellow"> **反证法**</font>
  - 假设子问题存在 $\left ( Z_{2},Z_{3}……Z_{n} \right )$ 是最优解，那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 就不是它的最优解
  - 则：$\sum_{i=2}^{n}V_{i}y_{i}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}$
  - 两边同时加上 $V_{1}y_{1}$
  - $\sum_{i=2}^{n}V_{i}y_{i}+V_{1}y_{1}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}+V_{1}y_{1}$ ===> 得出 $\left ( y_{1},y_{2}……y_{n} \right )$ 并不是大问题的最优解，最优解是 $\left ( y_{1},Z_{2}……Z_{n} \right )$
  - 就与我们最开始的设定相矛盾了
- 证明这个问题是有最优子结构的

使用记忆化搜索优化递推：

```java
public class jianchi {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  baseCase
        if (i == 0){    //  当没有物品，价值为 0
            return 0;
        }
        if (dp[i][j] != 0){
            return dp[i][j];
        }
        if (j < price[i - 1]){  //  买不起当前物品，就无需比较
            return dp[i][j] = f(i - 1, j);
        }
        //  不买第 n 个物品
        //  买第 n 个物品，总钱数就会减少，接下来就用 j - price[i - 1]钱考虑剩下的 i - 1个物品喽
        return dp[i][j] = Math.max(f(i - 1, j), f(i - 1, j - price[i - 1]) + value[i - 1]);
    }
        
}
```

记忆化搜索，时间复杂度：$O(n*t)$

- n：物品个数
- t：拥有的钱

下面用递推实现：

从已知 ---> 未知

- 即：物品数从1个开始计算
- 同时利用了数组未赋值时，初始值是0【我有 13 块钱，但是数组下标是从 0 开始的哦，所以大小要开成 14】

```java
package com.alq.dp;

public class Bag {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  递推实现
        //  遍历记忆数组
        for (int k = 1; k <= i ; k++) {    //  物品个数
            for (int l = j; l >= 0 ; l--) {   //  拥有的钱
                if ( l < price[k - 1]){
                    dp[k][l] = dp[k - 1][l];    //  买不起当前物品 【数组，未赋值时，默认值是 0】
                }else {
                    //	状态转移方程
                    dp[k][l] = Math.max(dp[k - 1][l], dp[k - 1][l - price[k - 1]] + value[k - 1]);
                }
            }
        }
        return dp[i][j];
    }
}
```

我们发现：当状态转移时

- 当前状态只与上个状态有关
- 只在 i - 1 或 i + 1 这一行中取需要的值
- 再前面状态的值便没有用了

定义一个数组，存放放入物品信息

| 手办名     | 价格 | 喜欢程度 |
| ---------- | ---- | -------- |
| 兵长       | 10   | 24       |
| 蕾姆       | 4    | 9        |
| 小埋       | 4    | 9        |
| 和泉纱雾   | 5    | 10       |
| 空条承太郎 | 3    | 2        |

![image-20230425103347747](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251033061.png)

然后逐步向上查询：

![image-20230425104234188](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251042354.png)

```java
int i = path.length - 1;
int j = path[0].length - 1;
while (i > 0 && j > 0){
    if (path[i][j] == 1){
        System.out.println("第 " + i +  " " + j + "个被选中");
        j -= price[i - 1];
    }
    i--;
}
```



```java
package com.alq.dp;

public class Bag {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];

    static int[][] path = new int[6][14];


    public static void main(String[] args) {
        System.out.println(f(5, 13));
        for (int[] ints : path) {
            for (int anInt : ints) {
                System.out.print(anInt + "\t");
            }
            System.out.println();
        }

        System.out.println("==================");


        for (int[] ints : dp) {
            for (int anInt : ints) {
                System.out.print(anInt+"\t");
            }
            System.out.println();
        }
        //  输出我们最后放入的是那些物品
        int i = path.length - 1;
        int j = path[0].length - 1;
        while (i > 0 && j > 0){
            if (path[i][j] == 1){
                System.out.println("第 " + i +  " " + j + "个被选中");
                j -= price[i - 1];
            }
            i--;
        }
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  递推实现
        //  遍历记忆数组
        for (int k = 1; k <= i ; k++) {    //  物品个数
            for (int l = j; l >= 0 ; l--) {   //  拥有的钱
                if ( l < price[k - 1]){
                    dp[k][l] = dp[k - 1][l];    //  买不起当前物品 【数组，未赋值时，默认值是 0】
                }else {
                    //	状态转移方程
                    // dp[k][l] = Math.max(dp[k - 1][l], dp[k - 1][l - price[k - 1]] + value[k - 1]);
                    if (dp[k - 1][l] < dp[k - 1][l - price[k - 1]] + value[k - 1]){ //  买
                        dp[k][l] = dp[k - 1][l - price[k - 1]] + value[k - 1];
                        //  将当前情况记录到 path
                        path[k][l] = 1;
                    }else {
                        dp[k][l] = dp[k - 1][l];
                    }
                }
            }
        }
        return dp[i][j];
    }
}
```



##### 滚动数组

于是空间复杂度可以优化为：

​	$O(n*t)=O(t)$

## 4. KMP算法（字符串搜索算法）

利用之前判定过的信息，通过一个 next 数组，保存模式串前后最长公共子序列长度，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间

![image-20230427131240837](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271312942.png)

这种情况下，原来<font color="yellow">绿色部分</font>就<font color="yellow">不能用了</font>，显然，我们想要缩小绿色的部分，还要满足最长公共子串的要求，也就像下面这样：

![image-20230427131259101](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271312200.png)

这个蓝色的部分，首先要<font color="yellow"> 内容相同</font>，其次还要<font color="yellow">位于深绿色部分的开头和结尾</font>，这不就是**深绿色部分的最长公共前缀子串**吗

即：更新 $j$ $=$ $next[j-1]$

- i：next 数组下标，从 1 开始
- j：最长公共前后缀，从 0 开始

```java
package com.kmp;

public class KMP {
    static List<Integer> list = new ArrayList<>();

    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDEdsafeABCDABD";
        String str2 = "ABCDABD";

        int[] next = kmpNext(str2);
        kmpSearch(str1, str2, next);
        System.out.println(list);
    }

    public static List<Integer> kmpSearch(String str1, String str2, int[] next){
        int m = str2.length();

        //  遍历
        for (int i = 0, j = 0; i < str1.length(); i++) {
            while (j > 0 && str1.charAt(i) != str2.charAt(j)){  //  如果不等了，就一直向前推进，找到上一个相等的位置
                j = next[j - 1];
            }

            if (str1.charAt(i) == str2.charAt(j)){
                j++;
            }

            if (j == m){
                list.add(i - m + 1);
                j = 0;  //  j 重置【匹配上了之后，j 重头开始匹配】
            }
        }
        return list;
    }

    //  获得一个模式串的 next 数组
    public static int[] kmpNext(String dest) {
        int[] next = new int[dest.length()];
        next[0] = 0;
        for (int i = 1, j = 0; i < dest.length(); i++) {
            while (j > 0 && dest.charAt(i) != dest.charAt(j)){
                j = next[j - 1];
            }

            if (dest.charAt(i) == dest.charAt(j)) {
                next[i] = ++j;
            }
        }
        return next;
    }
}
```



问题引出：找到 B 在 A 中的位置

![image-20230425110232870](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251102958.png)

暴力解法：$(n*m)$

- 每次匹配失败后
- i，j 都回溯，从头开始匹配【并没有从上次失败中，获取信息，并总结经验教训】

| KMP算法来源                                   |      |
| --------------------------------------------- | ---- |
| D.E.<font color="yellow">K</font>unth【美国】 | K    |
| J.H.<font color="yellow">M</font>orris        | M    |
| V.R.<font color="yellow">P</font>ratt         | P    |

### 1. KMP算法核心

1. 利用匹配失败后的信息
2. 尽量减少模式串（B）与主串（A）的匹配次数
3. 以达到快速匹配的目的
4. 通过一个 <font color="yellow">next 数组</font>，保存模式串（B）中 <font color="yellow">前后最长公共子序列的长度</font>，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间



### 2. 如何减少匹配次数

字符串的前缀和后缀

比如：字符串 ababacb

| 前缀集合                                        | 后缀集合                                        |
| ----------------------------------------------- | ----------------------------------------------- |
| $\left \{ a,ab,aba,abab,ababa,ababac \right \}$ | $\left \{ b,cb,acb,bacb,abacb,babacb \right \}$ |

- 暴力算法中：每次匹配失败后，ij都回溯，从头开始匹配
- KMP算法：不回溯 $i$，<font color="yellow">**只回溯**</font> $ j$ 到指定位置

当我们发现字符匹配失败时，这个失败的信息

1. 并不单单只代表失败，同时也代表着

   - 除了当前这 2个 字符不匹配

   - 前面的 j 个字符都匹配成功了

     ![image-20230425111737694](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251117784.png)

2. 就需要将 B 串后移（这时候有选择地移，移到有可能成功地位置）

   - B 串移动到 <font color="yellow">i 之前</font>

     - 需要满足 A后缀 与 B前缀 有交集时

       ![image-20230425112456735](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251124829.png)

       ![image-20230425115009063](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251150166.png)

     - 这时候，只需要将 j 回溯，然后再从 i 处开始匹配

       ![image-20230425115233431](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251152538.png)

       那么这个 B 串要后移多少位？【 j 指针回溯到哪个位置 ？】

       1. 找到 A 字串后缀 与 B字串前缀 的交集里：最长的那个元素。才能使B串后移得最少，且在已知信息中匹配的最多
       2. 最长元素的长度：就是 <font color="yellow">j</font> 指针 <font color="yellow">回溯的位置</font>
          - 前面讲过：<font color="red">匹配失败 </font>这个信息，代表着：<font color="yellow">前面的 j 个字符</font>是匹配成功了的，也即是说：<font color="yellow">A 子串与B子串是相同的</font>
          - 那么 “找到 A 子串后缀 与 B子串前缀的交集” <font color="red">等价于</font> “找到 B 子串后缀 与 B子串前缀的交集”
          - j 指针回溯的位置 = B 子串的最长公共前后缀
       3. 通过隐藏信息（匹配失败时，A串与B串存在着一段相同的子串），我们可以认为：
          - j 指针回溯的位置，<font color="yellow">只与 B 串有关</font>，与 A串无关
          - 在 A、B字符串匹配之前，就通过 B 串计算回溯位置，存在<font color="yellow">一个数组 next</font> 里【<font color="green">关键</font>】
          - 这个 next 与 B串形成映射数组，存的数据 next[ i] 就是 B[1] ~ B[i] 的最长公共前后缀的长度

   - B 串移动到 <font color="yellow">i 之后</font> 【即：B 没有最长公共前后缀】，肯定就不用回溯 i 了

### 3.  A【主串】、B【模式串】字符串匹配步骤

$i,j$ 初始化为 0

1. 如果 $A[i+1]=B[j+1]$，$i++$，$j++$ 继续匹配
2. 如果 $A[i+1]!=B[j+1]$
   - 回溯 $j$ 到 $next[j]$，直到$A[i+1]=B[j+1]$
   - 这里有一种情况（B串需要移动到 i 后面）
     - $j$ 回溯到 0 时也不能满足使 $A[i+1]=B[j+1]$
     - 当 $j=0$ 时，忽略$j$，增加 $i$，直到 $A[i+1]=B[j+1]$ （注意：只有一个元素时：它没有最长公共前后缀 ---> j = 0）
3. 当 j = m （B串完全匹配）时，输出位置，继续匹配（因为后面可能还有匹配的子串）

### 4. 构建 next 数组

注意：当只有一个元素，它没有最长公共前后缀

![image-20230425125113313](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251251399.png)

![image-20230425125125654](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251251748.png)



| 元素          | 最长公共前后缀                            |
| ------------- | ----------------------------------------- |
| A             | 0（只有一个元素时候，没有最长公共前后缀） |
| A B           | 0                                         |
| A B C         | 0                                         |
| A B C D       | 0                                         |
| A B C D A     | 1                                         |
| A B C D A B   | 2                                         |
| A B C D A B D | 0                                         |

B 串自己与自己匹配

$B[1]-B[i]$ 的前缀与后缀匹配

- 后缀为主串
- 前缀为模式串

递推方式求出 next 数组

目的：对于 $i\epsilon [2,m]$，求出 $B[1]-B[i]$ 的最长公共前后缀的长度

1. 如果匹配：
   - $next[i]=j+1$，$j$ 是 B串前缀的指针，也就是当前字符匹配之前的最长公共前后缀的长度，这里匹配成功了，要 + 1
2. 如果不匹配：
   - 回溯 j 指针，$j=next[j]$，直到匹配成功

可以看出 KMP 算法的时间复杂度是线性的，即：$O(m+n)$

关于 j 如何回溯，<font color="yellow">模式串（B）整体都是向前移的</font>，只要模式串 <font color="yellow">移到主串（A）的尾部</font>，算法就 <font color="yellow">结束 </font>了

KMP 一次比较只会产生 2种结果：

1. 匹配成功主串的指针（i）向前移动
2. 匹配失败模式串（B）整体向前移动

![image-20230427100927101](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271009265.png)



## 5. 贪心算法（greedy algorithm）

得到的结果不一定是最优的

### 1. 集合覆盖

| 广播台 | 覆盖地区         |
| ------ | ---------------- |
| K1     | 北京、上海、天津 |
| K2     | 广州、北京、深圳 |
| K3     | 成都、上海、杭州 |
| K4     | 上海、天津       |
| K5     | 杭州、大连       |

贪心策略：

1. 遍历所有电台，找到一个覆盖了最多 <font color="red">**未覆盖的地区**</font> 的电台【可能包括一些已覆盖的地区】
2. 将这个电台放入到一个集合中（比如：ArrayList），想办法把该电台覆盖的地区在下次比较时去掉
3. 重复第 1 步直到覆盖了全部的地区

![image-20230427165627223](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271656383.png)

![image-20230427165841072](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271658207.png)

![image-20230427170000829](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271700970.png)

![image-20230427170103726](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271701854.png)



```java
package com.greedy;

public class Greedy {

    static Set<String> allAreaSet = new HashSet<>();   //  存储剩余的 城市

    static HashMap<String, Set<String>> map = new HashMap<>();

    static List<String> listFinalKey = new ArrayList<>();

    public static void main(String[] args) {

        Set<String> set1 = new HashSet<>();
        set1.add("北京");
        set1.add("上海");
        set1.add("天津");
        Set<String> set2 = new HashSet<>();
        set2.add("广州");
        set2.add("北京");
        set2.add("深圳");
        Set<String> set3 = new HashSet<>();
        set3.add("成都");
        set3.add("上海");
        set3.add("杭州");
        Set<String> set4 = new HashSet<>();
        set4.add("上海");
        set4.add("天津");

        Set<String> set5 = new HashSet<>();
        set5.add("杭州");
        set5.add("大连");

        map.put("K1", set1);
        map.put("K2", set2);
        map.put("K3", set3);
        map.put("K4", set4);
        map.put("K5", set5);

        for (Set<String> value : map.values()) {
            for (String s : value) {
                allAreaSet.add(s);
            }
        }
        System.out.println(allAreaSet);

        List<String> k_list = getK_List(map, allAreaSet);
        System.out.println(k_list);

        System.out.println(allAreaSet);


    }

    public static int getNum(String K, Set<String> set){
        int count = 0;
        Set<String> listK = map.get(K);
        for (String s : listK) {
            if (set.contains(s)){
                count++;
            }
        }

        return count;
    }

    public static void remove(String K, Set<String> allAreaSet){
        Set<String> list_K = map.get(K);

        for (String s : list_K) {
            allAreaSet.remove(s);
        }
    }

    public static List<String> getK_List(Map<String, Set<String>> map, Set<String> allAreaSet) {

        while (allAreaSet.size() != 0){    //  城市集合不为空
            ArrayList<Integer> list = new ArrayList<>();

            Map<String, Integer> countMap = new HashMap<>();

            for (String s : map.keySet()) {
                int num = getNum(s, allAreaSet);
                list.add(num);
                countMap.put(s, num);
            }
            Collection<Integer> values = countMap.values();
            Integer max = Collections.max(values);  //  找到要处理的是哪个 key ！！！

            StringBuilder sb = new StringBuilder();

            for (Map.Entry<String, Integer> stringIntegerEntry : countMap.entrySet()) {
                if (stringIntegerEntry.getValue() == max){
                    sb.append(stringIntegerEntry.getKey());
                    break;
                }
            }

            String area =  sb+"";

            listFinalKey.add(area);

            remove(area, allAreaSet);
        }
        return listFinalKey;
    }
}
```

