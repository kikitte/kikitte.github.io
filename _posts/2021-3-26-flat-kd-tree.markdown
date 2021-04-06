---
layout: post
title:  "kd树概念以及地理上的用处"
date:   2021-03-25 10:18:50 +0800
categories: geographic
---

### K-Dimensional Trees

> 参考：[Understanding K-Dimensional Trees](https://towardsdatascience.com/understanding-k-dimensional-trees-1cdbf6075f22)

1. 前言

   K近邻算法（K-Nearest Neighbords）在地理空间的用途之一是寻找距离某点最近的其他样本数据：比如距离手机最近的加油站等等等。与其他很多算法一样，实现K近邻算法也可以通过暴力计算（即将其它所有点按照与目标点的距离按从小到大进行排序）得到答案，但是在现实应用中显然不实际。另一种选择是使用高效的数据结构：如k-dimensional tree 或 ball-tree作为 空间搜索算法.

2.  Tree：这种数据结构的基本概念

   通常，一个Tree被定义成一个或多个节点(node)有限的集合：

   - 有一个被故意设计的节点叫做root
   - 剩余的节点被分成n ≥ 0个不相交的集合：T₁, ….., Tₙ

3. 常见类型的Tree

   - 二叉树（Binary Tree）

     In [computer science](https://en.wikipedia.org/wiki/Computer_science), a **binary tree** is a [tree data structure](https://en.wikipedia.org/wiki/Tree_(data_structure)) in which each node has at most two [children](https://en.wikipedia.org/wiki/Child_node), which are referred to as the *left child* and the *right child*.

   - 二叉搜索树（Binary Search Tree）

     In [computer science](https://en.wikipedia.org/wiki/Computer_science), a **binary search tree** (**BST**), also called an **ordered** or **sorted binary tree**, is a [rooted](https://en.wikipedia.org/wiki/Rooted_tree) [binary tree](https://en.wikipedia.org/wiki/Binary_tree) whose internal nodes each store a key greater than all the keys in the node's left subtree and less than those in its right subtree.

     （所有节点的内部存储一个key，这个key大于该节点左子树的所有key但是小于该节点所有右子树的所有key）

4. K-Dimensional Tree思想以及范例

   KD树是BSD树的一个变种，其与BSD树的差别在于这个树分支的每个层级都有与该层级相关的维度(The difference is that each level of the tree branches based on a particular dimension associated with that level.)，即对于有n层级的KD树，对于∀ *i* = {1, … , *n*}，有一唯一的维度d = i mod k 被用于比较，d被称为分割维度(splitting dimension)。当遍历该树时，使用对应层级的分割维度对节点进行比较。如果比较结果小则向左分支查找否则向右分支查找。除了在维度上其他都保留了BST树的数据结构。

5. Javascript空间索引算法实现：[KDBush](https://github.com/mourner/kdbush)

   该实现基于2d kd树算法。

   