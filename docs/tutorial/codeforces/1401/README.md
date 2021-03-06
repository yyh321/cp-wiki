# Codeforces Round 665 (CF1401) 题解

## Problem A - [Distance and Axis](https://codeforces.com/contest/1401/problem/A)

### 题目描述

数轴上一点A的位置为$x=n$，问，至少将A移动多少距离，才能使得存在一个整数点B，满足$||\vec{AB}|-|\vec{OB}||=k$？

### 题解

$||\vec{AB}|-|\vec{OB}||\leq|\vec{AB}-\vec{OB}|=|\vec{OA}|=n$，因此如果$n<k$，我们必须移动A。此时移动的最小距离为$k-n$，移动后，我们可以将B取在O点处。

如果$n>k$，我们必定可以在$\vec{OA}$上找到一点B使得$||\vec{AB}|-|\vec{OB}||=k$，只要取$x_B=\frac{n-k}{2}$即可。但由于题目要求B为整数点，因此如果$n-k$为奇数，我们还需要将A右移$1$个单位。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/a.cpp

:::

## Problem B - [Ternary Sequence](https://codeforces.com/contest/1401/problem/B)

### 题目描述

有$a$和$b$两个数组，它们长度相等。$a$中有$x_1$个$0$，$y_1$个$1$和$z_1$个$2$，$b$中有$x_2$个$0$，$y_2$个$1$和$z_2$个$2$。现在要求找出一种$a$和$b$的排列方式，使得$\sum_{i=1}^N f(a_i,b_i)$最大，求出这个最大值。

