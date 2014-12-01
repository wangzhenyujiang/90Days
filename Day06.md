###算法导论复习

######最小生成树算法

>最小生成树是用贪心算法来解决的，但是看后面的具体算法实施，我也是醉了..

在这个问题中的贪心算法可以这样来描述。这个方法遵守下述循环不变式的边集合A。

*在没次循环之前，A是某课最小生成树的一个子集。*

通用方法的伪代码描述：


		GENERIC-MST(G,w)
		
		1 A=null
		2 while A does not from a spanning tree
		3 	find an edge(u,v)that is safe for A
		4 	A U (u,v)
		5 return A
		
接下来我们讨论的算法中主要讨论的是第三行，**如何找到一个安全的边。**

理论基础：**假设G=(V,E)是一个在边E上定义了实数值权重函数w的联通无向图。设集合A为E的一个`子集`，且A包括图G的某课最小生成树中，假设（S,V-S）是图G中尊重集合A的任何一个`切割`，又设(u,v)是横跨切割(S,V-S)的一条`轻量级边`。那么边(u,v)对于集合A是安全的。**

######Kruskal算法

话不多说，先上伪代码:

		MST-KRUSKAL(G,w)
		
		1 A=null
		2 for each vertex v属于G.V
		3 		MAKE-SET(v)
		4 sort the edges of G.E into nondecreasing order by weight w
		5 for each edge(u,v) 属于 G.E
		6 		if FIND-SET(v) != FIND-SET(v)
		7			A=A U (u,v)
		8 			UNION(u,v)
		9 return A

解释一下这个算法的思想：
 
操作FIND-SET(u)来来返回包含元素u的集合代表元素。我们可以用FIND-SET(u)和FIND-SET(v)来判断结点u和结点v是否属于同一颗树。Kruskal算法使用UNION过程来对两棵树进行合并。

>今天上了一天的课，小小的复习一下，然后明天没有意外的话会是一篇`autolayout`的学习历程。

