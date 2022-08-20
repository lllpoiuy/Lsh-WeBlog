#### [[CmdOI2019] 算力训练](https://www.luogu.com.cn/problem/P5577) √

实际上就是做一个 $k$ 进制异或卷积，那么转移矩阵就是范德蒙德矩阵（容易验证 $c(i,j)\cdot c(i,k)=\omega^{i(j+k)}=c(i,j\oplus j)$）。将 $1+x^{a_i}$ 进行 $\operatorname{FWT}$ 之后得到的每一位都是 $1+\omega^a$ 的形式。我们要做的，是加速求出 $\operatorname{FWT}(\mathrm{Ans})$ 的每一位上，每种值分别用了多少次。我们用一个循环卷积来维护这个单位根，并求出 $C(x)=\sum_i x^{a_i}$ 的 $\operatorname{FWT}$，然后在每一位的多项式上就能看出每种 $1+\omega^a$ 有多少种了。

#### [小猪佩奇学数学](https://www.luogu.com.cn/problem/P5591) √

$$
\begin{aligned}
\mathrm{Ans}&=\sum_{i=0}^n\pmatrix{n\\i}\cdot p^i\cdot \frac{i-}{k}\\
k\cdot \mathrm{Ans}&=\sum_{i=0}^n\pmatrix{n\\i}\cdot p^i\cdot i - \sum_{i=0}^n\pmatrix{n\\i}\cdot p^i\cdot (i\bmod k)\\
&=\sum_{i=1}^n\pmatrix{n-1\\i-1}\cdot p^i-\frac 1k\cdot \sum_{g=0}^{k-1}g\sum_{j=0}^{k-1}\sum_{i=0}^n\omega^{(i-g)j}\cdot\pmatrix{n\\i}\cdot p^i\\
&=(1+p)^{n-1}\cdot p-\frac 1k\sum_{h=0}^{k-1}g\sum_{j=0}^{k-1}\omega^{-gj}\cdot \left(1+p\omega^{j}\right)^n\\
&=(1+p)^{n-1}\cdot p-\frac 1k\sum_{j=0}^{k-1}-\omega^{gj}\cdot \left(1+p\omega^{j}\right)^n\sum_{g=0}^{k-1}g\omega^{-gj}
\end{aligned}
$$

#### [[XR-4] 混乱度](https://www.luogu.com.cn/problem/P5598)

事实上，我们将组合数 $\pmatrix{a_l+a_{l+1}\cdots a_r\\a_l, a_{l+1},\cdots,a_r}$ 用 Lucas 定理展开后可以发现，这个组合数 $\bmod p\not=0$ 当且仅当 $\sum_{i=l}^ra_i$ 在 $p$ 进制下每一位都不进位！所以我们对于每个 $l$，向右暴力计算每个 $r$ 的答案，遇到 $0$ 就跳过（整段的对答案的贡献是一样的），遇到非 $0$ 的元素，就遍历它非 $0$ 的位进行计算，均摊一一下是 $O(np\log_p\left(10^{18}\right))$ 的。

#### [[XR-4] 文本编辑器](https://www.luogu.com.cn/problem/P5599) √

用一个维护区间的平衡树维护每个循环的区间，然后就可以正常做了，均摊复杂度是对的，每次先 $O(50)$ 往左跑一下找到在哪个节点上，再向右 $50$ 完成对后面的修正。

#### [[Ynoi2013] 文化课](https://www.luogu.com.cn/problem/P5608)

直接用线段树维护下区间和、区间前缀的连乘的积、后缀的连乘的积、中间的和，即可转移和维护区间覆盖为加法/乘法。难点在于，区间赋值为一个数怎么做？我们维护下所有本质不同的连乘长度，这个数组长度是小于等于 $O(n^{0.5})$ 的；而区间赋值时只要遍历这个数组并快速幂即可计算出新的区间和。`pushup` 的复杂度为 $n^{0.5}+\frac 12n^{0.5}+\frac 14n^{0.5}+\cdots=2n^{0.5}$，而快速幂部分，如果按照顺序从小到大进行计算，每次计算 $val^{a_i-a_{i-1}}$，复杂度则为 $\sum\log(a_i-a_{i-1})\sim O(n^{0.5})$。

