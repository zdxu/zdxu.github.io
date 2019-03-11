---
layout: post
title: "数据结构之最大、最小堆"
date: 2018-12-02
tags: "Data&nbsp;Structure"
author: zdxu
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 最大堆

  - 定义：根结点（亦称堆顶）中的关键字是所有结点中的最大者，亦被称为大根堆。
  - 要点：
    - 满足完全二叉树
    - 以树中结点为根结点的子树依然是最大堆
    - 图例

  - 具体操作
    - 初始化
      步骤一：tree 以数组为基础结构，增加 sentinel 做头结点。

      步骤二：依次对每层结点（该结点为根结点的树高要大于 1）做操作3，保证该结点满足该结点的关键字大于左右子树的所有结点的关键字。

      步骤三：比较结点与左右子结点的最大结点比较，若小于则交换，并交换后的子结点继续做步骤三操作，保证以子结点为根结点的树依然是最大堆。

      时间复杂度 O(n)：

      $$
       = 2^{h-1} + 2^{h-2}2 + 2^{h-3}3 + ... + 2^{h-h}h
       = 2^h(\frac{1}{2} + \frac{2}{2^2} + \frac{3}{2^3} + ... + \frac{h}{2^h})
       \approx 2^h * 2
      $$

      这里的 h 为树高，因而时间复杂度O(n) = n

    - 新增
      添加到数组末尾，然后与其父结点比较，大于则替换小于则终止，对于大于的情况以替换后的父结点为起始与其父结点比较，重复上述步骤无父结点

      O(n) = log(n)
    - 弹出
      步骤一：删除数组 1 位置结点，并将末尾位置结点替至于 1 位置，删除末尾结点
      步骤二：以数组位置 1 结点起始，比较其左右子结点，若子结点大于结点则交换结点位置，并将起始改为被替换子结点所在位置，重复比较、交换直至结点大于左右子结点结束

      O(n) = log(n)

  - 关键代码
    - 初始化
    ```python
    tree_height =  int(math.floor(math.log(arr_length-1, 2)))
    i = tree_height
    while i >=  1:
        # 第 i 层的所有结点
        for idx in  range(pow(2, i -  1), pow(2, i)):
        # 操作以该位置结点为根结点的树，使其属于最大堆
        self.__compare_and_swap(idx, idx *  2, idx *  2  +  1)
        # 切换到上一层
        i = i -  1
    ```
    - 新增
    ```python
    # 数组末尾增加元素，并往上比对替换，保证所在树的都符合最大堆
    idx =  len(self.data) - 1
    while idx >  1:
        parent_idx = idx // 2
        parent_node = self.data[parent_idx]
        node =  self.data[idx]
        if node["priority"] <= parent_node["priority"]:
            # 该结点未对父级结点树的最大堆造成影响
            break
        else:
            # 子结点大于父结点则交换，并设置父结点位置为起始
            self.data[idx] = parent_node
            self.data[parent_idx] = node
            idx = parent_idx
    ```
    - 弹出
    ```python
    # 删除最大关键字结点并用末尾元素替换
    idx =  1
    node =  self.data[idx]
    self.data[idx] =  self.data[len(self.data) - 1]
    del  self.data[len(self.data) - 1]
    # 操作该树，根结点位置从上往下比对替换使其属于最大堆
    self.__compare_and_swap(idx, idx *  2, idx *  2  +  1)
    return node
    ```

最小堆与最大堆十分类似，这里不在详细描述。
具体最大堆、最小堆的实现代码:[data_struct/src/tree/heap at master · zdxu/data_struct · GitHub](https://github.com/zdxu/data_struct/tree/master/src/tree/heap)

- 备注
   - 1、数组、链表（线性无序）实现

   |操作|操作描述|时间复杂度
   |---|---|---
   |初始化|找到最大关键字元素，置于尾部|n
   |新增|跟最大元素比较后替换或存放相邻位置|1
   |弹出|弹出最大关键字元素后在剩余元素中找到最大元素与尾部替换|n

   - 2、数组、链表（线性无序）实现

   |操作|操作描述|时间复杂度
   |---|---|---
   |初始化|排序，归并算法|nlog(n)
   |新增|比较（二分查找）+ 移位 logn + (n+1)*n/2|n