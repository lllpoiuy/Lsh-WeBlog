# 分治与分块

### Example 1

> $n\times m$ 的网格图，有一些点是障碍点，不可以经过 $q$ 组询问，给定起点 $(x_1,y_1)$ 和终点 $(x_2,y_2)$ 询问能否只通过向右和向下从起点到达终点。$n,m\le 500,q<=6\times 10^5$。

一种想法是，我们直接从右下向左上 dp，并用 `bitset` 进行加速，复杂度 $O\left(\frac 1w \cdot n^4\right)$。

更快的做法是，分治，每次处理跨过中线的询问；对于每个点，预处理出它能到达中线的哪些点，询问时看看两个 `bitset` 有没有交集即可。$O\left(\frac 1w\cdot (n^3\log {n}+qn)\right)$。

一道类似的题是：

> 给定一个 01 矩阵。求：有多少子矩形的和恰好为 $k$。$n,m\le 2500,k\le 6$。

一样也是分治，处理跨过中线的子矩形，但是仔细思考会发现只对行分治时复杂度是 $nm^2k$ 的！要对行和列交替分治复杂度才是正确的 $O(nmk\log{n})$。

### Example 2

> 有一棵树，每个点有一个点权，边权都是 $1$。问路径上的所有点的 $\gcd$ 不是 $1$ 的最长路径是多少？$n\le  2\times 10^5$。

这是一道点分治能做但是边分治不能做的题。边分治时难以合并子树答案，但是点分质时有一个重要的性质是，$\gcd$ 是当前树根 $u$ 的 $A_u$ 的因数，值的个数是不多的。因此可以直接暴力合并。

类似的一个题是：

> 有一棵 $n$ 个节点的树，每个点有一个不超过 $n$ 的点权，边权都是 $1$。要求对每个从 $1$ 到 $n$ 的 $i$，求出满足 $x$ 到 $y$ 简单路径上所有点权 $\gcd$为 $i$ 的数对 $(x,y)$ 数量。$n\le 2\times 10^5$。

我们当然也可以像上面那样点分治，但是有更方便的做法：枚举每个因数，只保留点权是这个因数倍数的点，显然每个连通块的贡献系数是其大小取 $2$ 的方案数。$A\sqrt A$ 地遍历点来计算，最后一次莫反即可。

### Example 3

> $n$ 个节点的树，每次往每个节点上放 $1\sim n$ 不重复的数，要求不存在某个节点，满足其父节点的数比自己大 $1$。求方案数。$n \le 2.5\times 10^5$。

我们容斥，枚举哪些儿子的权值等于其父亲加一，容斥的系数就是 $(-1)^{|S|}$，其中 $S$ 为不合法的儿子集合。

容易发现，一个不合法的儿子相当于绑定了儿子和父亲；每个不合法的儿子，都能使自由元的数量减少恰好一。问题就变成了统计有 $i$ 个儿子不合法的方案数。

倒过来统计，枚举父亲和哪个儿子绑定还是不绑定，答案的生成函数就是 $\prod\limits_{i=1}^n{\left(1+|son_i|x\right)}$。分治 FFT 即可。

### Example 4

> $n$ 个节点的树，点有点权，边有边权。$q$ 次操作，
>
> 1. 询问一个点当前的点权。
> 2. 给定 $u,x,y$，对于所有在 $u$ 的子树中，且满足 $u$ 到 $v$ 路径上所有边权均 $>=y$ 的点 $v$，点权 $+x$。
>
> $n,q <= 10^5$，强制在线。

对于这种“**只存在从祖先到后代的链的修改**”的题目，有根树点分治是非常有效的做法。

对于一个分治子树，记 $root$ 为其中深度最浅的节点，$u$ 是其分治中心，那么跨过 $u$ 的修改的左端点一定在 $root\rightarrow u$ 的链上，右端点一定在 $u$ 的子树中。对于每个 $u$ 开一个以到 $u$ 点相对深度为下标的数据结构用于维护 $u$ 的子树。

查询时，查询在点分树上的祖先且是原树上祖先的所有点，查询这些点的数据结构。

修改时，修改所有点分树上的祖先且是原树上的后代的所有点，修改这些点的数据结构（若修改点到分治中心的路径上已经存在边 $<y$ 则可以忽略这个分治中心）。

这类方法可以维护所有修改可以方便地叠加的修改操作（类似线性性？）。

官方题解的做法是每根号次修改重构一下：https://csacademy.com/contest/round-10/task/yurys-tree/solution/。

### Example 5

> 小 $A$ 得到了众多迷妹的倾慕，他的迷妹共有 $n\times m$ 个（$1\le n,m\le 500$），分布在一个 $n\times m$ 的网格里，其中所有网格大小均为 $1\times 1$，每个网格中恰好有一个迷妹且处于网格正中央，第 $i$ 行第 $j$ 列的迷妹坐标为 $(i,j)$，对小 $A$ 的倾慕程度为 $A_{i,j}$（$A_{i,j}$为非负实数）
>
> 为了量化自己的人赢程度，小 $A$ 定义他在一点的魅力值如下——当小 $A$ 处在坐标为 $(x,y)$ 的网格正中央时，处于网格 $(i,j)$ 的迷妹对于他魅力值的贡献为 $\frac {A_{i,j}}{d+1}$，其中 $d$ 为两人的欧几里得距离（距离越近越有可能擦出火花嘛），特别地，当两人欧几里得距离大于等于 $r$（$0<r\le 1000$）时，由于距离过远，迷妹并不会对小 $A$ 产生倾慕，所以对于魅力值的贡献为 $0$。现在给定 $n,m,r$ 小 $A$ 想知道——给定正整数 $k$（$1\le k\le nm$），在所有 $n\times m$ 种可能中，他能得到的前 $k$ 大魅力值分别是多少？
>
> $n,m \le 500$，$ r \le 1000$。

