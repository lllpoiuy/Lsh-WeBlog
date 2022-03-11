# 2021 Noi.ac 省选专题营 Day 3 笔记

时间：20210813    讲师：代晨昕    专题：数据结构   地点：线上

### 1.1 树链剖分

>**1.1.1 重链剖分**：按重儿子来划分重链，常可以解决链上问题 / 树剖 `LCA` 使很快的。重要性质：一个点到根的路径上最多有 $\log$ 条轻边。
>
>**1.1.2 长链剖分**：常用于优化深度相关的 `dp` 问题；同时，可以用于 $O(1)$ 求树上 $k$ 级祖先；重要性质：一个点到根的轻边最多有 $\sqrt{n}$ 条。
>
>**1.1.3 奇怪的剖分**：用于 `LCT` 的实链剖分；用于莫队的有 $\sqrt{n}$ 个重儿子的树剖。

### 1.2 树链剖分例题：

#### 1.2.1 [NOI 2021] 轻重边

假设一开始每个点都有一种独特的颜色，每次操作将一条链染成一种新颜色；那么一条边使轻边等价于端点颜色不同。我们直接树链剖分，线段树上维护左端点颜色、右端点颜色、相邻点颜色相同的对数。

#### 1.2.2 练习题

> 有一棵 $n$ 个点的树，每个点 $i$ 有一个物品，重量为 $1$。对 $k=1\sim m$ 求出，树上有多少个连通块物品重量和为$k$，模 $1e9+7$。$N<=200$，$m<=50000$。

事实上可以做到 $n\log^2n$。我们将其看作是多项式的卷积，直接继承重儿子的答案，然后按照哈夫曼树快速合并轻儿子们的答案。不能直接合并到重儿子上，只能先存着，相当于在重链的每个点上都挂一个多项式，最后在重链链顶再一并用哈夫曼树合并（好像是这样）。

#### 1.2.3 HDU 6566

>$n$ 个点的树，每个点有两种权值 $a_i,b_i$。你需要选出一个独立集，使得 $a_i$ 的总和恰好为 $m$，并且 $b_i$ 的总和尽量大。同时你需要计算这些独立集的数量。$n<=100$，$m<=5000$。

值域较小的树上背包是 $O(n^2)$ 的；但是值域很大的树上背包是 $n|U|^2$ 的；现在我们可以利用重链剖分 / 点分治将其优化到 $n^2|U|$。

考虑单点加入的复杂度是很优秀的，我们按照 `dfs` 序进行考虑（先轻边后重边），我们发现只需父节点的状态就能完成该点的转移；而我们可以状压下所有轻边父亲的状态，并在状态中记下当前点父亲的状态（如果是重儿子）。由于巧妙的转移顺序，这么做是对的，于是复杂度就是 $O(2^{\log_2{n}}\cdot n\cdot|U|)$。事实上，状压点分树的祖宗链也是很可以的。

#### 1.2.4 [2020牛客多校] 第六场D

> 给定一棵 $n$ 个节点的有根树，第 $i$ 个点的编号是 $i$。
>
> 有 $m$ 次询问，每次询问给出 $l,r,x$，求有多少点编号的二元组 $(i,j)$ 满足 $l <= i< j <= r$ 且 $i$ 和 $j$ 的最近公共祖先是节点 $x$。$n,m<=2e5$。

设 $f(l,r,x)$ 为 $x$ 子树内编号在 $[l,r]$ 之间的点的个数的平方，不难发现答案等于：
$$
f(x,l,r)-\sum_{u\in \mathrm{son}_x}f(u,l,r)
$$
我们直接重链剖分，对 $(l,r)$ 莫队；莫队时维护每个点的轻儿子的 $f$ 的和，我们发现每加入 / 删除一个点只会更改 $\log$ 个点；查询时只需查询重儿子和点本身的 $f$，这个直接二维偏序一个 $\log$。总复杂度 $O(n^{1.5}\log_2 n)$。

