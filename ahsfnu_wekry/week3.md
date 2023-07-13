| 编号 |                             题目                             | AC sub |     Sol Link     | done |
| :--: | :----------------------------------------------------------: | :----: | :--------------: | :--: |
|  1   | [P9165 「INOH」Round 1 - 意外](https://www.luogu.com.cn/problem/P9165) |        | [Link](#table1)  |  √   |
|  2   | [P8258 [CTS2022] 独立集问题](https://www.luogu.com.cn/problem/P8258) |        |                  |      |
|  3   | [P9157 「GLR-R4」夏至](https://www.luogu.com.cn/problem/P9157?contestId=103328) |        | [Link](#table3)  |  √   |
|  4   | [P9159 「GLR-R4」大暑](https://www.luogu.com.cn/problem/P9159?contestId=103328) |        |                  |      |
|  5   | [CF725E Too Much Money](https://www.luogu.com.cn/problem/CF725E) |        | [Link](#table5)  |  √   |
|  6   | [CF949E Binary Cards](https://www.luogu.com.cn/problem/CF949E) |        | [Link](#table6)  |  √   |
|  7   | [P2051 [AHOI2009] 中国象棋](https://www.luogu.com.cn/problem/P2051) |        | [Link](#table7)  |  √   |
|  8   | [P5435 基于值域预处理的快速 GCD](https://www.luogu.com.cn/problem/P5435) |        |                  |  √   |
|  9   | [P7606 [THUPC2021] 混乱邪恶](https://www.luogu.com.cn/problem/P7606) |        | [Link](#table9)  |  √   |
|  10  | [P9005 [RC-07] 超超立方体](https://www.luogu.com.cn/problem/P9005) |        | [Link](#table10) |  √   |
|  11  | [P4494 [HAOI2018]反色游戏](https://www.luogu.com.cn/problem/P4494) |        | [Link](#table11) |  √   |
|  12  | [P3516 [POI2011]PRZ-Shift](https://www.luogu.com.cn/problem/P3516) |        |                  |  ×   |
|  13  | [CF207C3 Game with Two Trees](https://www.luogu.com.cn/problem/CF207C3) |        | [Link](#table13) |  √   |
|  14  | [P7620 CF1431J Zero-XOR Array](https://www.luogu.com.cn/problem/P7620) |        | [Link](table#14) |  √   |
|  15  | [F - Communication Towers](https://codeforc.es/contest/1814/problem/F) |        | [Link](#table15) |  √   |
|  16  | [F - Wrap Around](https://codeforc.es/contest/1038/problem/F) |        | [Link](#table16) |  √   |
|  17  |  [E - Permanent](https://codeforc.es/contest/468/problem/E)  |        | [Link](#table17) |  √   |
|  18  | [F2 - Survival of the Weakest (hard version)](https://codeforc.es/contest/1805/problem/F2) |        |                  |      |
|  19  |          [「PA 2021」Areny](https://loj.ac/p/3608)           |        |                  |      |
|  20  |          [树特卡克林](https://qoj.ac/problem/5410)           |        | [Link](#table20) |  √   |
|  21  |           [匹配计数](https://qoj.ac/problem/4921)            |        |                  |      |

### <a id="table1">Sol 1</a>

传递信息的难点在于**每个信息都要是对的**，在接受处验证每个信息的正确性，如果信息错误则需要补传送一些信息来告知这个错误位的信息，但是哪一位是错误不可预知。

一种**不需要每一位都是对的**的信息传递方式是：**插值**。只要能累计传过去 $10^2$ 个正确的点值即可，此事的概率是很大的。

### <a id="table3">Sol 3</a>

对于任意积性函数 $f$，和整数 $n\le 10^5,m\le 10^{10}$，这个东西是好求的：
$$
\sum_{i=1}^n\sum_{j=1}^m f(ij)
$$
设 $F(x,m)$ 表示 $\sum_{j=1}^m f(xj)$，$p$ 为 $x$ 的最大质因子，那么容斥 $p$ 的指数，可以得到：
$$
F(x,m)=\sum_{i=0}^{\log_p m} f\left(x^{c_x(p)+i}\right)\cdot\left(F\left(\frac{x}{p^c},\frac{m}{p^i}\right)-F\left(\frac{x}{p^{c-1}},\frac{m}{p^{i+1}}\right)\right)
$$
记忆化，并预处理 $xm\le 10^6$ 的 $F$，并用 $\mathbb{PN}$ 筛筛出 $F(1,m)$ 的结果。

### <a id="table5">Sol 5</a>

首先处理的顺序非常确定，从大到小。设 $f_i$ 表示剩下 $i$ 元，需要加多少个硬币；然后就可以 dp 了。

### <a id="table6">Sol 6</a>

首先 $+2^k$ 和 $-2^{k}$ 只会有一个，否则可以用 $2^{2k}$ 和 $-2^{k}$ 替代；也不会有两个 $2^k$，否则可以换成 $2^k$ 和 $2^{2k}$；不断更换直到满足上述两个条件为止。

如果没有奇数，那么所有数除以 $2$。

如果有奇数，那么 $+1$ 和 $-1$ 恰好选一个，枚举选哪一个，然后把所有数 $+1/-1$ 后除以二，递归进两种情况去做。总复杂度 $a\log a$。

### <a id="table7">Sol 7</a>

随便竖着 dp 一下就好了。

### <a id="table9">Sol 9</a>

$n^3p^2$ 的 dp：设 $f_{i,x,y,a,b}$ 表示前 $i$ 个数，坐标 $x,y$，$(L,G)=(a,b)$ 是否可达。

优化：考虑一个合法的方案，给它一个随机顺序，那么 $x,y$ 最大值的期望只有 $n^{0.5}$。所以直接给所有 idea 随机一个顺序，然后就可以缩小 $f$ 中 $x,y$ 的上限。复杂度 $\frac{n^2p^2}w$。

### <a id="table11">Sol 11</a>

提出任意一个生成树，不难发现只要所有点的和是偶数则一定有恰好一组解，否则无解。解的个数其实就是 $2%^{m-n+1}$ 组。

在圆方树上统计一下就行了。

### <a id="table13">Sol 13</a>

广义 SAM 随便统计一下就好。当更方便的是树上后缀排序一下也是一样的。

### <a id="table14">Sol 14</a>

从高位到低位考虑，记下每个数是否顶着上界、是否顶着下界，可以得到 $4^n$ 做法。考虑去除无用状态：

既顶着上界又顶着下界：等价于头几位上界下界相同，不需要记下来。

既不顶着上界又不顶着下界：那么剩下的其它位任意，一定存在合法解，可以直接计算答案。

### <a id="table15">Sol 15</a>

线段树分治维护出每个时刻里和 $1$ 相连的连通块。需要找到所有这个连通块里还没有被标记过的点。

用一个 treap 状物维护连通块的点集，在 treap 的节点上维护子树内未被标记的结点的个数。

### <a id="table16">Sol 16</a>

考虑求出不包含的个数。不包含意味着：

1. $t$ 中不包含 $s$。
2. 设 $S$ 集合为 $t$ 前缀中那些存在 $s$ 的一个等长后缀与其完全相同的，$T$ 集合为满足类似条件的后缀。那么不存在两个元素 $x\in S,y\in T,x+y=|s|$。

然后发现 $S$ 集合只和它中最大的一个有关，$T$ 也只和 $T$ 中最大的一个有关，那么就简单了，dp 扫过去记下 $S$ 的最大值、当前在 kmp、SAM 上的结点编号就好。

### <a id="table10">Sol 10</a>

由于矩阵树定理：**一张图的生成树个数，等于其拉普拉斯矩阵的所有非零特征值的乘积除以点数**。

完全图的拉普拉斯矩阵的特征多项式为 $\lambda(\lambda-n)^{n-1}$，也就带来了 $n^{n-2}$ 的结论。

设图 $A$ 的所有特征值为 $\{a_i\}$，$B$ 的所有特征值为 $\{b_i\}$，$A\times B$ 的特征值为 $\{a_i+b_j\}$。那么再上生成函数做一下就成了。

拓展：求两张图的笛卡尔积的生成树个数（因为只知道特征多项式而不知道特征值），需要用到**结式**的知识。

### <a id="table17">Sol 17</a>

考虑状压最难考虑的是**恰好**选择了这些位置，而别的自由位不能碰到 $a$ 有特殊值的地方。考虑容斥掉：
$$
\prod_{i=1}^n a_i=\sum_{s\subseteq 2^n}\prod_{i\in S}(a_i-1)
$$
证明考虑乘法分配律。那么现在就不需要恰好了！直接记下可能会在同一列的列集合，就可以 $O(k^22^{\frac k2})$ 做了。

有另一个做法是：将 $a_{x,y}\neq 1$ 视为一个 $x$ 到 $y$ 的边，那么就是要选取一个匹配。保留一个生成树，枚举所有非树边的状态，就能做到 $2^{k-n} n^2$，$n$ 为去重后点的个数。两个做法平衡一下得到 $O(k^2k^{\frac k3})$ 做法。

### <a id="table20">Sol 20</a>

直接维护每条边是轻边还是重边，切换的总次数是 $O(n\log n)$ 的！考虑换根的过程，只有两根之间的这一条路径上的边以及改变的边的端点相连的一条边会发生改变，原本有 $O(\log n)$ 个轻边，后来有 $O(\log n)$ 个，总的变化量只有 $O(\log n)$ 个。

直接先全改成重边，然后试着找到所有轻边，发现轻边有一个必要条件是 $a_{father}\ge 2a_u$，这一定会覆盖某一个 $2^k$。所以维护 $sz$，然后对于所有 $2^k$，分别在平衡树上二分找到位置即可。