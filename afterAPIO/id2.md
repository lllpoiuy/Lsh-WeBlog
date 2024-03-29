#### [[Ynoi2018] 天降之物](https://www.luogu.com.cn/problem/P5397)

静态问题下，可以预处理出每种权值的数对应的子序列，然后**小的序列向大的归并**即可做到很正确的复杂度（$(O(n^{1.5}\log(n))$，$\log(n)$ 是为了保证每次询问只花 $O(\min(a,b))$ 左右的复杂度）。但这个是可以**去掉 $\log(n)$** 的，我们可以对于所有长度大于等于 $n^{0.5}$ 的都进行一遍 $O(n)$ 的扫描来处理出它与每种数之间的最短距离。

考虑动态维护这个事情。首先一定可以转化为**小的向大的加入**。我们可以对每种数，再维护一个**附属集合**。每次加入时，先将集合归并进附属集合中。**每当附属集合大于 $O(n^{0.5})$**，则将其与祝集合合并，并重新进行一次 $O(n)$ 的预处理，复杂度在均摊意义下依然正确。

#### [[Ynoi2018] GOSICK](https://www.luogu.com.cn/problem/P5398)

莫队二次离线，即将莫队移动端点时需要进行的询问进行离线处理。**利用 $O(n+m)$ 空间存下所有的二离询问**。

那么有两种转化形式：1. 若这个询问满足可减性，则可以将其转化为**前缀询问**，可以用**高复杂度插入低复杂度查询**的数据结构来快速维护。2. 进行 $O(m)$ 次区间到区间之间的代价计算。

#### [[Ynoi2018] 駄作](https://www.luogu.com.cn/problem/P5399)

树分块：将树分成不超过 $6\frac nB$ 个大小不超过 $B$ 的块，且使得每个块内，与其他块有连边的点（成为**界点**）不超过 $2$ 个。这题可以很好地利用上树分块的性质。

#### [[PKUWC2018] 随机算法](https://www.luogu.com.cn/problem/P5492) √

一种想法是从 $O(3^n)$ dp 想起，先用大状压压下所有已经被尝试的点和已经被选择的点。然后考虑少记一点信息，注意到：**不可加入独立集的点之间是等价的**，我们进行完每一次加入后，可以把此次加入后产生的**新的不可加入的点**，给**提早确定它们的加入时间**。于是就只需要记下接下来的可以加入独立集的点的集合。疑似是 $O(2^n\cdot n)$ 的（但至少不会大于 $O(2^n\cdot n^2)$）。

一种想法是，这种的随机方式，**尝试一个不可能了的点是没有用的**，其实也就相当于**从还可以加入的点中等概率选一个**。所以直接倒着 dp 就行了。

#### [[THUPC2019] 找树](https://www.luogu.com.cn/problem/P5406)

一个想法是从高到低位确定，考虑 `check` 的过程，`|` 运算的位相当于是直接把所有这一位为 $1$ 的边删掉，`&` 运算相当于存在一个 `0`，那么我们可以容斥哪几位全部为 `1`。只有 `^` 这一位最难算，只得将其转为计数问题，利用矩阵树定理套循环卷积的多元多项式（其实就是 `xor fwt`）来解决。$O(w\cdot 2^w\cdot n^3)$。

其实，不妨一开始直接求出每种答案的生成树的方案数，直接用一个每一位的运算不同的 `fwt` 来解决就行了。$O(2^w\cdot n^3)$。

#### [[CTSC2016] 萨菲克斯·阿瑞](https://www.luogu.com.cn/problem/P5417)

我的思考是：对于一个后缀数组，可以贪心地求出它的最小字符集大小——从小到大考虑，$i$ 和 $i-1$ 比较，如果能令 $s_i=s_{i-1}$ 就这么干，否则令 $s_i=s_{i-1}+1$。然后就难以思考下去了。

更加深入的办法应该是将这个贪心性质给**形式化**：对于一个后缀数组，可以求出一个形如 $s_{sa_1}\le s_{sa_2}<s_{sa_3}\le s_{sa_4}\le s_{sa_5}<s_{sa_6}\cdots$ 的一个由大于号和小于号形成的链，最小字符集大小其实就等于其中的 $<$ 个数 $+1$。设 $f:sa\rightarrow I$ 为求出排列 $sa$ 对应的链，记 $|I|$ 为 $I$ 中的 $<$ 个数，$I_0\subseteq I$ 当且仅当其小于号的集合被包含。那么问题通过提取同类项可以转化为：
$$
\sum_I \operatorname{check}(I)\cdot \sum_{f(P)=I}P
$$
对于 $\operatorname{check}(I)$ 依然可以直接对 $c$ 序列贪心；对于 $\sum[f(P)=I]$，我们可以求出 $a$ 数组为 $I$ 由 $<$ 划分成的每一段的长度，设 $g(I)=\frac {\left(\sum a_i\right)!}{\prod a_i!}$，则有 $\sum[f(P)=I]=\sum_{I_0\subseteq I}(-1)^{|I|-|I_0|}\cdot g(I_0)$。那么就可以直接设计一个基于容斥的 dp 了，因为 $g$ 函数是一个可分的 `product`。

#### [[USACO19OPEN] Valleys P](https://www.luogu.com.cn/problem/P5423) √

从小到大考虑，每次考虑值小于等于 $h'$ 且最大值等于 $h'$ 的连通块，显然这个连通块是确定的，问题就变为如何快速判断这个块里面有没有洞。

记 $X$ 为**元环**（一个 $2\times 2$ 小矩形里四个点都被选了）个数，那么有洞的个数 $=|E|-|V|+1-X$。（由欧拉公示可以得到）。

#### [[SNOI2017] 礼物 加强版](https://www.luogu.com.cn/problem/P5430)

首先，问题等价于求：
$$
s(n)=\sum_{i=n}^k i^kq^i
$$
这是一个经典问题，有 $s(n)=q^n\cdot G(n)-G(0)$，其中 $G$ 是一个不超过 $k$ 次的多项式 [证明](https://blog.csdn.net/qq_35649707/article/details/79595192)。那么根据 $s(n)-s(n-1)$ 得：
$$
G(n)=\frac{(n-1)^k+G(n-1)}{q}
$$
设 $G(0)=x$，则可以 $O(k\log_{k}k)=O(k)$ 地求出 $G(1)\sim G(k)$ 关于 $x$ 的表达式。根据其为不超过 $k$ 次的多项式，则 $k+1$ 次差分后一定为 $0$：
$$
\sum_{i=0}^{k+1}(-1)^{i+1}\cdot \pmatrix{k+1\\i}\cdot G(k+1-i)=0
$$
于是可以解出 $x$，从而使用 $O(n)$ 插值得到 $G(n)$ 的值。

#### [月宫的符卡序列](https://www.luogu.com.cn/problem/P5433)

首先 `Manacher` 一下，然后变成了一个统计类问题，一个显然的办法是在 `SAM` 上打 tag。

事实上，不同回文子串数量只有 $O(n)$ 级别，并且一定可以用一棵树来表示，树上一条从父亲向儿子的有向边表示从父亲回文子串到儿子回文子串只需要从左右各添加一个相同字母。我们把这棵树建出来，然后在上面子树求和一下，就可以 $O(n)$ 了。