对于两部分分别有一个优化利器。对于轻儿子的部分，我们令一个点前 $\sqrt{n}$ 大的儿子为重儿子，那么一个点到根的路径上最多有两条轻边，莫队时的复杂度得以为 $O(n^{1.5})$。

这时，我们发现询问数量高达 $O(n^{1.5})$；我们平衡修改和插入的复杂度——二维偏序直接拆成一个二维前缀的问题，然后从大到小插入点，同时回答询问；插入时维护块内前缀和，这样就是 $O(n^{0.5})-O(1)$，两部分的复杂度得以平衡，。总复杂度 $O(n^{1.5})$。

#### 1.2.5 [Bzoj4543/luogu3565] 加强

> 给定一棵树，在树上选 $3$ 个点，要求两两距离相等，求方案数。$n<=1e6$。

有两种情况，一种是三个点的 `LCA` 属于这三个点中的一个点的情况，一种是 `LCA` 不属于的情况。后者容易，我们考虑前者。我们利用长链剖分优化 `dp` 维护两个数组：$f_{u,i}$ 表示 $u$ 子树中，到 $u$ 距离为 $i$ 的节点数量；$g_{u,i}$ 表示在 $u$ 子树中，点到 `lca` 距离 - `lca` 到 $u$ 距离 为 $i$ 的同深度点对的数量。

长链向上继承时，$f$ 和 $g$ 都是位移一位即可（一个向左，一个向右）；轻链向长链合并时，枚举短链数组上的每个数 $j$，执行 $f_{u,j+1}\cdot f_{v,j}->f'_{u,j+1}$；同时也向 $g$ 的 $j+1$ 这一位执行这样的操作；这一部分对答案的贡献就为 $\sum\limits{g_{u,0}}$。

#### 1.2.6 bzoj 3252

> 给定一棵树，每个点有点权，对每个合法的 $k$：选定 $k$ 个叶子，使得这 $k$ 个叶子到根的所有路径的点权和最大。

按照权值来划分长链，选出前 $k$ 长的长链作为答案即可。（正确性显然）。

### 2.1 伸展树

> **2.1.1 旋转**：下面是来自谢队的高级 `rotate`：
>
> ```cpp
> inline void rotate(int u)
> {
> 	int v = fa[u], g = fa[v], z = ch[v][1] == u, h = ch[u][z ^ 1];
> 	fa[fa[fa[ch[ch[ch[g][ch[g][1] == v] = u][z ^ 1] = v][z] = h] = v] = u] = g;
> 	pushup(v), pushup(u);
> }
> ```
>
> **2.1.2 定义**：`spaly` 是在每次查询后，均通过 `rotate` 将询问点旋转至根的二叉树；而 `splay` 是在旋转时考虑上了那种特殊情况的二叉树；时间复杂度是均摊一个 $\log$ 的。
>
> **2.1.3 势能分析**：见 `Chen_03` 的课件（）。

### 2.2 例题精选

#### 2.2.1 bzoj 3786

> 给定一棵有根树，根节点为 $1$，三种操作：1、将 $x$ 的父亲修改为 $y$；2、子树加；3、求x到根路径和。
>
> 范围大约 $1e5$。

我们维护这棵树的括号序列；具体来说，每个点分配两个在平衡树上的编号；取出子树时，我们找到这两个点在树上的前驱 / 后继（直接向左 / 右走到底），然后 `splay` 前驱和后缀，将这个区间取出；插入区间时，找到 $y$ 的左括号点的后继，分别 `splay` $y$ 和其后继，将子树接在 `ch[y][0]` 上。

如何维护路径和？我们使左括号为加权值，右括号为减去其权值，那么到根的路径和对应于一个前缀和；直接在伸展树上再打上标记即可直接维护。

#### 2.2.2 cf573e

> 给定序列 $a$，请你求出一个长度为 $m$ 的子序列 $b$，最大化 $\sum_{i=1}^m i\cdot b_i$。

