# SYC2022 Day1 启发式算法

## Part 1. 贪心和构造

### 1.1 贪心：

> **解题思路：**猜结论（基于数学直觉或者做题累积的套路）+证明
>
> **证明方式（数学层面）：** 使用反证法（交换任意或相邻元素答案不会更好）、归纳法（由 $n-1$ 推 $n$）；
>
> **证明方式（实战层面）**：——写个数据生成器、再写个暴力，对拍 30min 不出锅，那就是结论对了。
>
> **常见解法：** 1. 排序（一般基于大胆的猜结论，得到不同解的优劣关系） 2. 反悔（接受当前选项，每一次有新选择后，与当前选项对比，选择更优的，常与数据结构、网络流等结合。

### 1.2 构造：

> **解题思路：**猜结论（基于数学直觉或者做题累积的套路）
>
> **常见解法：** 1. 归纳法：假设有 $n-1$ 的解，尝试构造出 $n$ 的解 2. 证明（猜测）问题所对应的理论最优解，尝试对着这个理论最优值构造相应的解 3. 打表找规律：小范围数据爆搜打表，利用人类智慧观察小范围数据解的特征，类比得解。

### 1.3.1 例 1

> 定义有向图直径为两个距离最远的点之间的距离.问如何构造 $n$ 个点，每个点出度均为 $m$ 的有向图，使得直径最小？若想获得满分，构造出图的直径允许比最优解多至多 $1$。$n\le 1000,m\le 5$。

这个题的理论最优解是什么呢？设理论最优解为 $d$，则 $1$ 号点可到达的点数 $=n<1+m+m^2+\cdots+m^d\le m^{1+d}$。设 $f=\lceil\log_{m}n\rceil$，于是可以得到 $f\le d+1$。（也就是说这种“比最优解多不超过一”可能是经过放缩得到的）。

我们首先构造一个环，以保证图是强连通的；而环是环同构的，比较容易从取模的角度入手，从而变成一个数学问题。

对于每个点 $i$，分别向点 $(im+j)\mod n$ 连边，于是从每个点除法均可到达所有 $m$ 进制的整数，于是这就是对的了。

### 1.3.2 例 2

