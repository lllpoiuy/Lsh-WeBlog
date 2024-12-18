# JSOI 模拟赛题解合辑 3

### JSOI13 T1 sumsum

> 给一棵 $n$ 个节点的树，用 $1$ 到 $n$ 的整数表示。每个节点上有一个整数权值 $a_i$。再给出两个整数 $[L,R]$。
>
> 现在有 $m$ 个操作，每个操作这样描述：给定树上两个节点 $(u,v)$ 和一个整数 $x$，表示将树上 $u$ 到 $v$ 唯一的简单路径上每个点的权值都加上 $x$。每次操作之后求树上所有节点个数大于等于 $L$ 小于等于 $R$ 的简单路径的节点权值和之和。$n,m\le 10^5$。

一次点分治求出每条边的贡献系数，然后用普通 lca 求链和即可。

### JSOI13 T3 排序

> 一天，小 $\Delta$ 学习了鸡尾酒排序算法，他想计算一共有多少个长度为 $n$ 的排列可以被 $k$ 轮鸡尾酒排序排好序。（对 $998244353$ 取模）。$n\le 10^{18},k\le 10^5$。鸡尾酒排序的伪代码：
>
> ```
> for t=1,...k
> 	for i=1,2,...,n-1: if a[i]>a[i+1]: swap(a[i],a[i+1]);
> 	for i=n-1,n-2,...,1: if a[i]>a[i+1]: swap(a[i],a[i+1]);
> ```

“能够被排好序”当且仅当：对于所有 $x\in [1,n]$，令 $b_i=[a_i\ge x]$，$b[1\cdots n]$ 可以再 $k$ 轮内完成排序。

单独考虑每个 $b$ 序列：一轮鸡尾酒排序，相当于把一个身处本来该放 $1$ 的位置的 $0$ 移回到本来该放 $0$ 的位置，并相应地把一个 $1$ 挪过去。因此 $\operatorname{check}(x)=1$ 当且仅当 $s_x=\sum_{i=1}^{x-1}[a_i\ge x]\le k$。

一个 $a_i$ 对 $s$ 的影响是：$\forall x\in[a_i,i)$，$s_x\leftarrow s_x+1$。依然是考虑将排列具象为一个二分图匹配：设 $f_{i,j}$ 表示前 $i$ 个还有 $j$ 对点没匹配的方案数。枚举当前一对点的连边方案，有转移方程：
$$
f_{i,j}=f_{i-1,j}+f_{i-1,j-1}+2j\cdot f_{i-1,j}+(j+1)^2f_{i-1,j+1}
$$
限制任意时刻 $j\le k$ 即可。直接 `dp`：$O(nk)$；矩阵快速幂优化：$O(k^3\log{n})$；求出前 $2k+2$ 项然后 `BM` 然后 $k^2\log{n}$ 递推可以获得 $60pts$。

考虑特征多项式优化：转移矩阵形如每行有两个或三个整数，$A_{i,i-1}=i^2$，$A_{i,i}=2i+1$，$A_{i,i+1}=1$。注意到最后一行只有两个整数，所以枚举最后一行选了哪个。设 $d_n$ 为 $n$ 阶转移矩阵的行列式，有 $d_n=(2n+1-x)d_{n-1}+n^2 d_{n-2}$。

将这个递推式也写作矩阵的形式（矩阵的每个元素都是一个多项式），进行一次分治 FFT 即可使得复杂度为$O(k\log{n}(\log{k}+\log{n}))$。

### JSOI18 T3 异或 (xor)

> 给定正整数 $d$ 和两个长度为 $2d$ 的数组 $a_{0,1,\cdots ,2^d−1}$，$b_{0,1,\cdots ,2^d−1}$。记集合 $S = {0, 1, ..., 2^d − 1}$。
> 对于 $S$ 的子集 $T$ ，定义 $T$ 的权值为 $b_{\oplus_{x∈T}x} ∏_{x∈T} a_x$。对每个 $k = 1, 2, \cdots , 2^d$，求 $S$ 所有大小为 $k$ 的子集权值和。答案对 $10^9 + 7$ 取模。$d\le 11$。

