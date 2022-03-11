# 2021 Noi.ac 省选专题营 Day 2 笔记

时间：20210812    讲师：冯哲    专题：网络流   地点：线上

### 1.1 最大流

> **1.1.1 流网络**：满足 $f(u,v)\le c(u,v)$ 且 $\forall u,\sum_{v}f(u,v)=0$ 的有向图。
>
> **1.1.2 最大流常见方法**：找到“流”对应的实际含义。

### 1.2 最大流模型例题

#### 1.2.1 Collector's Problem

> `Bob` 和他的朋友在收集贴纸。`Bob` 和他的朋友共有 $n$ 人。共 $m$ 种不同的贴纸。
> 每人手里有一些(可能有重复的)贴纸，并且只跟别人交换他所没有的贴纸，贴纸总是一对一交换。
> `Bob` 比他的朋友更聪明，他意识到只跟别人交换自己没有的贴纸并不总是最优的。某些情况下，换来一张重复贴纸更划算。
> 假设 `Bob` 的朋友只和 `Bob` 交换，且这些朋友只会用他们重复的贴纸来交换自己没有的贴纸。Bob最多能得到多少不同的贴纸? $2 ≤n ≤10$，$5 ≤m ≤25$。

我们将流等价于贴纸，每个人（除了 `Bob`)、每种贴纸（代表 `Bob`）我们都建一个点，每个人连向他能给出的贴纸，并被连向他想要的贴纸。最后每个贴纸向汇点连 $1$ 的边，原点向每种贴纸连代表 `Bob` 有几张，跑最大流。

正确性显然——每个人的流入流出显然可以保证数量不变和只换入没有的贴纸、只换重复的贴纸；而对于 `Bob` 来说，有用的贴纸也就最后输出的这些；很对了。

#### 1.2.2 [CF793G] Oleg and chess

> 给定一个 $N\times N$ 的格点矩形，每个格子 $(i ,j)$ 可以放一个士兵。
> 现在要求每行每列同时至多只能放一个士兵，同时，还有 $M$ 个矩阵 $(a,b,c,d)$, 表示 $[a,b]\times [c ,d]$ 中不能放置士兵。问最多能放多少个士兵？$1 ≤n,q ≤10000$，保证矩阵不相交。

这就是一个普通的二分图匹配问题，但是少掉了一些边；我们可以发现，剩余的空白矩形只有 $O(m)$ 个。我们把矩形拆成上边界和下边界，放在一起排序一下；对于上边界，我们统计它上方的矩形；对于下边界，我们统计它两边的矩形，拿一个 `set` 维护区间即可找到所有矩形。

知道矩形后，就相当于区间向区间连边，直接线段树优化建图即可做到 $O(n\log{n})$ 建图；通过谢队法可以做到 $O(n)$ 建图。（谢队这做法真滴牛逼）。

### 2.1 最小割

> **2.1.1 定义**：最小割的定义很重要，一种定义是找到一组边集使流网络不连通；一种定义是给图划分成两个点集，一个归 $S$ 一个归 $T$；这两个定义显然等价，而这个等价性是很厉害的武器（特别是 `dp` 优化最大流 / 最小割时）。
>
> **2.1.2 定理**：最大流等于最小割。
>
> **2.1.3 平面图**：平面图最小割等于其对偶图最短路，这是优化最小割 / 最大流问题复杂度的利器之一。
>
> **2.1.4 可行边与必须边**：可行的条件是满流且残余网络中 $u$ 不能到 $v$；必须的条件是满流且 $s$ 能到 $u$ 且 $t$ 能到 $t$；直接把残余网络跑 `Tarjan` 就行了（包括反向边），$u$ 和 $v$ 不在一个强连通分量内则说明可行；$u$ 和 $s$ 在一个强连通分量内且 $t$ 与 $v$ 在一个强连通分量内则说明必须。

### 2.2 最小割模型例题选讲：

#### 2.2.1 [BZOJ2132]圈地计划

> 有一块 $N\times M$ 的土地。每个格子上可以建商业区和工业区，收益分别为 $A_{i,j}$ 和 $B_{i ,j}$.同时，假设格子 $(i ,j)$ 四周有 $K$ 块和 $(i ,j)$ 类型不同的格子，则还能额外收益 $K\times C_{i ,j}$。问如何分配建设使得收益最大?
> $N ,M ≤100$。

每个点，从源点向它连 $A_{i,j}$，且向汇点连 $B_{i,j}$；每一种割对应于一种选择方案；假如是相同才有收益，我们可以直接将每点连向它四周的点；不同的格子有收益，我们直接黑白染色，白色点的源汇权值反过来就行了。

#### 2.2.2 [Sum of Abs](https://vjudge.net/problem/AtCoder-arc107_f/origin)

