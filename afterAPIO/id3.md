#### [[XR-2] 记忆](https://www.luogu.com.cn/problem/P5438) √

设 $x=y\sqrt z$，我们显然是要把 $z$ 相同的都摆在一起。枚举 $z$，可以得到：
$$
\sum_{z=1}^n\mu^2(z)\cdot \max\left( \left\lfloor\sqrt\frac rz\right\rfloor-\left\lfloor\sqrt\frac lz\right\rfloor-1,0 \right)
$$
我们枚举 $\left\lfloor\sqrt\frac rz\right\rfloor$ 和 $\left\lfloor\sqrt\frac lz\right\rfloor$ 的变化，当 $\frac rz\le B$ 时，取值不超过 $B^{0.5}$ 种，对于每种分别利用 $\sum_{i=1}^n\mu^2(i)=\sum_{i=1}^n\frac n{i^2}\cdot \mu(i)$ 来 $O(n^{\frac 13})$ 来计算；当 $\frac rz\ge B$ 时，$z\le B$，利用线性筛求出前缀和。取 $B=n^{\frac 49}$，有总复杂度 $O(n^{\frac 59})$。

事实上，$\frac rz\le B$ 的情况可以利用积分得到更为准确的复杂度上界，线性筛的部分可以将大于等于某一值的部分只处理其整除模组上的值，可以将复杂度压到 $n^{\frac 37}$。

#### [[XR-2] 永恒](https://www.luogu.com.cn/problem/P5439) √

考虑枚举在 `Trie` 树上的 `lca`，然后求出这个 `lca` 贡献次数，也就是 `lca` 为它的点对的贡献和，而点对的贡献为经过它的链的数量，可以根据两个点是否含有在原树上的 `lca` 进行分类讨论。对 `Trie` 做启发式合并，过程中在原树上维护一个全局平衡二叉树，即可做到 $O(n\log^2(n))$。

#### [[THUPC2018] 赛艇](https://www.luogu.com.cn/problem/P5447) √

想了 10+ min 终于发现这竟然只是一个二维差卷积，于是就做完了。

更为高效的实现是：首先把矩阵的所有行坐标和列坐标改为以右下角为原点，于是只需要和卷积；展开成一维数组，结果是一样的（但是需要把列的坐标范围放大一倍，防止一行的贡献错算到上一行）。

#### [[THUPC2018] 好图计数](https://www.luogu.com.cn/problem/P5448)

