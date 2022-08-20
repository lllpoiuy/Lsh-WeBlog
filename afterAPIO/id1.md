### [[CF1677F] Tokitsukaze and Gems](https://codeforces.com/problemset/problem/1677/F)

#### 题意：

> 给定序列 $a_{1\sim n}$，令 $G(l,r)=[0,0,\cdots,a_l,a_{l+1},\cdots,a_{r-1}, a_r,0,0,\cdots,0]$。$n,k\le 10^5,p^2\not=1,a_i\le 998244352$，求：
> $$
> \sum_{l=1}^n\sum_{r=l}^n\sum_{[t_1,t_2,\cdots,t_n]\subseteq G(l,r)}\left(\sum_{i=1}^n[t_i>0]\right)\cdot\left(\sum_{i=1}^np^{t_i}\cdot t_i^k\right) \pmod{998244353}
> $$

#### 题解：

考虑拆为每个 $i$ 的贡献，得到：
$$
\begin{aligned}
\mathrm{Ans}&=\sum_{i=1}^n\left( \sum_{j=1}^{a_i}p^i\cdot i^k \right)\cdot 
\sum_{l=1}^i\sum_{r=i}^ncalc(l,r,i)\\
&=\sum_{i=1}^n\left( \sum_{j=1}^{a_i}p^i\cdot i^k \right)\cdot
\left(
	x\cdot
	\left(\sum_{j=i}^n{\prod_{q=i+1}^j(1+a_qx)}\right)\cdot
	\left(\sum_{j=1}^i{\prod_{q=j}^{i-1}(1+a_qx)}\right)
\right)'(1)
\end{aligned}
$$
而有个很好的事情是：已知 $(f(1),f'(1))$ 和 $(g(1),g'(1))$ 可以推出 $((f\times g)(1), (f\times g)'(1))=(f(1)\cdot g(1),f(1)\cdot g'(1)+f'(1)+g(1))$，所以后面那坨东西可以用很方便的 dp 来维护。那么问题就变为了求 $\sum_{j=1}^{a_i}p^i\cdot i^k$，事情就交给 EI 科技了。

### [[CF1666K] Kingdom Partition](https://codeforces.com/problemset/problem/1666/K)

#### 题意：

> 有一张 $n$ 个点、$m$ 条边的无向边带权图，请在每个点上表上一个 `A/B/C`，最小化总代价（强制 $a$ 点为 `A`，$b$ 点为 `B`），代价为每条边的代价的和，每条边的代价计算方式如下：
>
> |       | A    | B    | C    |
> | ----- | ---- | ---- | ---- |
> | **A** | $2w$ | $0$  | $w$  |
> | **B** | $0$  | $2w$ | $w$  |
> | **C** | $w$  | $w$  | $0$  |

#### 人类智慧题解：

将每个点拆成两个点，令每条边拆成两条边 $(u,v)\rightarrow(u,v')+(u',v)$，那么看看这张新图的贡献表：

|        | ST   | TS   | SS   | TT   |
| ------ | ---- | ---- | ---- | ---- |
| **ST** | $2$  |      | $1$  | $1$  |
| **TS** |      | $2$  | $1$  | $1$  |
| **SS** | $1$  | $1$  |      | $2$  |
| **TT** | $1$  | $1$  | $2$  |      |

令 $ST=A$，$TS=B$，$SS=C$,$TT=C$，那么唯独的区别就是 $SS-TT$ 和 $TT-SS$。但事实上，我们令图中所有的 $TT$ 都变为 $SS$，会发现答案一定是缩小的，所以要么没有 $TT$ 要么没有 $SS$。所以 $O(\mathrm{dinic})$。

### [[agc051e] Middle Point](https://atcoder.jp/contests/agc051/tasks/agc051_e)

#### 题意：

> 平面上最初有 $n$ 个点 $\vec{v_{1\sim n}}$，你可以进行 finite 次操作，每次操作选择两个已经存在的点，将其中点画在平面上。问最后能得到多少个整数点？坐标绝对值小于等于 $10^9$，保证任意三点不共线。

#### 题解：

首先试着 `check` 一个整点 $(X,Y)$ 是否可以 plot。有三个条件要满足：

1. 在初始点构成的凸包内。
2. 若在凸包的边界上，那么容易判断并数出来。
3. 若在凸包内部，则需要存在一组**非负整数** $w_{1\sim n}$ 和 $k$ 使得 $\sum_{i=1}^nw_i\cdot \vec{v_i}=(X,Y)\cdot 2^k$ 且 $\sum_{i=1}^n w_i=2^k$。

第三个条件是最为棘手的，`rng_58` 告诉我们，[第三个条件] 等价于：存在一组**整数** $w_{1\sim n}$ 使得 $\sum_{i=1}^n w_i\cdot \vec{v_i}=(X,Y)$ 且 $\sum_{i=1}^nw_i=1$。证明他没告诉我们。一个比较有效的转化是：令 $a_i\leftarrow a_i-a_1,a_1\leftarrow 0$，那么方程里就多了一个自由元 $w_1$，所以 $\sum_{i=1}^nw_i=1$ 的方程相当于被去掉了。而剩余的那个东西大致是一个二元裴蜀定理的形式，详见 [[CERC2015]Looping Labyrinth](https://linshey.blog.luogu.org/solution-p4356#)（感谢 cyl）。而满足这个二元方程有解的整数对 $\{(X,Y)\}$ 一定可以被描述为 $a\vec{i}+b\vec{j}$ 的形式。我们可以直接用裴蜀定理和拓展欧几里得算法来求出 $\vec{i}$ 和 $\vec{j}$，下面介绍一个更为简洁的办法：

我们增量地维护 $\vec{i},\vec{j}$，每次新增一个 $\vec{k}$，我们试图通过类似欧几里得的办法把三个向量消成两个，剩下的一个是 $(0,0)$，不妨设 $\left|\vec{i}+\lambda\vec{j}\right|<\max\left(\left|\vec{i}\right|,\left|\vec{j}\right|\right)$（不难发现这样的一对向量一定存在），那么令 $\vec{j}\leftarrow \vec{i}+\lambda\vec{j}$，生成空间不变，且可以使三个向量的幅长的最大值减小，于是便可以在 $O\left(\log{\left|\mathrm{MAX}\right|}\right)$ 的时间内完成增量（Thanks https://codeforces.com/blog/entry/86100 @[ecnerwala](https://codeforces.com/profile/ecnerwala)）。

于是斜角坐标系 $x\vec{i}+y\vec{j}$ 内的所有坐标的最简分母为 $2$ 的次幂的点都可以满足 [第三个条件]。事实上，我们可以进行如下的一个欧几里得法，使得对于斜角坐标系 $x\vec{i'}+y\vec{j'}$ 来说，只有整数点满足 [第三个条件]：

```cpp
for (int i = 1; i <= 100; i++)
{
    while (bs[1].x % 2 == 0 && bs[1].y % 2 == 0) bs[1].x /= 2, bs[1].y /= 2;
    while (bs[2].x % 2 == 0 && bs[2].y % 2 == 0) bs[2].x /= 2, bs[2].y /= 2;
    if ((bs[1] + bs[2]).x % 2 == 0 && (bs[1] + bs[2]).y % 2 == 0)
    {
        auto e1 = bs[1] + bs[2], e2 = bs[1] - bs[2]; bs[1] = e1, bs[2] = e2;
    }
}
```

正确性我也不知道为啥，我试了很多其它的办法都不行，然后就想到这个东西也许有用（逃）。然后只要将凸包上的点转为这个新的斜角坐标系下的点，然后用皮克定理（$S=n+\frac s2-1$）求出凸包内的可达的整点个数即可。复杂度可能是 $O(n\log{U})$。