其中，$f(x,y)$的表达式为：
$$
f(x,y)=\left\{
\begin{aligned}
xy & & x>y \\
0 & & x=y \\
-xy & & x<y 
\end{aligned}
\right.
$$

### 题解

不难发现，只有$x=2,y=1$时$f(x,y)$才为正，只有$x=1,y=2$时$f(x,y)$才为负。所以我们需要最大化$x=2,y=1$的对数，同时最小化$x=1,y=2$的对数。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/b.cpp

:::

## Problem C - [Mere Array](https://codeforces.com/contest/1401/problem/C)

### 题目描述

有一个数组，现在每次可以选择两个元素，如果这两个元素的最大公约数等于数组的最小值，则可以将这两个元素交换位置。问能否经过若干次操作后，将数组升序排列？

### 题解

所有是最小值倍数的元素可以排成升序（因为任意一个这样的元素都可以和最小值交换位置，所以经过若干次操作后一定可以将这些元素排成升序），而其他任意元素的位置都不能改变（不是最小值的倍数，意味着它和任意一个其他元素的最大公约数不会是最小值）。所以按照这个方式进行模拟，然后检查得到的数组是否是升序排列即可。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/c.cpp

:::

## Problem D - [Maximum Distributed Tree](https://codeforces.com/contest/1401/problem/D)

### 题目描述

有一棵$n$个节点的树，另外有$m$个质数，要求给这棵树的$n-1$条边分别标记一个权重$w_i$，使得所有权重的乘积等于这$m$个质数的乘积，并且权重中所含$1$的数量尽可能少。在这样的情况下，求$\sum_{i=1}^{n-1}\sum_{j=i+1}^ndist(i,j)$的最大值（模$10^9+7$）。其中，$dist(i,j)$为$i$节点到$j$节点简单路径的权值之和。

### 题解

显然可以求出每条边被计算的总次数。为了使得结果最大，我们应该把最大的权重赋给计算次数最多的边，依次类推。

如果质数的个数超过$n-1$个，我们可以把前$m-n+2$个合并（相乘）；否则我们需要在末尾补$1$。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/d.cpp

:::

## Problem E - [Devide Square](https://codeforces.com/contest/1401/problem/E)

### 题目描述

有一个左下角为$(0,0)$，右上角为$(10^6,10^6)$的正方形，现在有$n$条水平线段和$m$条竖直线段，已知所有线段至少有一边在正方形的边上，而另一边在正方形内，且任意两条线段均不共线，问这些线段把正方形分成了多少个区域？

### 题解

不妨把正方形的四条边也加入一并考虑。

经过试验和观察，我们可以发现，如果一条竖直的线段与$k$条水平线段相交，其中有$p$个交点是对应的水平线段上的第二个或以后的交点，那么就会增加$p-1$个区域。我们可以维护一个线段树，记录每一个高度处的水平线段当前的交点数情况（没有交点，一个交点，两个或更多交点）。这个线段树需要支持以下几种操作：

- $[l,r]$范围内的每条线段交点数增加$1$，也即增加了一条高度范围为$[l,r]$竖直线段，此时原本一个交点的变成两个交点，没有交点的变成一个交点
- $y$处的线段数增加$1$，此时新增一条没有交点的线段。
- $y$处的线段数减少$1$，此时所有情况都变为$0$（因为原本至多有一条）。

分别实现对应的操作，然后从左向右扫描即可。

本题坐标最大为$10^6$，而离散化之后最多需要处理$3\times10^5$个端点，二者差别不大，因此可以不离散化。如果坐标范围更大，比如到$10^9$，则必须进行离散化。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/e.cpp

:::

## Problem F - [Reverse and Swap](https://codeforces.com/contest/1401/problem/F)

### 题目描述

有一个总长度为$2^n$的数组，要求支持以下四种操作：

- $replace(x,k)$，将位置$x$处的数设为$k$
- $reverse(k)$，将每一段长度为$2^k$的子区间倒序
- $swap(k)$，交换每两段相邻的长度为$2^k$的子区间
- $sum(l,r)$，求$[l,r]$范围内所有数的和

### 题解

整个数组可以用一个线段树维护（并且这个线段树是一个完全二叉树），麻烦的是如何处理倒序和交换这两种操作。

无论倒序还是交换，相同的操作连续进行两次，等价于没有进行操作；另一方面，对于任意一层，它的两个孩子的分组关系是恒定不变的，会改变的只有它们的左右位置和内部顺序。以$[1,2,3,4,5,6,7,8]$为例，无论是进行倒序，还是交换，都不会改变$\{1,2,3,4\}$和$\{5,6,7,8\}$这样的分组关系；同样，向下一层，$\{1,2\}$和$\{3,4\}$的分组，$\{5,6\}$和$\{7,8\}$的分组也是始终保持的。

因此，我们可以用两个二进制数，一个记录当前的倒序操作，另一个记录当前的交换操作。每次有新的操作，就与当前的状态进行异或即可。

对于倒序和交换这两种操作，我们并不需要对线段树进行任何操作，直接修改对应的状态即可。实际需要实现的线段树操作就是单点赋值和区间查询。这里的难点在于，操作中给出的位置，并不一定对应实际的位置，而需要我们根据当前的倒序和交换状态，将其进行变换，以得到最终的实际位置。

赋值的情况相对简单，因为我们只需要处理一个点。如果当前层有倒序操作，我们就把这个点翻转到对称点上；如果还有交换操作，我们就再将这个点进行一个循环平移。

查询时我们需要处理一个区间，这时我们不能简单地只处理两个端点，因为对于两个端点跨越了当前区间中点的情形，这段连续区间实际上会对应两个不连续的区间。比如，数组$[1,2,3,4,5,6,7,8]$经过$swap(2)$和$reverse(3)$操作后，变为了$[4,3,2,1,8,7,6,5]$。这时，假设我们查询$[3,7]$，因为在最上层有倒序操作，所以我们翻转区间，将其变为$[2,6]$；然后，因为有交换操作，两个端点会变为$6$和$2$，但此时，如果我们继续按照$[2,6]$来进行操作，显然会发生错误，因为我们可以看到这段区间对应的值是$[2,1,8,7,6]$，实际上是$[1,2]$和$[6,8]$这两段不连续的区间。因此，我们要把倒序后得到的$[2,6]$首先拆分为$[2,4]$和$[5,6]$，然后对这两段区间分别进行平移，就可以得到$[1,2]$和$[6,8]$了。而如果两个端点在当前区间中点的同一侧，则不需要进行这一额外处理。

::: details 参考代码（C++）

<<< @/docs/tutorial/codeforces/1401/src/f.cpp

:::

<Utterances />