> 给定 $n\times m$ 的 `01` 矩阵，每次可以任意翻转一行或一列 (`0` 变 `1`，`1` 变 `0`）。请最小化矩阵中 $1$ 的数量。$n\le 20,m\le 10^5$。

枚举每一行是否翻转，设行翻转的向量记为 $s$，那么 $ans_s=\sum_{i}\min(|x_i\oplus s|,n-|x_i\oplus s|)$。

发现数据结构维护并不容易，考虑用测度式的方法快速统计——变形得 $ans_s=\sum_v\min(|v\oplus s|,n-|v\oplus s|)\cdot cnt_v$。我们发现这**居然**是一个异或卷积的形式！

### 1.3.3 例 3 [WC2018] 通道

> 给定 $3$ 棵树。
>
> 设 $d1(a,b)$ 表示 $a,b$ 两个点在第 $1$ 棵树的距离，$d2(a,b)$ 表示 $a,b$ 两个点在第 $2$ 棵树的距离，$d3(a,b)$ 表示$a,b$ 两个点在第 $3$ 棵树的距离。
>
> 求满足 $d1(u,v)+d2(u,v)+d3(u,v)$ 最大的点对 $(u,v)$。$n<=10^5$。

正解做法当然是经典的边分治套虚树（然后用归并排序消 $\log$）并嵌套的经典方法。但是，这道题我们发现非常容易是深度比较深的叶子节点。按照 $dep1$、$dep2$、$dep3$、$dep1+dep2$、$dep1+dep3$、$dep1+dep2+dep3$ 分别排序，取分别的前 $60$ 名，与其他点两两配对试试看。复杂度 $O(360n)$。

## Part 2. 搜索 & 剪枝

### 2.1 搜索

> **搜索：**穷举所有的可能解并逐一判断。
>
> **剪枝：**通过设定条件，break掉确定不可能成为解的分枝，加快搜索进程。
>
> **折半搜索：**从起点开始搜一半深度，再从终点倒推一半深度，看两者能否顺利接在一起。

### 2.2 爬山

> **定义：**随机搜索起点，每次随机一个方向跳一步，如果更优则接受，反之就跳回来。本质上依然是对状态空间的枚举。
>
> wiki：爬山算法一般会引入**温度参数**。类比地说，爬山算法就像是一只兔子喝醉了在山上跳，它每次都会朝着它所认为的更高的地方（这往往只是个不准确的趋势）跳，显然它有可能一次跳到山顶，也可能跳过头翻到对面去。不过没关系，兔子翻过去之后还会跳回来。显然这个过程很没有用，兔子永远都找不到出路，所以在这个过程中兔子冷静下来并在每次跳的时候更加谨慎，少跳一点，以到达合适的最优点。兔子逐渐变得清醒的过程就是降温过程，即温度参数在爬山的时候会不断减小。
>
> **关于降温：**降温参数是略小于 1 的常数，一般在 $[0.985, 0.9999999]$。这个温度使得不会落入次优解，但是增大了时间开销。

### 2.3.1 例 1 [NOIP2012] 文化之旅

> 有一位使者要游历各国，他每到一个国家，都能学到一种文化，但他不愿意学习任何一种文化超过一次（即如果他学习了某种文化，则他就不能到达其他有这种文化的国家）。
>
> 不同的国家可能有相同的文化，不同文化的国家对其他文化的看法不同，有些文化会排斥外来文化 （即如果他学习了某种文化，则他不能到达排斥这种文化的其他国家）。
>
> 现给定各个国家间的地理关系，各个国家的文化，每种文化对其他文化的看法，起点和终点（在起点和终点也会学习当地的文化），国家间的道路距离。试求从起点到终点最少需走多少路。国家数，文化数 $≤ 100$。

这题被证明是 NPC 的，直接爆搜即可。**正着搜 $30$ 分，倒着搜 $90$ 分！**

### 2.3.2 例 3 [WC2018] 通道

这道题同样可以用爬山法通过，随机点对 $(a,b)$，每次随机一个点 $c$，看看 $(b,c)$ 或 $(a,b)$ 是否更为优秀。

### 2.3.3 例 3

> 定义图的密度为：图中三角形（即互相有连边的三元组）数量 除以 节点数。
>
> 给定 $n$ 个节点无向图，求最大密度子图。$n\le 50$。

先 `01` 二分，然后暴力遍历每个环，每个环依赖于三个点，这就变成了一个经典的最大权闭合子图问题。

但事实上，退火稳过。

## Part 3 随机化、位运算、乱搞

### 3.1 位运算

> 定义和使用：https://oi-wiki.org/math/bit/。
>
> **注意点：**右操作数为负数或大于左操作数位数时都是未定义行为。对负数的左移操作也是未定义的。
>
> **位运算内建函数**：`__builtin_???`。
>
> `ffs` 表示末尾最后一个 $1$ 的位置，位置的编号从 $1$ 开始（数为 $0$ 时返回 $0$）。
>
> `clz` 表示前导 $0$ 的个数（数为 $0$ 时未定义）。
>
> `ctz` 表示末尾 $0$ 的个数（数为 $0$ 时未定义）。
>
> `popcount` 表示其中 $1$ 的个数。

### 3.2 随机化

> **首要的事情：**https://oi-wiki.org/misc/random/

### 3.3.1 例 1 [THUSCH2017] 巧克力

> 定 $n\times m$ 的矩阵，每个位置有权值 $a_{i,j}$ 和图案 $c_{i,j}$（$a_{i,j}$ 非负，$-1\le c_{i,j}\le nm$）。
>
> 对于 $c_{i,j}=-1$ 的位置，不可以被选中。要求选出一个连通块，至少包含 $k$ 种图案，且连通块大小尽可能小。在满足以上要求前提下，要求权值中位数尽可能小。$n\times m\le 233，k\le 5$。

如果 $m=k=5$ 怎么做？连通块大小尽可能小，这个问题接近于斯坦纳树的问题。记 $f_{S,i}$，表示已经有了颜色集合 $S$ 且当前在点 $i$ 的最少格子数，两种转移，一种是把关键点移动到旁边，一种是选择一个同关键点的其他子树把它合并进来。

但是现在 $m$ 可能很大。我们把所有颜色随机赋值为 $[1,k]$，然后直接按照上述方法做。显然可以保证颜色不同，且正确率为 $\frac{k!}{k^k}$。$100$ 次即可使失误率降至 $0.01$ 以下。

### 3.4 杂谈

前面说的，无论是贪心、退火还是随机化，真正成为题目标算的，在现在的算法竞赛中，无疑是少之又少，更多还是作为乱搞骗分的手段出现。

但涉猎还是必要的。毕竟，谁会跟分数过不去呢？

## Part 4 博弈 & SG 函数

### 4.1 博弈 (Game Theory)

> **运筹学中的博弈论**：通常需要去分析纳什均衡和判断稳定状态，涉及多人博弈。
>
> **算法竞赛中的博弈论**：大多是双人博弈、组合游戏，判断谁能获胜，或者对获胜态进行计数的问题。
>
> **主要思路**：找规律、猜结论、SG函数、套路。
>
> 如何写组合类博弈的暴力？$\alpha-\beta$ 搜索即可。

### 4.2 SG 函数 (Sprague-Grundy)

> SG 函数为估测一个博弈局面胜负的函数。
>
> **性质 1**：$SG(X)=\operatorname{mex}\limits_{X_i\in next(X)}$。
>
> **性质 2**：$SG(A)=\oplus SG(A_i)$。
>
> 注意，此处的 $SG$ 要求无出度的点的函数值必须为 $0$，不然不会满足上面这两条性质。

### 4.3 Nim 系列博弈

>**Nim 博弈**：许多堆石子，轮流操作，每次取走一堆石子中的一些石子，不能取的为输。若异或和为 $0$ 则先手败。
>
>**Misere Nim 博弈**：先取完的人失败。若所有石子个数都是 $1$ 则判奇偶性；否则判定方法同普通 Nim。
>
>**Zero-Move Nim 博弈**：每堆石子只允许进行一次 Zero-Move。做法依然是直接找去每一堆的 `SG` 值异或起来。

### 4.4.1 例 1

>有 $n$ 堆非空石子，每堆石子的数量由你钦定，但是不能超过 $2^m-1$ 个。
>
>同时，要求任意两堆石子的个数不同。给定 $n,m$，求有多少种情况 Nim 游戏先手必胜，答案取模输出。数据范围：$n\le 1e7$，$m\le 1e18$。

设 $f_i$ 表示 $i$ 个数两两不同且异或和为 $0$ 的方案数。

用总方案数减去异或和不为零的方案数，即 $f_i=A_{2^m-1}^i-f_{i-1}$（在 $0$ 的后面添加任意一个数一定会破坏异或和为 $0$ 的性质）。但是还要减去出现两个相同的数的情况，于是减去一个 $(2^m-1)\cdot(i-1)\cdot f_{i-2}$。

### 4.4.2 例 2

> 有一个 $n\times m$ 的棋盘，每一步可以从 $(x,y)$ 移动到 $(x+1,y)$,$(x,y+1)$,$(x+k,y+k)$，不能移动到棋盘外。
>
> 从 $(1,1)$ 开始，不能移动者负，Alice 和 Bob 都按照最优策略移动，问谁赢 数据范围：$n,m,k\le 1e9$。

若 $\min(n,m)\bmod (k+1)=0$ 则先手必胜；否则若 $\min(n,m)/(k+1)$ 与 $n+m$ 奇偶性相同则先手必胜，否则先手必败。

### 4.4.3 例 3

> Sam 和 Jon 在玩 Nim 游戏，但是他们厌倦了 Nim 游戏，于是换了一个规则：
>
> 每一次一个人在一堆石子中选择某个数目的石子拿走后 另一个人就不能在同一堆石子中选择同样数目的石子拿走。其他规则不变，两边都按最优策略走，问谁能赢。$N\le 10^6, X_i\le 10^{18}$。

显然每个人都是独立的，问题变成了求每个人的 SG 函数。打表可以发现 SG 函数形如：`1 1 2 2 2 3 3 3 3 4 4 4 4 4 5 5 5 5 5 5`。于是就 $O(n)$ 地解决了。

证明是：每堆石子最多取大约 $\sqrt{a}$ 次；每次操作都会使操作次数的上限减少若干——也就等价于在操作上限上进行的普通 Nim。

### 4.4.4 例 6

> Shrek 和 Donkey 正在玩一个纸牌游戏。他们总共有 $(n+m+1)$ 张两两不同的纸牌，Shrek 有 $m$ 张，Donkey 有 $n$ 张，还有一张在桌子上。每次一个玩家有两种选择 第一种是报出一张牌，另一个玩家需要如实说出自己有没有这张牌；第二种是猜测桌子上的牌是什么，猜对自己赢，猜错则对方赢 显然这个游戏并没有什么必胜策略，两人都希望最大化自己获胜概率，且两人都绝顶聪明 给定 $n,m$，且 Shrek 先手，问 Shrek 最终取胜的概率是多少. 数据范围：$n,m<=1000$。

存疑？大概是每个人有三种决策——欺骗（猜一个自己手中的数）、正经询问、猜测。设 $f_{i,j}$ 

### 4.4.5 例 7

> 有一个密码锁，两个人一起玩游戏，给出初始的密码，规定：每一次都可以转动一个位置的数字一个单位，但不可以转动到已经出现过的密码 每个位置都有一些数字损坏，因此也不可以转动到损坏的数字；无法转动的人失败，问谁能获胜。数据范围：密码锁的位数不超过 5，每个位置都由 0-9 组成，但分别有所损坏。

我们把点集按照所有维度坐标的和的奇偶性分成两部分，我们发现每次转移必然要走到另一部点；建出这张图，运用二分图博弈的结论——如果出发点一定在二分图最大匹配里，那么先手必胜；否则后手必胜。