从左到右增量构造。设 $f_j$ 当前选择了 $j$ 个数的最大值，加入一个点时，会存在一个分界线，使得大于等于这个分界线的都会采纳当前物品，而小于分界点的都不会。我们维护 $f$ 的差分，修改时直接大力二分出分界点，然后就成了单点插入、子树加（大于分界点的部分要把差分位移一位，再都加上 $b_i$）。

### 3.1 Link Cut Tree

> 核心函数有：
>
> `Access`：拉出一条从 $x$ 到 `root` 的实链。
>
> `Makeroot`：换根。

### 3.2 例题精选

#### 3.2.1 bzoj4530

> 支持动态加边，询问某个连通块中以 $x$ 为根时 $y$ 的子树大小。

当维护的子树信息可以 $O(1)$ 加减的话，那么 Link Cut Tree 是可以维护子树信息的——对于每个点，维护其轻儿子的信息之和，再在 `splay` 上维护和的和。注意在连边时，需要把两个点都换成根，这样才能避免整体修改。

#### 3.2.2 Zjoi2016 大森林

> 请你维护一个森林并支持三种操作：区间替换生长点、区间生长单点、单树查询两点距离。

我们发现一个重要的事情：第一个操作可以看作是对 $[1,n]$ 都操作，只不过是 $[l,r]$ 内才加上权值；

我们新建一个虚点链，每个虚点代表一个时间，每个时间连向上一个时间和它插入的值；我们从左到右扫描过去，并在扫描的过程中用 link Cut Tree 维护这棵树（时间 $j$ 上的换生长点为 $x$ 相当于 `cut(j,j+1)` 然后 `link(x,j+1)`；反之，亦能维护）。

#### 3.2.3 比赛

> 一排 $n$ 个选手，编号依次为 $0\sim n-1$，初始时有个 $x=0$；每个人有一个技能 $a_i$，可以使得 $x=(x+a_i)\bmod n$；从左到右依次发动技能，每个人会发动技能当且仅当他发动了必胜且不发动不能胜。多次单点修改 $a_i$ 问赢家是谁？

我们从右到左考虑，一个人发动技能能胜利当且仅当 $(x+a_i)\bmod n=i$，也就是 $x=(i-a_i)\bmod$；我们从右到左维护会有人胜利的 $x$ 的集合，那么当前点会发动技能当且仅当 $(x+a_i)\bmod$ 不在集合内；不难发现只会有一个人发动技能，这个人是满足 $a_i=i$ 的、之后没有 $x=a_i$ 的。

然后发现这也不一定是对的，我们将每个 $i$ （除了 $i=0$）向 $(i-a_i)\bmod n$ 连边，这是一棵内向树，我们在这棵树上标记 $f_u=[\mathrm{SG}_u>0]$，每次找到第一层儿子中最前面的、$f_u=1$ 的点；直接 `Lct` 维护这棵博弈树即可。

博弈树的修改只会修改一条从节点到根的链，直接 `Access` 一下，二分出转换状态的分界点，打标记反转即可。（为了支持这些，我们需要在节点中保存虚子树的信息，并维护一个当前子树是否 `01` 交替的信息）。

### 3.3 离线动态图连通性

用 `lct` 维护原图的一个生成树；加边时，如果成环，那么找到环上最早 `cut` 的边 `cut` 掉即可。

#### 3.3.2 cf1172e

> 一棵树，每个点上有点权，设 $\mathrm{dis}(x,y)$ 为路径 $(x,y)$ 上的颜色种类，多次修改颜色，求：
> $$
> \sum_{u,v\in V} \mathrm{dis}(u,v)
> $$

离线出来，每种颜色的贡献为不是这种颜色的点构成的连通块的大小的平方和；对于每种颜色都维护一遍，维护一棵 `lct`；注意不能暴力断边，只能断掉与父节点的连边，然后将点权设为 $0$（反之同理）；每个点都要维护其轻儿子大小的平方和（才能处理断儿子时的修改）、当然也要有当前点连通块的大小。