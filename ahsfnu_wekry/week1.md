| 编号 |                     题目                      |                         链接                          |                        AC submission                         |    Solution     |
| ---- | :-------------------------------------------: | :---------------------------------------------------: | :----------------------------------------------------------: | :-------------: |
| $1$  |              Mr. Kitayuta's Gift              | [**CF506E**](https://www.luogu.com.cn/problem/CF506E) |                                                              |                 |
| $2$  |                    邮箱题                     |  [**P9150**](https://www.luogu.com.cn/problem/P9150)  |                                                              |                 |
| $3$  |           [THUPC 2023 初赛] 种苹果            |  [**P9136**](https://www.luogu.com.cn/problem/P9136)  |               [Link](https://loj.ac/s/1732044)               | [Link](#table5) |
| $4$  |         Drazil and His Happy Friends          | [**CF516E**](https://www.luogu.com.cn/problem/CF516E) | [Link](https://codeforces.com/contest/516/submission/198069817) | [Link](#table4) |
| $5$  |              [CQOI2018] 异或序列              |  [**P4462**](https://www.luogu.com.cn/problem/P4462)  |                              -                               | [Link](#table3) |
| $6$  |               [IOI2009] Regions               |  [**P5901**](https://www.luogu.com.cn/problem/P5901)  |                              -                               | [Link](#table1) |
| $7$  |              [IOI2009] Salesman               |  [**P5902**](https://www.luogu.com.cn/problem/P5902)  |                              -                               | [Link](#table2) |
| $8$  |               [IOI2009] Hiring                |  [**P9113**](https://www.luogu.com.cn/problem/P9113)  |      [Link](https://www.luogu.com.cn/record/105269914)       | [Link](#table6) |
| $9$  |               【清华集训2015】V               |       [**uoj164**](https://uoj.ac/problem/164)        |                              -                               | [Link](#table7) |
| $10$ |             【清华集训2015】Flea              |       [**uoj165**](https://uoj.ac/problem/165)        |           [Link](https://uoj.ac/submission/613579)           | [Link](#table8) |
| $11$ |             【清华集训2015】King              |       [**uoj166**](https://uoj.ac/problem/166)        |                                                              |                 |
| $12$ |              【JOISC2020】变色龙              |       [**uoj504**](https://uoj.ac/problem/504)        |                                                              |                 |
| $13$ |               【JOISC2020】扫除               |       [**uoj503**](https://uoj.ac/problem/503)        |               [Link](https://loj.ac/s/1735013)               | [Link](#table9) |
| $14$ |              【JOISC2020】汉堡肉              |       [**uoj502**](https://uoj.ac/problem/502)        |                                                              |                 |
| $15$ |            【JOISC2020】建筑装饰 4            |       [**uoj501**](https://uoj.ac/problem/501)        |                                                              |                 |
| $16$ |             【ULR #1】多线程计算              |       [**uoj574**](https://uoj.ac/problem/574)        |                                                              |                 |
| $17$ |              【ULR #1】光伏元件               |       [**uoj575**](https://uoj.ac/problem/575)        |                                                              |                 |
| $18$ |             【ULR #1】服务器调度              |       [**uoj576**](https://uoj.ac/problem/576)        |                                                              |                 |
| $19$ |              【ULR #1】打击复读               |       [**uoj577**](https://uoj.ac/problem/577)        |                                                              |                 |
| $20$ |               【ULR #1】校验码                |       [**uoj578**](https://uoj.ac/problem/578)        |                                                              |                 |
| $21$ |            【ULR #1】卫星基站建设             |       [**uoj579**](https://uoj.ac/problem/579)        |                                                              |                 |
| $22$ |             【UER #2】手机的生产              |       [**uoj113**](https://uoj.ac/problem/113)        |                                                              |                 |
| $23$ |             【UER #2】信息的交换              |       [**uoj114**](https://uoj.ac/problem/114)        |                                                              |                 |
| $24$ |             【UER #2】谣言的传播              |       [**uoj115**](https://uoj.ac/problem/115)        |                                                              |                 |
| $25$ |            「BalticOI 2018」交流电            |         [**loj2777**](https://loj.ac/p/2777)          |                                                              |                 |
| $26$ | 「BalticOI 2011 Day2」树的镜像 Tree Mirroring |         [**loj2637**](https://loj.ac/p/2637)          |                                                              |                 |
| $27$ |          「BalticOI 2016 Day1」公园           |         [**loj2781**](https://loj.ac/p/2781)          |                                                              |                 |
| $28$ |          「BalticOI 2015」文本编辑器          |         [**loj2706**](https://loj.ac/p/2706)          |                                                              |                 |
| $29$ |   「BalticOI 2022 Day1」Uplifting Excursion   |         [**loj3776**](https://loj.ac/p/3776)          |                                                              |                 |
| $30$ |             「BalticOI 2013」Vim              |         [**loj2687**](https://loj.ac/p/2687)          |                                                              |                 |
| $31$ |     「BalticOI 2022 Day2」Boarding Passes     |         [**loj3779**](https://loj.ac/p/3779)          |                                                              |                 |

### <a id="table1">Regions</a>

根据地区内的人数大于根号和小于根号分治即可。

### <a id="table2">[IOI2009] Salesman</a>

线段树维护即可。

### <a id="table3">[CQOI2018] 异或序列</a>

莫队即可。

### <a id="table4">Drazil and His Happy Friends</a>

首先，对于 $\bmod \gcd(n,m)$ 不同的男女生互不相关，所以可以先规约为 $n,m$ 互质的情况。

这个问题可以泛化为多源最短路问题，而多源最短路可以视作多个单源最短路取 $\min$，所以去考虑一开始只有一个男生 $x$ 是快乐的情况。

不难发现，$(x+m)\bmod n$ 男生在第 $m$ 秒变为快乐，$(x+2m)\bmod n$ 在第 $2m$ 秒变为快乐……这和女生已经无关了！所以应该去把男女生分开考虑然后取 $\min$（一个初始快乐的女生对男生的影响，可以把她转化为她对面的男生是初始快乐的，这同时也带来了上述结论的正确性）。

现在单独去考虑男生的 $\max$。而男生唯一有用的信息其实 $x'=(x\cdot m^{-1}\bmod n)$，把所有男生按照 $x'$ 排序，那么可能作为最值的只有每个关键男生的前面一个男生，断环成链做一下就好了。

### <a id="table5">[THUPC 2023 初赛] 种苹果</a>

用一个 LCT 维护，每个结点上维护 splay 子树内对应点的权值排序后的结果。若子树大小小于根号，则直接存下；否则不存，在询问的时候再递归下去。这个做法的可拓展性极强，可惜过于麻烦，在序列上劣于分块，在树上却很优秀。

还有一种办法是用 Linshey 分块，再加上根号重构。感觉码量很大。

### <a id="table6">[IOI2009] Hiring</a>

先不考虑总钱数最小的限制，因为第二个限制很弱，只要二分一下就能去掉。

对于每个选中的工人，他提供的限制是：$W\cdot \frac{Q_k}{\sum Q}\ge S_k$，也就是 $\sum Q\le X\cdot\frac {Q_k}{S_k}$，按照 $\frac{Q_k}{S_k}$ 排序，从大到小枚举其比值最小的一个，剩下的一定是贪心选 $Q$ 最小的，直到不能选了为止。用堆维护即可。

怎么把二分的 $\log$ 去掉？比值和比值排序的结果是固定的，直接枚举比值最小的工人，对应贪心选出尽可能多的工人，再算出这些工人对应的最小总工资即可。

### <a id="table7">【清华集训2015】V</a>

吉老师线段树板子题。

事实上，$a_i=\min(B,a_i+A)$ 类型的标记是符合结合律的，可以直接用懒标记来维护。

### <a id="table7">【清华集训2015】Flea</a>

一个想法是，求出 $f(1),f(m)$，并求出这两条抛物线的交点 $(x,y)$，再求出 $f(x)$，若 $f(x)=y$ 的话，直接返回 $y$；否则，向 $[1,x],[x,m]$ 两边递归下去，这样需要 $2n-1$ 次。

事实上，我们只需要每次取出最高的一个交点向下递归，如果不能向下递归就输出答案，这样就只需要 $n+1$ 次了；最后一次是细节讨论可以优化掉的。

### <a id="table7">【JOISC2020】扫除</a>

考虑由修改操作构成的序列，一次查询，相当于询问一个点，经过了操作 $[l,r]$ 后，会在哪一个点。

对修改序列分块，散块容易处理，整块的话，平面会被分成 $O(n^{0.5}\times n^{0.5})$ 份，需要对每一份分别预处理出它会移动到哪里。

预处理：考虑分治下去，$T(m)=2T(\frac {m}2)+m^2\rightarrow O(T(m))=O(m^2)$，所以递归下去加上双指针的复杂度是正确的。

询问时：离线，并预处理一个长度为 $O(n)$ 的数组记下双指针的结果，就可以去掉 $\log$，总复杂度 $O(m^{1.5})$。

事实上，这个做法没有完全利用上题目性质，一个性质是：

> 对于所有操作过至少一次的点，都满足 $x_i<x_j \Leftrightarrow y_i>y_j$。

所以，对于每个还没有操作过的点单独维护，剩下的点就是一个单调栈的形状，随便维护。