$b$ 的下标里是集合元素的异或和，$k$ 这一维是答案的维度，都不能优化掉，要记在状态里。所以实际上就是要快速求：
$$
c(x,y)=\sum_{T\subseteq S} \prod_{e\in T}a_e\cdot x^{\oplus_{e\in T}e}\cdot y^{|T|}=\prod_{i=0}^{2^d-1}\left(1+a_ix^iy\right)
$$
统计答案时就可以直接：$\mathrm{Ans}(y)=\sum_{i=0}^{2^d-1} b_i\cdot [x^i]c(x,y)$。参照占位多项式的优化方法，$x$ 和 $y$ 中得取一维为主元；当取 $y$ 为主元时，问题就变成了一个快速求 $\exp$ 的问题，用递推式优化复杂度仍然为 $O(2^{3d})$；只有借助多项式牛顿迭代才能做到 $O(2^{2d}d)$，但是不优秀。所以我们取 $x$ 为主元，$c$ 看作一个集合幂级数，每一项的次数都是一个关于 $y$ 的多项式。

等式两边同时取 $\operatorname{FWT}$ 得：
$$
[x^i]\operatorname{FWT}(c(x))=\prod_{j=0}^{2^d-1}\left(1+(-1)^{|i\cap j|}\cdot y\right)
$$
要想求出这个东西的具体形式就得依赖分治 FFT；这依然不优美，我们考虑不求出这个东西的具体形式怎么求出答案。注意到 $\operatorname{FWT}$ 的线性性，我们可以只对 $b$ 进行 $\operatorname{iFWT}$，起到同样的效果：
$$
\mathrm{Ans}(y)=\sum_{i=0}^{2^d-1}[x^i]\operatorname{iFWT}(b(x))\cdot\prod_{j=0}^{2^d-1}\left(1+(-1)^{|i\cap j|}\cdot y\right)
$$
我们考虑求出 $2^d+1$ 个点值，然后把 $\mathrm{Ans}(y)$ 拉格朗日插值出来。问题变成了对于每个 $i$ 快速求出 $2^d+1$ 个点值；考虑把它们合在一起做，想普通的 $\operatorname{FWT}$ 一样。一开始，把所有的 $j$ 的贡献只记在 $v_i(y)$ 一个位置上。然后 `for k := 0,1,2,3,...,d-1`（依次考虑每一位的贡献），有 $v'_{i}(y)=v_i(y)\cdot v_{i+2^k}(y)$，$v_{i+2^k}(y)=v_i(y)\cdot v_{i+2^k}(-y)$。对于每个 $v(y)$，维护 $y=-2^d,-2^d+1,\cdots,2^d-1,2^d$ 共 $2^d+1$ 个点值即可。复杂度 $2^{2d}d$。

### JSOI31 T2 中鱼人 (medium)

> 对于一个字符串 $t$，定义 $f (t)$ 表示使得 $t$ 变成有序的最小操作次数。
> 操作：对于所有 $t$ 的长为 $2$ 且形如 `BA` 的子串，将 `BA` 变成 `AB`，所有位置的 `BA` → `AB` 同时进行。
> 给定一个长为 $n$ 的 `AB` 串 $s$，有 $q$ 次询问，每次翻转 $s$ 的一个区间 $[l, r]$（`A` 变成 `B`，`B` 变成 `A`），并求出修改后 $f (B + s + A)$ 的值（加法表示拼接，修改永久）。

设 `A` 的位置值为 $1$，`B` 的位置值为 $-1$，将其画成一条折线。一次操作会把所有的极小值（谷底）都往上翻，最低谷的高度恰好 $+1$。而有序序列相当于完整的一座山，高度为 $1$ 的个数。所以排序需要次数为 $1$ 的个数减去前缀极小值。

线段树，区间维护翻转前和翻转后的前缀和和最小值即可做到 $O(n\log{n})$。

### JSOI31 T3 大鱼人 (large)

> 给定一棵 $n$ 个点的树，对于每个 $0 ≤ i ≤ K$，求是否能断 $i$ 条边使每个连通块点权和在 $[L, R]$ 间。
>
> $n\le 1000$，$K\le 50$，$1\le L\le R\le 10^{18}$。

记 $f_{i,j}=\{x\}$ 表示存在一种断边方式使得 $i$ 子树内断了 $j$ 条边，$i$ 所在连通块的和为 $x$ 的所有 $x$ 构成的集合。

考虑剪枝：若存在 $a,b,c\in S,a<b<c$ 且 $c-a<R-L$，则 $b$ 可以从 $S$ 中被删掉。证明使用调整法，若存在任意一种合法的方案，对于方案中使用的已经被删掉的元素，依次将其调整为 $a$ 或者 $c$，一定有一个合法。

