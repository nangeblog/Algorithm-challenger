##### 最小生成树

- **Prim**

  简单的说就是, 先选取一个源点, 然后更新和它相连的点, 更新成功的加入队列, 从队列里弹出来的都是边的权重小的先出队列, 然后不断重复这个过程, 直到有n个节点(即n - 1条边)被标记. 整个过程类似水滴扩展. 最开始, 选中的点u, dis[u] = 0, 加入优先队列里. 优先队列是按(距离, 点的编号)保存的, 这样每次出队列时才是未标记的距离标记集合最近的点出队列. 

  注意代码里用的是**链式前向星存图**, 它的介绍可以看[这篇文章](https://blog.csdn.net/acdreamers/article/details/16902023), 而且模板代码里有比较详细的注释

  [模板例题P3366](https://www.luogu.org/problem/P3366), [模板代码](./P3366.cc)

  时间复杂度: O( E * log V ), E是边数, V是顶点数(下同)

- **Kruskal**

  将边按权值从小到大排序, 然后依次取边, 取出的边判断两端点是否是在一个联通块, 不在就连接, 边的权值贡献给最小生成树, 联通块的判断用并查集维护即可.

  [模板例题P3366](https://www.luogu.org/problem/P3366), [模板代码](./P3366-solution2.cc)

  时间复杂度: O(E * log E) 

---

##### 最短路

- **Floyd**

  可以处理负边权.

  本质是dp, 所以我们先用dp实现一遍. 

  定义dp\[k]\[i]\[j]为: 在只使用1, 2, 3, ..., k结点的情况下(可以都不用), 从结点i走到结点j最短的路径.

  状态转移为: 考虑k这个结点, 经过和不经过即可。

  [模板例题P3905](https://www.luogu.org/problem/P3905), [模板代码-dp实现](./P3905-solution2.cc)

  接着我们压缩掉第一维, 便是我们常见的Floyd写法了. (如果写滚动数组的话, 稍微有点麻烦, 我就不实现啦)

  [模板代码-常见实现](./P3905.cc)

  时间复杂度: O(N ^ 3)

- **Dijkstra**

  处理非负边权.

  定义dis[i]为从源点到结点i的最短路径. 每次从优先队列里弹出dis最小的点, 如果它未被访问(即标记)过, 就用它更新它相邻的点, 并将可以更新的点入队列. 重复上述过程, 直到队列为空, 或者终点已被标记, 则可结束算法. 

  它的实现和**Prim**非常类似, 只是在**Prim**中, dis[i]记录的是结点i距离已标记集合最短的距离, 而在**Dijkstra**中,dis[i]记录的是从源点到结点i的最短路径.

  [模板例题P4779](https://www.luogu.org/problem/P4779), [模板代码](./P4779.cc)

  时间复杂度: O(E * log V)

- **Bellman_Ford**

  可以处理负边权.

  也即大家口中说的SPFA算法. 它的实现其实和Dijkstra是类似的.

  同样定义dis[i]为从源点到结点i的最短路径, 每次选出所有的边进行松弛操作, 直到图中不能进行松弛操作, 算法结束. 如果图存在最短路, 则最多进行(顶点数n - 1)次松弛操作, 即当是一条链的情况. 如果第n次还可以进行松弛操作, 说明图中存在负环.

  松弛操作即: if(dis[u] + w < dis[v]) dis[v] = dis[u] + w; 即把dis[v]变小, 好像把它松弛了, 所以叫松弛操作.

  [模板例题P3385](https://www.luogu.org/problem/P3385), [模板代码](./P3385.cc)

  时间复杂度: O(E * V)

---

小结: 可以发现以上5种基本的图论算法, 十分类似, 有很多相同的地方. 希望大家多用类比的方式思考问题.

掌握了以上基本的算法, 接下来我们就可以去刷一些相关的题, 熟悉套路以及加深理解.

1. [P1144](https://www.luogu.org/problem/P1144), AC[源码](./P1144.cc)

   题意: 给出一个N顶点M条边的无向无权图, 顶点编号为1 ~ N . 问从顶点1开始, 到其他每个点的最短路有几条.

   `题解`: 多用一个数组ans[i]: 表示从源点到结点i最短路的条数. 在用**Dijkstra**更新最短路的时候, 也同时更新ans数组即可. 

   时间复杂度: O(E * log V)

2. [P1608](https://www.luogu.org/problem/P1608), AC[源码](./P1608.cc)

   题意: 略

   `题解`: 本题和例题1是类似的, 但是本题注意输入有重复的边, 需要去重. 然后解法和上个题一样.

   时间复杂度: O(E * log V)

3. [P1629](https://www.luogu.org/problem/P1629), AC[源码](./P1629.cc)

   题意: 有一个邮递员要送东西, 邮局在节点1.他总共要送N-1样东西, 其目的地分别是2~N.求送完这N-1样东西并且最终回到邮局最少需要多少时间.

   `题解`: 送的过程是一个出发点到多个目的地, 即1对多, 跑单源最短路**Dijkstra**即可. 难点是回来的时候, 是多对1. 回来的时候, 我们可以建立**反向图**, 这样就又变成1对多的情况, 再跑一遍单源最短路**Dijkstra**即可.

   反向图: 即u - - -  > v, 改为u < - - - v. 即结点u 到结点v, 改为结点v 到结点u.

   时间复杂度: O(E * log V)

4. [P1342](https://www.luogu.org/problem/P1342), AC[源码](./P1342.cc)

   题意: 略

   `题解`: 本题和例题3是类似的, 但是本题要开long long即可.

   时间复杂度: O(E * log V)

5. [P1119](https://www.luogu.org/problem/P1119), AC[源码](./P1119.cc)

   题意: 给定N个顶点和M条边, 每个顶点有个修建完成的时间. 顶点编号越小, 越先修建完成. 有q个询问, 对每一个询问(x, y, t)输出对应的答案，即在第t天，从村庄x到村庄y的最短路径长度为多少。

   `题解`: 最暴力的方法, 即对每个询问跑最短路算法. 这样显然超时.

   我们发现, 顶点修建完成的是按时间从小到大排序的, 我们知道**Floyd**算法的本质其实是dp, 本题就需要把最外面一层的k拎出来. 这样表示只用顶点编号1, 2, 3, . . . , k, 从顶点i 到顶点j的最短路.

   另外如果有这样一个情况1---3 ----2, 即顶点1 连接顶点3, 顶点3连接2. 如果此时k 等于1, 即顶点1 和顶点2 已经修建好了, 这时根据题目的定义, 顶点1到顶点3是不能到达的, 因为顶点3未修建完成, 不可到达. 但是此时dis\[1]\[3] = weight(1, 3) = w. 即此时顶点1到顶点3的最短路是w, 但是并不是题目定义的最短路. 这样也解释了最开始的时候dis\[i]\[j] = weight(1, 3), 尽管最开始顶点都是未修建的, 但是我们任然需要记录, 一旦修建完成就是dis\[i]\[j]了. 

   时间分钟: O(N ^ 3)

6. [POJ-3255](https://vjudge.net/problem/POJ-3255), AC[源码](./POJ-3255.cc)

   题意: 求次短路, 而不是最短路.

   `题解`: 和求最短路类似, 多用一个数组dis2维护次短路. 优先队列里, 最短路, 次短路都入队列里, 再弹出来后, 注意如何更新dis最短路和dis2最短路.

   给组数据: 

   2 1

   1 2 1

   答案是3, 到2的最短路是1, 次短路是3(即从2再走回1再回到2)

   ps: 求k短路呢?

   时间复杂度: O(E * log V)

   ---

   小结: 上面的6道例题, 大多都是类似的, 它们都是多用了一个数组维护相关的信息, 如最短路条数, 次短路. 希望大家多用类比的思维思考.
   
7. [P1576](https://www.luogu.org/problem/P1576), AC[源码](./P1576.cc)

   题意: 在n个人中，某些人的银行账号之间可以互相转账。这些人之间转账的手续费各不相同。给定这些人之间转账时需要从转账金额里扣除百分之几的手续费，请问A最少需要多少钱使得转账后B收到100元。

   `题解`: 将B当作源点, 初始值为100元, 用**Dijkstra**的方式去更新其他点, 就能求出A最少的钱.

   时间复杂度: O(E * log V)

8. [POJ-1797](https://vjudge.net/problem/POJ-1797#author=SWUN2018), AC[源码](./POJ-1797.cc)

   题意: N个点，M条边，每条边有权值。求一条1号点到N号点的路径，要求使得路径中的边权最小值最大。

   `题解`: 定义dis[i]: 表示从源点到结点i的路径中的边权最小值中的最大值, 即题目所要求的. 用**Dijkstra**类似的方式更新, 因为求最大, 所以优先队列是最大的先出队列, 即用std::less.

   时间复杂度: O(E * log V)

9. [POJ-1860](https://vjudge.net/problem/POJ-1860#author=riba2534), AC[源码](./POJ-1860.cc)

   题意: 略

   `题解`: 一种货币就是一个点 一个“兑换点”就是图上两种货币之间的一个兑换方式，是双边，但A到B的汇率和手续费可能与B到A的汇率和手续费不同。 唯一值得注意的是权值，当拥有货币A的数量为V时，A到A的权值为K，即没有兑换 而A到B的权值为(V-Cab)*Rab 本题是“求最大路径”，之所以被归类为“求最小路径”是因为本题题恰恰与**bellman-Ford**算法的松弛条件相反，求的是能无限松弛的最大正权路径，但是依然能够利用bellman-Ford的思想去解题。 因此初始化dis(S)=V 而源点到其他点的距离（权值）初始化为无穷小（0），当s到其他某点的距离能不断变大时，说明存在最大路径；如果可以一直变大，说明存在正环。判断是否存在环路，用Bellman-Ford和spfa都可以。

   时间复杂度: O(E * V)

10. [POJ-2253](https://vjudge.net/problem/POJ-2253#author=dusenlin), AC[源码](./POJ-2253.cc)

    题意: 两块石头之间的青蛙距离被定义为两块石头之间所有可能路径上的最小必要跳跃距离，某条路径的必要跳跃距离即这条路径中单次跳跃的最远跳跃距离。你的工作是计算Alice和Bob石头之间的青蛙距离。

    `题解`: 定义dis\[i]\[j]: 表示只使用1~k个点, 从i结点到j结点的最小必要跳跃距离(即答案所求), 第一维k被压缩掉了. 用**Floyd**类似的方式更新. 输出有两个换行.

    时间复杂度: O(N ^ 3)

11. [POJ-2387](https://vjudge.net/problem/POJ-2387#author=dusenlin), AC[源码](./POJ-2387.cc)

    题意: 略

    `题解`: 源点是n, 终点是1. 跑裸的**Dijkstra**即可

    时间复杂度: O(E * log V)

12. [POJ-3259](https://vjudge.net/problem/POJ-3259#author=chen_zhe_), AC[源码](./POJ-3259-solution.cc)

    题意: 略

    `题解`: 从S到E可以使时间倒流T秒. 在建图时将边的权值赋为负数. 用**Bellman-Ford**判断是否存在负权环.

    另外本题也可以用**Floyd**判断是否存在负权环.  不过时限很紧. [参考代码](./POJ-3259.cc)

    时间复杂度: Floyd: O(N ^ 3) ---- > Bellman-Ford: O(E * V)

13. [POJ-3268](https://vjudge.net/problem/POJ-3268#author=orz_LZT), AC[源码](./POJ-3268.cc)

    题意: 略.

    `题解`: 建**反向边图**, 跑**Dijkstra**即可.

    时间复杂度: O(E * log V)

14. [POJ-3723](https://vjudge.net/problem/POJ-3723#author=windsky1), AC[源码](./POJ-3723.cc)

    题意: 需要征募女兵N人，男兵M人。 每招募一个人需要花费10000美元。 如果已经招募的人中有一些关系亲密的人，那么可以少花一些钱。 给出若干男女之前的1 ~ 9999 之间的亲密度关系， 招募某个人的费用是 10000 - （已经招募了的人中和自己的亲密度的最大值）。 要求通过适当的招募顺序使得招募所有人所花费的费用最小。

    `题解`: 用**最小生成树**类似的方式求. 按边权从大到小排序, 依次选择边, 判断边的两个结点是否联通. 因为依次选的是最大的边, 这种方法我们叫做**最大生成树**.

    时间复杂度: O(N * log N)

15. [LightOJ-1074](https://vjudge.net/problem/LightOJ-1074#author=0), AC[源码](./LightOJ-1074.cc)

    题意: 给定n个点, 每个点有busyness, 给出ｍ条有向边, 边权的定义是**(busyness of destination - busyness of source)3**, 即相减的3次方. 问源点1到其他点的最小busyness.

    `题解`: 注意边权是3次方, 所以边权可能会有负数. 因为n <= 200, 可以先用**Floyd**跑一遍, 然后判断dis\[i][i], 如果dis\[i]\[i] 即从结点i出发构成了负环, 再用dfs从结点i出发, 标记它能到达的点. 它们都是busyness无限小, 因为可以在负环走很多圈, 再走到它.

    时间复杂度: O(N ^ 3)

16. [POJ-1502](https://vjudge.net/problem/POJ-1502#author=0), AC[源码](./POJ-1502.cc)

    题意: 给出n个点, 输入数据是, 先输入n, 再输入2~n每个点的下三角矩阵关系. 问源点1到所有结点的最短路径的最大值.

    `题解`: 裸题. n <= 100 也可以用**Floyd**, 参考代码写的是**Dijkstra**.

    时间复杂度: O(E * log V)

17. [POJ-1511](https://vjudge.net/problem/POJ-1511#author=0), AC[源码](./POJ-1511.cc)

    题意: 有向图,求点1到所有点来回最短路之和

    题解`: 建**反向边图**, 跑**Dijkstra**即可.

    时间复杂度: O(E * log V)

18. [POJ-2240](https://vjudge.net/problem/POJ-2240#author=0), AC[源码](./POJ-2240.cc)

    题意: 货币能相互兑换, 问能否套利.

    `题解`: 源点和本金都可以自己指定, 定义 dis[i]: 表示从源点到结点i的最大能获得的货币价值, 按照**Bellman_Ford**的方式更新. 

    时间复杂度: O(E * V)

19. [POJ-2502](https://vjudge.net/problem/POJ-2502#author=0), AC[源码](./POJ-2502.cc)

    题意: 给出数条地铁线, 问起点站到终点站的最短时间.

    `题解`: 本题建图比较麻烦. 一条地铁线上, 相邻的两站的速度是40km/h, 到其他站是10km/h. 建好图后跑**Floyd**. 有几个注意点, 算两站的时间时t = s/v, 如不是t = s / 40, 而是t = s / 40000(单位的换算), 最后求的是分钟所以要* 60 + 0.5.

20. [POJ-3159](https://vjudge.net/problem/POJ-3159), AC[源码](./POJ-3159.cc)

    题意: 给n个人派糖果，给出m组数据，每组数据包含A，B，c  三个数，意思是A的糖果数比B少的个数不多于c,即B的糖果数 - A的糖果数<= c 。最后求n 比 1 最多多多少糖果。

    `题解`: 网上大多数认为本题是差分约束题, 可能是A和B做差吧, 所以叫差分. 将c当作A到B的边权, 源点为1, 最后便是求源点1到结点n的最短距离.

    时间复杂度: O(E * log V)

21. [POJ-3660](https://vjudge.net/problem/POJ-3660), AC[源码](./POJ-3660.cc)

    题意: 本题也可以在[洛谷P2419](https://www.luogu.org/problem/P2419)提交. "于是他找来了奶牛们所有 M(1 <= M <= 4,500)轮比赛的结果，希望你能根据这些信息，推断出尽可能多的奶牛的编程能力排名。"

    注意输入的M轮比赛结果, 如A B, 则表示A和B比赛, 胜者是A, 即前者.

    `题解`: 推断出尽可能多的奶牛的编程能力排名, 如果一个参赛者和其他所有参赛者都确定了关系, 那么他的排名是确定的. 所以我们可以先跑**Floyd**, 即定义dis\[i]\[j]: 在只用到1~k结点, 结点i和结点j能否直接或者间接的联通, 即确定题目所说的谁的能力更强. 

    时间复杂度: O(N ^ 3)

22. ---

    小结: 从前面的一系列最短路题目中(绝大部分来自[kuangbin专题四-最短路练习](https://vjudge.net/article/752)), 我们可以发现, 这些题都只是将经典的最短路**Dijkstra**, **Bellman_Ford**, **Floyd**改写而已. 其中判断是否有负环我们可以用Bellman_Ford和Floyd(数据量较小), 常见的建反向图技巧, 改写定义, 或多定义一些变量, 维护相关东西.