#### [[Ynoi2013] 对数据结构的爱](https://www.luogu.com.cn/problem/P5609)

开一个线段树，线段树的每个节点上要保存一个 $O(len)$ 的分段函数，即记下 $c_{1\sim len}$ 表示输入至少要达到 $c_x$ 才会在最后减去 $xp$。考虑在 build 的时候如何 Pushup，在左儿子的 $c$ 数组上进行枚举，每一段 $[c_x,c_{x+1})$ 的可达区间是一个区间 $[c_x-xp,c_{x+1}-xp)$，那么它所能控制的区间其实应该是：$[\max(c_x-xp,c_{x-1}-(x-1)p+1),c_{x+1}-xp)$，双指针一下在这个区间内的右儿子的 $c$ 即可。$O(n\log{n}+m\log^2{n})$。

附一个 jbc 树的牛逼做法：[Orz zhy & jbc](https://www.luogu.com.cn/blog/ryoku/solution-p5609)。

#### [[PKUWC2018] 随机游走](https://www.luogu.com.cn/problem/P5643) √

Min-max 容斥一下，然后就变成了计算到达一个 $S$ 的最小时间花费的期望，设一个转移矩矩阵 $A$ 表示在树上随机游走，但是走到 $S$ 中之后就不出去了，则根据 $\frac 1{1-A}$ 的第 $x$ 行中非 $S$ 元素的和就能求出期望步数。还有一种办法是在树上 dp，但是自下而上求出 $f_i$ 关于 $f_{fa_i}$ 的表达式（一定是一次函数），然后就能向上转移了，到根处就能直接求出 $f_x$，然后再往下递归一遍。

#### [[PKUWC2018] 猎人杀](https://www.luogu.com.cn/problem/P5644) √

一个重要的想法是，无视猎人的死亡，每次依然还是等概率选择一个不管死没死的猎人开枪，那么我们要计算的便是第一个人一枪不挨的前提下，其它猎人都被干掉的概率，那么我们 Min-max 容斥干掉所有别的猎人需要几枪：
$$
\sum_{S}(-1)^S\sum_{i=1}^{+\infty}\left(1-\frac{\sum_{i\in S}w_i}{\sum_{i\not=1}w_i}\right)^{i-1}\cdot\frac{\sum_{i\in S}w_i}{\sum_{i\not=1}w_i}\cdot\left(\frac{w_1}{\sum_{i}w_i}\right)^i
$$
那么这个式子是一个只和 $\sum_{i\in S}w_i$ 相关的等比数列求和。利用多项式 01 背包（$\operatorname{Exp}$）求出每个 $\sum_{i\in S}w_i$ 的贡献系数即可。当然，也可以枚举 $1$ 死亡时，至少有 $S$ 集合内的人还没死去，这个可能更靠谱一点（逃

#### [EI 的第六分块](https://www.luogu.com.cn/problem/P5693)

考虑经典的区间最大子段和的线段树，每个节点上维护 $sum,lmax,rmax,tmax$ 四个值，把这四个值看作是四个一次函数，对于每个点，再记一个 $x$ 表示偏差量最小达到 $x$ 就会发生“击败”操作（一条直线取代了另一条直线去作为 $lmax$ 或者 $rmax$ 或者 $tmax$）。每次修改的时候，暴力向下递归至所有会发生击败操作的节点上。EI 证明了这个击败操作的摊还复杂度是 $O((n+m)\log^3(n))$ 的，而查询显然是 $O(\log(n))$ 的。

#### [[MtOI2019] 手牵手走向明天](https://www.luogu.com.cn/problem/P5692)

直接序列分块，对每个块，维护一个 $\sqrt n\times\sqrt n$ 的表，存下每两种颜色的最近距离，并且存下每个颜色最靠左/右的距离，于是就可以 $O(\sqrt n)$ 询问。而修改的时候，若 $x\rightarrow y$，$x$ 为空则忽略，$y$ 为空则修改一下指针即可，都不为空则需要 $O(\sqrt n)$ 的时间来减少一种颜色，但好在均摊意义下是正确的。对于散块，暴力重构那两种颜色对应的两行、两列即可。