见 [this](https://lllpoiuy.github.io/Lsh-WeBlog/D3.pdf) 的 t2。可见，最小割模型的精华在于，把每一种只能多选一的决策放在一条从源到汇的路径上，并用一些边表示限制条件（如果是最大化，答案的统计一般用总的减去扣掉的）。

#### 2.2.3 Magic

> 有一张 $n$ 个点，$m$ 条边的有向图，每个点有个点权 $A_i$。对于每个点，你需要付出 $\max_{(i ,j)∈E} Aj$ 的代价。
> 幸运得是，你可以修正一些点的 $A$ 来减小代价：对于每个点 $i$ ,你可以花费 $B_i$ 的代价使 $A_i$ 变成 $0$。
> 求最小的代价。
> $n ≤1000$，$m ≤50000$,图随机生成，$A_i$ 不重复，不存在自环。

这题中，“决策链”的意味更加明显：将每个点周围一圈的值从大到小排序建出一条链，直接以值作为边权；链上每个点连向它的另一端点的虚点（边权 `inf`），每个虚点向 $T$ 连一条边权为 $B_i$ 的边。

### 2.3 最大权闭合子图

还有一种常见的最小割建模方法，就是构造“最大权闭合子图”的限制。

### 3.1 费用流

> **3.1.1 分类**：分为最小费用最大流和最小费用可行流（区别在于终止条件不同）。
>
> **3.1.2 线性规划**：等价于，每个变量都只出现一次或两次的线性规划问题（直接通过流量守恒来等价）。
>
> **3.1.3 带反悔的贪心**：费用流与其等价（退流即为反悔）。
>
> **3.1.4 线性规划对偶问题**：咕咕咕。

### 3.2 费用流例题精选：

#### 3.2.1 序列

> 给出一个长度为 $n$ 的正整数 $C_i$。选出一个子序列，使得原序列的任意一个长度为 $m$ 的连续子序列中，被选出的元素数量不超过 $k$ 个。最大化选出的子序列中元素的和。
> $1 ≤n ≤1000$，$1 ≤m,k ≤100$。

费用流中，流的意义可能会变得很含糊；例如这题中，我们将每个点 $i$ 向 $[i+m,n]$ 连边，从 $S$ 向 $1$ 连一条大小为 $k$ 的边。后缀和优化建图即可通过；正确性不是那么显然但也不是很难意识到（）。类似做法的还有 `NOI` 的流水线。

#### 3.2.2 [WC2007]剪刀石头布

> 在一些一对一游戏的比赛中，我们经常会遇到 $A$ 胜过 $B$，$B$ 胜过 $C$ 而 $C$ 又胜过 $A$ 的有趣情况，不妨形象的称之为剪刀石头布情况。有的时候，无聊的人们会津津乐道于统计有多少对无序三元组 $(A,B,C)$，满足其中的一个人在比赛中赢了另一个人，另一个人赢了第三个人而第三个人又胜过了第一个人.
> 有 $N$ 个人参加一场这样的游戏的比赛，赛程规定任意两个人之间都要进行一场比赛：这样总共有 $\frac {n(n−1)}2$ 场比赛。比赛已经进行了一部分，我们想知道在极端情况下，比赛结束后最多会发生多少剪刀石头布情况。即给出已经发生的比赛结果，而你可以任意安排剩下的比赛的结果，以得到尽量多的剪刀石头布情况。
> $1 ≤n ≤100$。

易见答案为：
$$
\pmatrix{n\\3}-\sum_{i=1}^n\pmatrix{\mathrm{ingree_i}\\2}
$$
由于一次项的和是固定的，现在就是要最小化二次项。二次项用费用流维护有一个经典做法是：建 $n$ 个虚点，向汇点连费用为 $1\sim n$、容量为 $1$ 的边，这样就能用费用流来描述二次的东西。

#### 3.2.3 

### 4.1 上下界

> **4.1.1 上下界无源汇循环流**：假设每条边都流下界，建一张新图，每条边容量为上界减下界，每个点向虚源 / 虚汇连边以补齐流量平衡，跑最大流即可；存在可行流等价于虚边满流。
>
> **4.1.2 上下界有源汇可行流**：直接汇点连向原点，变成循环流。
>
> **4.1.3 上下界有源汇最大流**：直接在可行流基础上，删掉 $t\rightarrow s$ 的边，再跑一次，初流 + 这次的流就是答案。
>
> **4.1.4 费用最小**：显然上面的这些东西改成费用流也都成立。

### 4.2 上下界网络流例题精选：

见 [WF2011]Chips Challenge，挺模板的。

### 5.1 二分图

> **5.1.1 定义**：没有奇环的无向图。
>
> **5.1.2 常见结论**：最大匹配 = 最小点覆盖，最大独立集 = 点数 - 最大匹配。
>
> **5.1.3 Hall 定理**：二分图存在完美匹配等价于每个左部点集的大小都小于等于它的邻集。事实上，最大匹配 = 
> $$
> |X|-\max(|W| - |N(W)|)
> $$
> **5.1.4 DAG**：最长反链 = 最小链覆盖 = 点数 - 最大匹配。
>
> **5.1.5 最大团**：最大团 = 补图最大独立集。

### 5.2 二分图相关例题：

#### 5.2.1 [ARC076F]Exhausted?

> 有 $M$ 把椅子，第 $i$ 把椅子的编号是 $i$。有 $N$ 个人，第 $i$ 个人只愿意坐编号 $≤L_i$ ,$≥R_i$ 的椅子。
> 现在可以加一些高级椅子，每个人都愿意坐高级椅子。问最少加多少高级椅子使得所有人都能够坐上椅子?
> $1 ≤N ,M ≤2 *10^5$，$0 ≤L_i < R_i ≤M + 1$。

直接二分图最大匹配会超时，我们考虑用拓展 `Hall` 定理；注意到：
$$
N(W)=\max_{i\in W}L_i+n-\min_{i\in W}R_i+1
$$
我们枚举一下这个 $\min(R)$，直接以枚举的这个值计入答案（显然“劣等于”）；开一棵线段树，对于每个 $\max(L)$ 维护左侧最多有多少个点；维护整体最大值（初始值为 $\max(L)$），就是答案。

### 5.3 最大权匹配

常常使用 `KM`；当然大部分时候都可以用费用流，费用流甚至可以求出顶标，挺 nb 的。