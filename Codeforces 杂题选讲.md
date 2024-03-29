# Codeforces 杂题选讲

讲师：PinkRabbit   时间：20210701   地点：福建师范大学附属中学

## Example.1 724E. Goods transportation

#### 题意：

> 有 $n$ 座城市，第 $i$ 座城市生产了 $p_i$ 单位货物，最多能卖出去 $s_i$ 单位货物。
>
> 对于 $1≤i<j≤n$，城市 $i$ 有一条单向运输线路通向 $j$。
>
> 所有运输线路的容量都为 $c$ 单位货物。不能反向运输，但是货物可以经过任意多个运输线路。
>
> 请求出总共最多能卖出去多少单位货物。
>
> 数据范围：n≤10^4。时间限制：2 s。Bonus：n≤10^6。

#### 题解：

首先考虑网络流建模，模型显然；但是边数是 $O(n^2)$ 的且难以优化。

这时一个惊世骇俗的步骤登场了——由最大流等于最小割，然后用动态规划来优化！

根据最小割的第二种定义：

> 将原图划分为两个点集，使得他们之间的连边边权和最小。

所以，一个点要么割掉它向 $s$ 的连边，要么割掉向 $t$ 的连边；且如果一个点割掉了 $t$，那么它后面的所有点都必须把 $t$ 割掉或者是要切断中层的所有连边。

根据第二定义也可以很快地得出转移方程：$f_{i,j}$ 表示 $i$ 个连向 $s$、$j$ 个连向 $t$ 的最小割，有
$$
f_{i,j}=\min(f_{i,j-1}+ic+s_{i+j},f_{i-1,j}+p_{i+j})
$$
于是这个及其震撼人心的 `DP` 就诞生了！

能不能再优化？设 $a_{1\sim k}$ 为与 $s$ 连接的点集，有：
$$
ans=\left(\sum_{i=1}^n p_i\sum_{i=1}^k(s_{a_i}-p_{a_i})+\right)+\left(\sum_{i=1}^k(a_i-1)c -\begin{pmatrix}k\\2\end{pmatrix}\cdot c\right)
$$
提取一下：
$$
ans=\sum_{i=1}^n p_i-\begin{pmatrix}k\\2\end{pmatrix}\cdot c-kc+\sum_{i=1}^k(s_{a_i}-p_{a_i}+a_ic)
$$
枚举 $k$ 并且排序一下即可做到 $O(n\log n)$。

## Example.2 802O. April Fools’ Problem (hard)

#### 题意：

> 给定两个长度为 $n$ 的正整数序列 $a_{1∼n}$ 和 $b_{1∼n}$，以及一个正整数 $k$。
>
> 请选出恰好 $k$ 对数 $(p_i,q_i)$ 满足 $p_i≤q_i$ 且 $p_{i-1}<p_i$ 且 $q_{i-1}<q_i$。
>
> 最小化 $\sum_{i=1}^k a_{p_i} +\sum_{i=1}^k b_{q_i}$ 。
>
> 数据范围：$n,k≤5×10^5$。时间限制：10 s。

#### 题解：

这题莫不是一个模拟费用流板子？但是 $k$ 的限制怎么办？`wqs` 二分即可解决。

模拟费用流具体流程如下：从左到右扫描，依次加入 $a_i$，并在堆中查找最大的与 $b$ 匹配；为了反悔，可以把 $-b_i$ 当作一个 $a$ 加入堆中。

如何去掉一个 $\log$ 呢？考虑更加暴力地模拟费用流，发现复杂度瓶颈在于最短路，用线段树来维护最短路即可。

## Example.3 827F. Dirty Arkady’s Kitchen

#### 题意：

> 有一张 $n$ 个点 $m$ 条无向边的图，每条边的长度为 $1$，但是第 $i$ 条边限制只能在时间段 $[l_i,r_i]$ 内通过（即从一端点出发时必须在时刻 $[l_i,r_i-1]$ 内），而且是禁止在一个点处停留的，也就是到达了一个点就必须选择一条出边继续走，经过一条边必须恰好花费 1 的时间，并且必须到达另一个端点。
>
> 求从 $1$ 到 $n$ 的最短路。
>
> 数据范围：$n,m≤5×10^5$。时间限制：$6$ s。

#### 题解：