数据结构别学傻了！把 $\frac 1{d+1}$ 用二维数组表示好，和 $A$ 进行一次二维 FFT 实现加法卷积即可。

### 其他一些根号算法：

> **图上根号分治**：对于 $n$ 个点，$m$ 条边的图（树也是图的一种），度数超过 $\sqrt{m}$ 的，最多 $O(\sqrt{m})$个点；而度数较小的点往往可以暴力。
>
> **根号重构**：带有修改的询问类问题，单次修改对单次询问影响可以高效算出，若无修改，用数据结构也可以高效回答询问，但重构开销较大（比如重建线段树或其他复杂数据结构等），可以每 $\sqrt{q}$ 次将所有修改放在一起重构一次，回答询问时，在上一次重构的基础上，暴力计算此后的修改对答案的影响。
>
> **根号分治**：分成某个变量小于等于根号或大于等于根号两类来讨论，以平衡两个做法的复杂度。

### Example 6 [九省联考 2018] IIIDX

> 给定 $k$ 和一个长度为 $n$ 的序列，你需要将其重新排序，满足 $a_i \ge a_{\left\lfloor \frac ik\right\rfloor}$。
>
> 输出字典序最大的解。 $1 \le n \le 500000$。

我们惊奇地发现直接递归下去的贪心在第二层可能是对的，但到了第三层就已经偏离了。原因是优先级的不同，直接递归下去的贪心是基于 dfs 序下字典序最小，而题目要求的是 bfs 序。

解决办法是改变分配的顺序。将序列先从大到小排序，然后按照 bfs 序遍历整棵子树，每次给其分配一个左边剩下的点数大于等于子树内点数的值。线段树即可做到 $O(n\log{n})$。

### Example 7 [CF1413F] Roads and Ramen

> 给定一棵 $n$ 个点的树，边权为 `0/1`。$m$ 次操作，每次操作翻转一条边的边权，你需要在每次操作后回答最长的经过偶数个 $1$ 的路径。$n, m \le 500000$。

必然以直径的一端为一端点（易证）。因此只需支持一个允许区间 01 翻转、查询状态为 1 的位置的权值的最大值。直接在线段树上维护同区间内翻转后和没翻转的两种最大值即可。

### Example 8 [CF1303G] Sum of Prefix Sums

> 对于一个序列 $a_1, a_2, \cdots , a_n$，定义它的“前缀和的和”为 $a_1 + (a_1 + a_2) + (a_1 + a_2 + a_3) + \cdots + (a_1 + a_2 + \cdots + a_n)$。给定一棵 $n$ 个点的树，你需要求出“前缀和的和”最长的一条路径。$n \le 150000$。

首先点分治，考虑如何拼接前半段和后半段。设前半段的前缀和的和为 $s$，元素个数为 $x$；后半段的元素之和为 $a$，前缀和的和为 $b$，那么总的前缀和的和就是 $ax+b+s$。固定 $x$ 和 $s$，发现 $a$ 和 $b$ 的选取一定是取坐标 $x$ 处最高的直线。用李超树维护即可。

### Example 9 [CF1290E] Cartesian Tree

> 给定一个 $1\sim n$ 的排列 $a$。对于一个整数 $k\in[1,n]$，将排列中 $\leqslant k$ 的项构成的子序列建**大根笛卡尔树**。这棵笛卡尔树的所有节点的子树大小之和记为 $s_k$。$\forall k\in[1,n]$，求 $s_k$。
>

设 $fl_i$ 表示左边第一个比 $a_i$ 大的位置，$fr_i$ 同理，我们可以得到：
$$
Ans=\sum_{i=1}^n fr_i-fl_i-1
$$
为了修正还未加入的位置的影响，我们把这些未加入位置 $i$ 的 $fl_i$ 设为 $i-1$，$fr_i$ 设为 $i$。每次插入都会使左边的每个位置的 $fr_i$ 和当前插入位置取 $\min$，右边的每个位置的 $fl_i$ 和当前插入位置取 $\max$。直接用吉老师线段树维护即可，因为我们惊喜地发现未插入的位置一定会被维持现状，而新插入的位置的 $fl,fr$ 只需要单点修改。

### Example 10 [SDOI2019] 染色

> 给定一棵树，支持两种操作：将从 $a$ 到 $b$ 的一条链染成新的颜色 $c$；查询 $a$ 到 $b$ 路径上的颜色段数量。$n, q \le 10^5$。

树剖上线段树，维护区间左端点颜色、右端点颜色、颜色段数量即可。



