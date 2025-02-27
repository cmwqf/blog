---
title: 浅谈Matrix-Tree定理
date: 2019-02-03 11:00:27
tags: [线性代数]
categories: [学习笔记]
---

# 行列式的定义

行列式指一个$$n$$​阶方阵（行数和列数相等）的值，记为$$\det(A)$$​或$$|A|$$​。

求行列式的公式为:
$$
\det(A)=\sum_P(-1)^{T(P)}A_{1,p1}...A_{n,pn}
$$
其中，$$P$$为$$1,..,N$$任意的一个排列，$$T(P)$$表示排列$$P$$的逆序对数，那个求和式的每一项就是从矩阵中选出$$n$$个数，使他们的行列都不重合。

<!--more-->

# 性质

1. 交换矩阵的两行（列），行列式变号

2. 如果矩阵有两行（列）完全相同，则行列式为$$0$$

3. 如果某一行（列）都乘以同一个数$$k$$，那么新行列式的值等于原行列式的值乘$$k$$

4. 如果矩阵有两行（列）成比例（比例系数为$$k$$），那么行列式为$$0$$

5. 如果把矩阵某一行（列）加上另一行（列）的$$k$$倍，行列式的值不变

# 求法

对于一个行列式，我们根据**性质5**，将它转化为一个上三角矩阵（左下半部分全是0），那么，**其行列式的值就等于对角线上数的乘积**（带入求行列式公式即证）。

这样，我们通过高斯消元的办法，把一个矩阵化成这个形式，再把对角线乘起来就可以了。

# Matrix-Tree定理无向图形式

这是一个计算一个无向图的生成树个数的定理，它的计算规则如下：

设$$D$$为度数矩阵，其中
$$
D[i][j]=0(i\ne j),D[i][i]=deg[i]
$$
设$$A$$为邻接矩阵。

定义**基尔霍夫矩阵**$$K=D-A$$

那么，最后的答案就是$$K$$在$$(i,i)$$处的余子式。

实际上，Matrix-Tree定理本质上是在求
$$
\sum_{T}\prod_{e\in T}val_e
$$
其中，$$T$$是生成树，$$e$$是边。换句话说，这个定理本质上是求这张图中所有生成树边权乘积之和，如果我们只是想求生成树的个数，只需要把每条边边权设成$$1$$即可。

否则，邻接矩阵$$(i,j)$$存的是所有$$(i,j)$$边的边权和，度数矩阵$$(i,i)$$存的是所有与$$i$$相邻的边的边权和。

# Matrix-Tree定理有向图形式

有向图的生成树分为两种：内向树和外向树。

内向树指树上所有的边都是孩子指向父亲，外向树则是所有边都是父亲指向儿子。

那么，设$$D_{out}$$表示所有点的出度矩阵，$$D_{in}$$表示所有点的入度矩阵，换句话说
$$
D_{out}[i][i]=\sum_{(u,v)\in E,u=i}val[u][v]\\
D_{in}[i][i]=\sum_{(u,v)\in E,v=i}val[u][v]
$$
此时，内向树的基尔霍夫矩阵为$$K_{out}=D_{out}-A$$，外向树为$$K_{in}=D_{in}-A$$。如果我们想要求出以$$rt$$为根的内向树（外向树）个数，那么只需要求$$K_{out}$$（$$K_{in}$$）在$$(rt,rt)$$​处的余子式即可。

# 证明

那么，如何证明Matrix-Tree定理呢？

内向树，外向树都差不多，而无向图也只是把无向边看成两条有向边，计算内向树（外向树）得到的。因此，我们只讨论内向树的情况。

我们先不讨论自环的情况，实际上在讲完证明后我们可以发现即使出现了自环也没有关系。

首先，考虑对角线上每个点出度的乘积，它的意义在于给每个点找一条到父亲的边作为树边。但是显然这样构造出来的可能会出现若干环，因此我们的目标是通过容斥把存在环的方案去掉。

考虑通过行列式算出来的是什么，它枚举了一个排列，而我们知道，每个排列都对应着一个环分解，因此，枚举排列，本质上是枚举了一个环的集合，相当于容斥中钦定一个环的集合，然后对于所有$$p_i=i$$的点，即乘上的是出度，即我们把他们随便选父亲的出边。那么，考虑每个排列的贡献系数是什么？行列式前面这个排列的贡献系数是$$-1$$的逆序对数次方，实际上逆序对数只是一种表达方式，它表示的是这个排列的奇偶性。

可以证明，对于任意一个排列，不管怎么对换（即交换两个元素）使其最终成为单位排列，交换次数的奇偶性是相同的，因此排列也被分为奇排列和偶排列，而逆序对数，即每次可以交换相邻两个元素而成为单位排列的操作次数。

而排列的奇偶性我们还可以通过另外的一种计算方式，即对于环分解的每个环考虑，那么一个大小为$$k$$​​​的环需要进行$$k-1$$​​​​次对换拆掉，假设排列中有$$k$$​​​个**大小大于$$1$$​​​的环**，这些环的大小和为$$s$$​​，那么行列式中一个排列前面的系数应该是$$(-1)^{s-k}$$​，而注意到矩阵中每条边前面的系数都是$$-1$$​​，而这些环中显然有$$s$$​条边，因此系数前面还会乘上$$(-1)^s$$​​，因此对于一种方案，系数为$$(-1)^{2s-k}=(-1)^k$$​，即$$-1$$​​的**钦定的环集合大小**次方。

那么，对于一种有$$k$$个环的方案，它的系数是$$\sum_{i=0}^k(-1)^i\binom{k}{i}$$，这个值在$$k=0$$时为$$1$$，而当$$k>0$$时为$$0$$。因此，只有当方案中没有环的时候才会被计算到贡献。

# 注意

注意内向树，外向树的度数矩阵到底是出度还是入度，其实如果会了证明那么就很好理解，因为第一步是给每个点选一个父亲边，而内向树只能在出边中选，外向树只能在入边中选。