考虑一条边可以反复走，且反复走时奇偶位是分开的，所以考虑按照奇偶将所有点拆点，每条边也拆成 $4$ 条有向边。

先讲做法：对于每个点记录 $f_{x,0/1}$ 表示点最后一次可达的时间；将所有边加入堆中（按照 $l$），依次取出，如果 $f_{u,l\&1}\ge l$，就拿 $r+1$ 更新 $f_{v,!(l\&1)}$；否则将其加入这条边的暂存桶里边；每次遇到可行边时也更新 $v$ 的桶，将 $[r+1,r_{v,1\sim?}]$ 重新加入堆中；可达终点时输出 $l+1$。

为什么是对的呢？因为 $l$ 是递增的，所以当 $f>l$ 时，$l$ 时刻一定是可达的；使用暂存桶的目的是保证这条新的边也是在正确的时候取出，不会影响前面的性质；由于这个性质，输出 $l+1$ 也就很显然了。

## Example.4 908H. New Year and Boolean Bridges

#### 题意：

> 存在一张 $n$ 个点的有向图，记 $f(i,j)$ 表示点 $i$ 能否直接或间接到达点 $j$。
>
> 你不知道这张图的边集，只给定了一个矩阵 $A_{n×n}$。
>
> 其中 $A_{i,j}$（$i≠j$）只能为字符 $A, O, X$ 之一，而 $A_{i,i}$ 均为字符 `-`。
>
> 对于两点 $u,v$（$u≠v$）：
>
> 1.如果 $A_{u,v}$ 为 `A`，则表示 $f(u,v) \and f(v,u)=1$。
>
> 2.如果 $A_{u,v}$ 为 `O`，则表示 $f(u,v) \or f(v,u)=1$。
>
> 3.如果 $A_{u,v}$ 为 `X`，则表示 $f(u,v) \oplus f(v,u)=1$。
>
> 请求出满足条件的有向图的最小可能边数，或判定这样的图不存在。
>
> 数据范围：$n≤47$。时间限制：$5$ s。

#### 题解：

考虑先将以 `A` 连接的 进行缩点，如果同一个强连通分量内存在 `X` 就矛盾无解。

注意到，若一个强连通分量大小大于等于二，那么它会多产生一的代价；如果大小等于一，那么没有额外代价；那么我们只要尝试将可以合并的尽可能合并。

因为大小为一的没啥关系，因此直接去掉；剩下的连通块数量 $\le \frac n2$，而 $2^{\frac n2}$ 是可以接受的！

因此我们考虑指数级做法，设 $f=\sum_X judge(X)\cdot z^X$，表示 $X$ 是否可以缩在一起（内部是否没有 `X`）。

我们的任务是找到最小的 $k$ 使 $[z^U]f^k=1$，其中乘法为子集卷积，复杂度 $n^3\cdot 2^{\frac n2}$，不可接受。

这时我们发现，即使改成或卷积，答案不变！复杂度 $n^2\cdot 2^{\frac n2}$，不可接受。

这时我们发现，复杂度的瓶颈不在于点积，而在于每次的 `iFWT`！由于我们只需要求一个点的值，因此我们直接 $2^{\frac n2}$ 算这个点的点值。复杂度 $n\cdot 2^{\frac n2}$，可接受。

计算单点点值可以参考 `FWT` 的右乘矩阵 $M_{x,y}=(-1)^{\operatorname{popcount}(x\oplus y)}\cdot[x\and y=x]$，得 $f_X'=\sum (-1)^{\operatorname{popcount}(x\oplus y)}\cdot[Y\subseteq X]\cdot f_Y$。

这时我来**吊打标算**了，直接**随机化**，随机一个排列，依次加入，每次争取加入最前面得强连通分量，直接 $50n^2$ 草过。

## Example.5 1067D. Computer Game

#### 题意：

> 有 $n$ 个游戏，每个游戏有收益 $a_i$，成功率 $p_i$。
>
> 每秒可以玩一个游戏，如果成功了则获得收益，并且获得一次可以升级游戏的机会。
>
> 每个游戏有升级后的收益 $b_i$（$a_i<b_i$）。
>
> 问玩 $t$ 秒游戏可以获得的期望收益的最大值。输出浮点数即可。
>
> 数据范围：$n≤10^5$，$t≤10^10$，$1≤a_i<b_i≤10^{18}$，$0<p_i<1$。时间限制：3 s。