首先考虑构造的过程，我们用若干个连通的小好图，可以拼成一个大的不连通的好图，取反可以得到一个大的连通的好图。那么我们只需要枚举一个类似整数划分或者背包的东西即可有一个 dp。设 $F$ 为连通好图的生成函数，然后可以得到方程：
$$
2F(x)=\prod_{i=1}\left(\frac 1{1-x^i}\right)^{[x^i]F(x)}+x
$$
两边同时取 $\ln$ 后可以得到一个很漂亮的式子，但难以递推，所以我们在此基础上继续求导：（记答案的生成函数为 $G=2F-x$）。
$$
\frac {2F'}{G=2F-x}=\sum_{i=1}[x^i]F(x)\cdot \frac{ix^{i-1}}{1-x^i}
$$
于是有：
$$
(n+1)[x^{n+1}]F=\sum_{i=0}^{n}[x^i]G\cdot \sum_{k|n-i+1}k[x^{k}]F
$$
设 $H_i=\sum_{k|i}k[x^{k}]F$，可以 $O(n\ln n)$ 递推出 $H$，于是就只剩下一个 $O(n^2)$ 的卷积；容易看出这是半在线卷积，可以做到 $O\left(\frac {n\log^2n}{\log\log{n}}\right)$。

事实上，$(2)$ 式取完 $\ln$ 后可以化成一个经典的形式：
$$
F(x)=\exp\left(\sum_{i}\frac {F(x^i)}{i!}\right)
$$
这个式子实际上是可以牛顿迭代的！复杂度 $O(n\log(n))$。

#### [[THUPC2018] 淘米神的树](https://www.luogu.com.cn/problem/P5450)

假设只有一个出发点，那么方案数为树的拓扑序数量，即：
$$
\frac {n!}{\prod_{i=1}^nsze_i}
$$
当有两个出发点的时候，一种想法是枚举断边，但是发现会算重；一种想法是枚举链上最后一个选的点，但是发现难以优化。但事实上，**断边的方案除以二**其实就是正确的答案，因为每种断边方案都可以把最后选的点换进另一个出发点的控制范围。现在枚举断边，问题变为快速**分别求出两棵树的方案数**，其实也就是要求出一条链上的 $sze$ 的乘积。我们可以发现它一定可以转化为以下形式（$S$ 是链上 $sze$ 的点缀和）：
$$
f(i)=\prod_{j\not=i}|S_j-S_i|
$$
分治 NTT + 多点求值即可。

#### [P5508 寻宝](https://www.luogu.com.cn/problem/P5508) √

区间连边的部分可以线段树优化建图，但是 $|i-j|\cdot v_i$ 的部分怎么办呢？其实可以建一棵李超树，然后模拟 dij 的过程每次取出最小的进行增广；这和线段树优化建图不冲突，可以同时用一个堆维护线段树上的虚点。$O(n\log^2(n))$。

#### [[MtOI2019] 埋骨于弘川](https://www.luogu.com.cn/problem/P5519) √

首先我们考虑递归地把系数传下去，算出坐标轴上的所有点的贡献系数，相当于要求出 $y$ 轴上 $0\sim k$ 的贡献系数，$x$ 轴上 $1\sim 42$ 的贡献系数。考虑传递贡献系数的过程，我们强势地给其划分阶段——不妨使得每一条 $x=?$ 一个阶段，额外记 $42$ 个数用于辅助 $x$ 轴上的转移，则转移矩阵 $-Ix$ 为：
$$
\begin{pmatrix}
1-x & 1 & 1 & \cdots & 1 \\
  & 1-x & 1 & \cdots & 1 \\
  &   & \ddots & \cdots & \vdots \\
  &   &   &   & 1-x & 1 \\
  &   &   &   & 2 &-x& 1  \\
  &   &   &   & 3 &&-x& 1  \\
  &   &   &   & \vdots &&&-x& \ddots  \\
  &   &   &   & 42 &&&-x& 0  \\
  
\end{pmatrix}
$$
不难发现上下两部分矩阵的行列式是独立的（因为高斯消元的过程是独立的），因此可以 $42^4$ 插值求出下面的行列式，再利用分治 NTT 求出上面部分的行列式。然后就正常地进行常系数齐次线性递推即可。

#### [[Ynoi2012] WC2016充满了失望](https://www.luogu.com.cn/problem/P5525)

我们发现最难以解决的是 $R$ 不同的问题，难以将问题转化到好的形式。这里有一个很厉害的 Trick：**收缩凸包**。我们把凸包上的每条边向内平移 $R$ 个单位距离，会形成一个新的多边形，如果我们能动态维护这个收缩的过程，就能离线地在凸包上二分出每个询问的答案。事实上收缩的过程是容易维护的，每个点是**沿着角平分线走**，我们只要维护所有角平分线交点里最近会达到的一个即可。

#### [[Ynoi2012] 惊惶的 SCOI2016](https://www.luogu.com.cn/problem/P5526) √

转化成对每种颜色分别求，总的修改次数只有 $O(n+m)$。而对每种颜色求的过程相当于加点、删点、维护连通块大小的平方和。利用动态 dp 可以求解；为了解决逆矩阵之类的问题，最好再写个哈夫曼树用于合并子树。$O((n+m)\log{n})$（其实也可以用 lct 来维护子树信息，维护字数大小、虚子树大小、虚子树大小平方和就可以快速维护了）。

#### [[CCO 2019] Winter Driving](https://www.luogu.com.cn/problem/P5533)

结论：存在一个中心点，使得其所有儿子要么是内向树要么是外向树，且这个中心点一定是两个重心之一。而这个儿子划分的最优方案为两边 $\sum A$ 相差最小的一个方案，那么 $2^{18}$ 处理出折半后的两个方案数组，双指针一下即可。

#### [[XR-3] Namid[A]me](https://www.luogu.com.cn/problem/P5538)

直接枚举叶子节点并分别进行一次 dfs，过程中维护出 $f$ 分成的 $w$ 段，可以 $O(ndw)$ 地求出一个会重复计算地答案。为了去掉重复计算的影响，一种办法是给每个点附上一个权表示另一个方向上的叶子个数的倒数，在 $w$ 段的栈上维护这个倒数和。另一个办法是：限制之前所有叶子节点都处在当前点的子树中，也就能保证当前计算的链以前都没有计算过。

事实上，暴力背包合并复杂度是正确的，因为 $O(\min(d^2w^2,n^2)=O(ndw)$！

#### [[CmdOI2019] 星际kfc篮球赛](https://www.luogu.com.cn/problem/P5573)

直接用 B 算法求出三个最小生成树，然后分别建出 Kruscal 重构树，对于每个重构树维护一个 Bitset 存下可达点集，分段进行此事即可。当然，这个其实是一个三维数电，所以可以 $O(m\log^2{n})$。

#### [[CmdOI2019] 简单的数论题](https://www.luogu.com.cn/problem/P5572)

先推推式子：
$$
\begin{aligned}
\mathrm{Ans}&=\sum_{i=1}^n\sum_{j=1}^m\varphi\left(\frac{\operatorname{lcm}(i,j)}{\gcd(i,j)}\right)\\
&=\sum_{d=1}^n\sum_{i=1}^{\frac nd}\sum_{j=1}^{\frac md}\varphi(ij)\cdot\sum_{e|i,j}\mu(e)\\
&=\sum_{d=1}^n\sum_{i=1}^{\frac nd}\sum_{j=1}^{\frac md}\varphi(i)\cdot \varphi(j)\cdot\sum_{e|i,j}\mu(e)\\
&=\sum_{T=1}^n\sum_{e|T}\mu(e)\sum_{i=1}^{\frac nT}\varphi(ie)\sum_{j=1}^{\frac mT}\varphi(je)\\
\end{aligned}
$$
第三行处的 $\varphi(ij)=\varphi(i)\cdot \varphi(j)$ 是利用了枚举过程中要求 $i,j$ 互质的性质；$O(n\ln{n})$ 求出 $G(x,y)=\sum_{i=1}^x \varphi(iy)$，即可得到：
$$
\begin{aligned}
\mathrm{Ans}&=\sum_{T=1}^n\sum_{e|T}\mu(e)\cdot G\left(\frac nT, e\right)\cdot G\left(\frac mT, e\right)\\
&=\sum_{T=1}^n F\left(\frac nT, \frac mT, T\right)
\end{aligned}
$$
我们可以预处理所有 $\frac nT\le L^{0.5}$ 且 $\frac mT\le L^{0.5}$ 的 $f$，复杂度为 $\sum_{i=1}^L\sum_{j=1}^L \frac{L}{\max(i,j)}\cdot\log\left(\frac{L}{\max(i,j)}\right)\sim \sum_{i=1}^L\sum_{j=1}^i\frac Li\log\left(\frac Li\right)\sim L^{1.5}\log{L}$。在询问时，这一部分也可以直接做，单次复杂度 $O(L^{0.5})$。

对于 $\frac {\max(n,m)}T>L^{0.5}$，有 $T\le L^{0.5}$，直接枚举 $T$ 的因数并计算即可，单次复杂度 $O(L^{0.5}\log{L})$。 