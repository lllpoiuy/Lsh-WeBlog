| 编号 |                             题目                             | AC sub |     Sol Link     | done |
| :--: | :----------------------------------------------------------: | :----: | :--------------: | :--: |
|  1   | [CF1534H Lost Nodes](https://www.luogu.com.cn/problem/CF1534H) |        | [Link](#table1)  |  √   |
|  2   | [CF1523G Try Booking](https://www.luogu.com.cn/problem/CF1523G) |        | [Link](#table2)  |  √   |
|  3   | [P7607 [THUPC2021] 赌徒问题](https://www.luogu.com.cn/problem/P7607) |        |                  |      |
|  4   | [P7611 [THUPC2021] 幸运位置](https://www.luogu.com.cn/problem/P7611) |        | [Link](#table4)  |  √   |
|  5   | [CF1515I Phoenix and Diamonds](https://www.luogu.com.cn/problem/CF1515I) |        | [Link](#table5)  |  √   |
|  6   | [CF1477E Nezzar and Tournaments](https://www.luogu.com.cn/problem/CF1477E) |        | [Link](#table6)  |  √   |
|  7   | [CF1434E A Convex Game](https://www.luogu.com.cn/problem/CF1434E) |        | [Link](#table7)  |  √   |
|  8   | [D. Same Descent Set](https://atcoder.jp/contests/agc060/tasks/agc060_d) |        | [Link](#table8)  |  √   |
|  9   | [D. Black and White Tree](https://atcoder.jp/contests/agc014/tasks/agc014_d) |        |                  |      |
|  10  | [E. Blue and Red Tree](https://atcoder.jp/contests/agc014/tasks/agc014_e) |        |                  |      |
|  11  | [D. Median Pyramid Hard](https://atcoder.jp/contests/agc006/tasks/agc006_d) |        |                  |      |
|  12  | [E. All Pair Shortest Paths](https://atcoder.jp/contests/arc158/tasks/arc158_e) |        |                  |      |
|  13  | [F. Random Radix Sort](https://atcoder.jp/contests/arc158/tasks/arc158_f) |        |                  |      |
|  14  | [E. Non-Adjacent Matching](https://atcoder.jp/contests/arc156/tasks/arc156_e) |        |                  |      |
|  15  | [#6398. 「THUPC 2018」生生不息 / Lives](https://loj.ac/p/6398) |        |                  |  ×   |
|  16  |       [#2549. 「JSOI2018」战争](https://loj.ac/p/2549)       |        |                  |      |
|  17  |    [#3347. 「APIO2020」有趣的旅途](https://loj.ac/p/3347)    |        |                  |      |
|  18  |  [#3277. 「JOISC 2020 Day3」星座 3](https://loj.ac/p/3277)   |        |                  |      |
|  19  |  [#6499. 「雅礼集训 2018 Day2」颜色](https://loj.ac/p/6499)  |        | [Link](#table19) |  √   |
|  20  |        [【IOI2016】messy](https://uoj.ac/problem/239)        |        |                  |      |
|  21  | [Winding polygonal line](https://www.luogu.com.cn/problem/CF1158D) |        |                  |  √   |

### <a id="table1">Sol 1</a>

考虑类似树剖的过程，摁换根 dp 就行了。

### <a id="table2">Sol 2</a>

注意到对于任意 $x$，最多有 $\frac nx$ 个人入住。所以只要快速找到所有要入住的人就行了，这个就容易了。

### <a id="table4">Sol 4</a>

首先 $a\bot b$，否则可以同时除以 $\gcd(a,b)$，若 $\gcd(a,b)$ 不与 $c$ 互质则无解，否则存在逆元可以除掉。

将 $c$ 分解为 $d\cdot e$，使得 $e\bot a$ 且 $e$ 极大。那么有 $b\bot d$，否则会导致 $a\not\bot b$；于是 $an+b\bot d$，所以只需要保证 $an+b\bot e$，由于 $a\bot e$，不妨解出 $an+b\equiv 1\pmod{e}$。

考虑如何分析复杂度：一次除法 $a/b$ 的复杂度是 $\ln b\cdot (\ln a-\ln b)$ 的，这东西的总复杂度可以分析为平方的。

### <a id="table5">Sol 5</a>

设当前 $c\in[2^k,2^{k+1})$，那么所有 $v\le 2^k$ 都可以被买得起（直到 $c<2^k$，如果发生这个问题不大，因为已经除二了）；设所有 $v\le 2^k$ 的数的前缀和为 $S_{k,i}$，那么一个 $v_i> 2^k$ 能被买得起当且仅当 $v_i+(S_{k,now}-S_{k,i})\le c$，直接线段树上二分就行。依次对每个 $k$ 做，总复杂度 $O(n\log n\log a)$。

### <a id="table6">Sol 6</a>

$c_i$ 的贡献为：
$$
c_i+\max\left\{-(K+c_j-c_1)\mid j<i,0\right\}+K
$$
也就只和前面的最小值有关，前面的最小值越小则贡献越大。那么肯定要让 $b$ 之前的最小值尽可能大，$a$ 之前的最小值尽可能小。

一个想法是 $b$ 降序排在前面，$a$ 升序排在后面。但是这是错的，因为其实完全可以把 $a$ 的一个最大值安排为 $c_1$，然后 $b$ 的贡献都大大降低了！$c_1$ 是一个非常特别的量！那么 $c_1$ 可能是什么呢？

其实这个答案是可以直接写成关于 $c_1$ 的函数的形式的，考察两个函数的性质，可以得到：

- $c_1=\max b$，这显然可以起到最小化 $b$ 的作用。
- $c_1=\min a$ 或者 $c=\max b$，这可以整体提高 $a$，如果 $a$ 特别多则很优。
- $c_1$ 为 $a$ 中 $b$ 的次大值的前驱或者后继。

### <a id="table7">Sol 7</a>

设 $f_{i,j}$ 表示上一步在 $i$，再上一步在 $j$ 的 $\mathrm{SG}$ 函数。

- $f_{i,j}$ 固定 $i$，$j$ 上升，$f_{i,j}$ 不降（因为 $\operatorname{mex}$ 的集合只会增大）。

扫描 $i$，考虑求出所有的 $f$ 的连续段。设 $g_k=f_{k,i}$，那么 $g$ 的变化次数也只会和连续段数同阶。所以不放在一个对 $g_k$ 的数据结构上去求 $f_{i,j}$ 的连续段。

考虑证明连续段的总数：因为凸性，所以这张 $\mathrm{DAG}$ 的深度只有 $O\left(n^{0.5}\right)$，通过归纳法证明出所有的 $\mathrm{SG}$ 也是 $O\left(n^{0.5}\right)$ 级别的。

### <a id="table8">Sol 8</a>

直接容斥所有 $\left[P_{i}<P_{i+1}\right]\neq\left[Q_{i}<Q_{i+1}\right]$ 的位置，那么 $P_{l}<P_{l+1}<\cdots<P_r,Q_l>Q_{l+1}>\cdots>Q_r$ 的 $[l,r]$ 这些连续段，是最一个基本的单位，所以直接算出段内的生成函数，并 $-\frac 1{1+f(x)}$ 就行了。

### <a id="table19">Sol 19</a>

摁 bitset，分成 $w$ 块，维护两两块构成的区间的或；散块可以暴力。这样空间需要 $w^2n$ 个 bit，存不下，解决方案是**用 ST 表来预处理**！**遇到需要快速查询任意区间的可并信息的时候，应该首选 ST 表**！

另一个做法：对于区间个数大于根号，暴力扫描即可。

对于区间个数小于根号，~~考虑每个出现次数大于根号的数，这个是容易的~~。不会做了。