#### 题解：

显然，第一次获得升级机会时，我一定要升级一个 $p_i\cdot b_i$ 最大得，然后一直点那个。

设 $f_t$ 表示剩下 $t$ 时间、没有升级时的最大期望收入，则：
$$
f_t=\max_{i=1}^n(1-p_i)f_{t-1}+p_i(a_i+bp_{\max}\cdot(t-1))
$$
变形一下：
$$
f_t=\min_{i=1}^np_i\cdot(bp_{\max}\cdot t-f_{t-1})+f_{t-1}+p_ia_i-p_i\cdot bp_{\max}
$$
这是一个一次函数！我们求出由每个 $p_i\cdot x+p_ia_i-p_i\cdot bp_{\max}$ 组成的上凸壳（$f_{t-1}$ 看作常数，直接忽略）。

对于凸壳上的每一段，均二分并矩阵快速幂加速找到分界点；当然利用倍增可以优化到一个 $\log$。
$$
\begin{bmatrix}f_t\\t\\1\end{bmatrix}\times
\begin{bmatrix}
1-p_i & 0 & 0 \\
p_i\cdot bp_{max} & 0 & 0 \\
-p_i\cdot bp_{max}+p_i\cdot a_i & 0 & 1
\end{bmatrix}
=\begin{bmatrix}f_{t+1}\\t+1\\1\end{bmatrix}
$$

## Example.6 1239E. Turtle

#### 题意：

> 有一个 $2$ 行 $n$ 列的棋盘，每个格子里有整数 $a_{i,j}$。
>
> 一只乌龟从 $(1,1)$ 走到 $(2,n)$，只能往下走和往右走。定义路径权值为路径上经过的数值之和，乌龟想要最大化这个权值。
>
> 你可以对棋盘上的数字重新排列，最小化乌龟可能获得的最大权值。
>
> 请构造达到这个最小值的方案。
>
> 数据范围：$n≤25$，$0≤a_{i,j}≤5×10^4$。时间限制：5 s。

#### 题解：

性质 1：第一行递增、第二行递减，证明使用交换法，显然不劣。

性质 2：乌龟要么走第一行，要么走第二行，证明考虑权值和的差分数组，即可证明是下凸的。

于是就可以愉快地背包拉！

## Example.7 1270H. Number of Components

#### 题意：

> 给定一个长度为 $n$ 的数组 $a_1∼a_n$，保证 $a_i$ 互不相同。
>
> 对于数组中每一对**顺序对**（$i<j$ 且 $a_i<a_j$），在 $i,j$ 之间连一条无向边。
>
> 求图中连通块个数。
>
> 你需要支持 $q$ 次修改 $a_i←x$，保证每次修改后 $a_i$ 互不相同。
>
> 数据范围：$n,q≤5×10^5$，$1≤a_i,x≤10^6$。时间限制：8 s。

#### 题解：



## Example.8 1285F. Classical?

#### 题意：

> 给定 $n$ 个正整数 $a_1,a_2,…,a_n$，求 $max_{1≤i,j≤n} ⁡\operatorname{lcm}⁡(a_i,a_j)$ 。
>
> 数据范围：$n,a_i≤10^5$。时间限制：1 s。

#### 题解：

考虑枚举 $\gcd$，然后找到 $a_x \perp a_y$ 中 $a_x\cdot a_y$ 最大的。

考虑从大到小加入一个栈中，如果存在栈中元素与当前元素互质，我们可以把这个元素及其之后所有元素弹出，因为显然没用了。

问题就转化为维护栈中是否有元素与当前元素互质，有：
$$
\sum_{i=1}^n[gcd(a_i,m)=1]=\sum_{k|m}\mu(k)\sum_{i=1}^n[k|a_i]
$$
考虑如何维护 $\sum_{i=1}^n[k|a_i]$；我们暴力枚举因数，因为可以去重，所以复杂度是 $\sum d=n\ln n$ 的。

总复杂度是两个 $\ln$ 的，能不能再优化？我们把每个 $a$ 的所有因数都加入 $a$ 中，答案显然不变；只需找到 $a_x \perp a_y$ 中 $a_x\cdot a_y$ 最大值即可。因为 $\operatorname{lcm}$ 必然是可以拆成这样两部分。