题解声称：此做法复杂度为 $O(nk^3)$，因为 $\max f_{i,j}-\min f_{i,j}\le j\cdot (R-L)$（考虑被断掉的集合大小的差），所以剪枝后有 $|f_{i,j}|=O(j)$。

### JSOI30 T2 宝藏

> 给定一张 $n$ 个变量的 `2-SAT`，求它的合法的解的个数。$n\le 40$。

拆成两部分点，左边 $25$ 个点用枚举的，右边 $15$ 个点用三进制 $\operatorname{FWT}$。复杂度 $O(2^{25}+15\cdot 3^{15})$。

### JSOI30 T3 树上随机

> 有一棵树，树上每个点初始有一个数字，这个数字是 $0$ 或者 $1$，你可以随机一个点开始，每次等概率随机选择一个新点并且沿着树上的路径移动过去，最后翻转这个新点的数字，也就是 $0$ 变成 $1$、$1$ 变成 $0$，注意只翻转这个新点的数字而不是路径上所有点的数字，且最开始的点不翻转，如果某个时刻，整棵树上的所有数字均为 $0$ 或者均为 $1$，那么结束，现在问你期望在结束之前期望要总共要移动多少距离。$n\le 10^5$。

总距离的期望 = $\sum_{i=1}^n$ 第 $i$ 个点出发的路程的期望 = $\sum_{i=1}^n$ 第 $i$ 个点出发的单条路径的期望长度 $\times$ 第 $i$ 个点的期望出发次数。所以问题就变为了求第 $i$ 个点期望作为多少次起点，这显然只和这个点的颜色和这个颜色的点的个数有关。设 $f_{i,0/1}$ 表示当前有 $i$ 个 $0$ 点时一个 $0/1$ 点的期望出发次数。有方程：
$$
f_{i,0}=\frac 1n f_{i-1,1}+\frac {i-1}n f_{i+1,0}+\frac {n-i}nf_{i-1,0}\\
f_{i,1}=\frac 1n f_{i+1,0}+\frac {n-i-1}nf_{i+1,1}+\frac {i}nf_{i-1,1}
$$
直接设 $f_1=\{x,y\}$，然后用 $f_1$ 的方程解出 $f_2$，再用 $f_2$ 的方程解出 $f_3$……最后用 $f_{n-1}$ 的方程解出 $x,y$。$O(n)$。

关于期望的性质：/fn/fn/fn 我完全搞不懂这都是为啥它有的时候可以拆开有的时候不能拆开 /fn/fn/fn！！！

### JSOI29 T1 幻想

> 给定 $k$，按照如下方式生成一个无限长的序列 $s$：（以 $k=3$ 为例）
>
> ```
> 012
> 012120201
> 012120201120201012201012120...
> ```
>
> 多次询问 $k,l,r$，求出 $\sum_{i=l}^r s_i\cdot\left\lfloor\frac{(i\bmod 20000116)^2+i+804}{233}\right\rfloor \pmod{2^{32}}$。$k\le 1000$，$\sum r-l\le 10^8$，$l\le r\le 10^{16}$。

考虑递归求解单个 $s_i$ 的过程，我们发现 $s_i$ 为 $i$ 在 $k$ 进制下的数位和模 $k$！

考虑模拟 $k$ 进制下的 $+1$ 运算，均摊下是 $O\left(\sum r-l\right)$ 的！所以此题是有一定思维含量的大卡常题。优化重点是，去掉取模运算和用数组预处理出每个 $<k$ 的数 $+1$ 的结果。

### JSOI29 T3 现实

> 给定一张有向图，求有多少个点满足删去它和它相邻的边后，整张图变成 `DAG`。$n\le 5\times 10^5$。

致敬 `Tarjan`。考虑魔改有向图 `Tarjan` 算法，以求出环的并集。

当遇到一条返祖边 $u,v$，意味着删的点一定在链 $v\rightarrow u$ 上，打上差分标记进行链加即可。

当遇到一条前向边 $u,v$，若 `low[v]<=dfn[u]` 即 $u,v$ 在同一个环内，那么删的点一定不在链 $u,v$ 上（除去点 $u,v$ 之外），因为一定存在 $v\rightarrow u$ 的路径，$u$ 到 $v$ 的链和边 $(u,v)$ 与这条路径构成两个环。一样用差分标记进行链加。

当遇到一条横叉边 $u,v$，找到 $u$ 和 $v$ 的 $lca$，那么就相当于一条从 $lca$ 到 $v$ 的前向边。用 `Tarjan LCA` 即可做到总复杂度 $O(n\alpha(n))$。