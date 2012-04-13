排序
------

计数排序
^^^^^^^^^

计数排序(Counting sort)是一种稳定的排序算法。计数排序使用一个额外的数组C，其中第i个元素是待排序数组A中，值等于i的元素的个数。然后根据数组C来将A中的元素排到正确的位置。

- 数据结构：数组

- 最差时间复杂度：O(n + k)

- 最优时间复杂度：O(n + k)

- 平均时间复杂度：O(n + k)

- 最差空间复杂度：O(n + k)

由于用来计数的数组C的长度取决于待排序数组中数据的范围(等于待排序数组的最大值与最小值的差加1)，这使得计数排序对于数据范围很大的数组，需要大量的时间和内存。

**算法步骤:**

#) 找出待排序的数组中最大和最小的元素

#) 统计数组中每个值为i的元素出现的次数，存入数组C的第i项

#) 对所有的计数累加(从C中的第一个元素开始，每一项和前一项相加)

桶排序
^^^^^^^

桶排序(Bucket sort)或所谓箱算法，工作原理是将数组元素分到有限数量的桶子里。每个桶子再个别排序(有可能再使用别的排序算法或以递归方式继续使用桶排序进行排序)。桶排序是鸽巢排序的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，桶排序使用线性时间(O(n))。桶排序并不是比较排序，不受O(n log n)下限的影响。

- 数据结构：数组

- 最差时间复杂度：O(n^2)

- 平均时间复杂度：O(n + k)

- 最差时间复杂度：O(n)

- 最差空间复杂度：O(n*k)

桶排序的步骤如下:

#) 设置一定量的数组当作空桶子

#) 遍历序列，并且把元素一个一个放到对应的桶子去

#) 对每个不是空的桶子进行排序

#) 从不是空的桶子里把元素再放回原来的序列中。

#) 反向填充目标数组：将每个元素i放到新数组的第C(i)项，每放一个元素就将C(i)减去1

基数排序
^^^^^^^^

基数排序(Radix sort)是一种非比较型整数排序算法，其原理是将整数按位切割成不同的数字，然后按每个位数分别比较。由于也可以用于字符串(比如名字或日期)和特定格式的浮点数，所以基数排序也不是只能用于整数。

- 数据结构：数组

- 最差时间复杂度： O(kN)

- 最差空间复杂度： O(kN)

**基数排序的基本步骤**:

#) 将所有待比较数值(正整数)统一为同样的数位长度，数位较短的数前面补零
#) 然后，从最低位开始，依次进行排序
#) 这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列。

**效率**

基数排序的时间复杂度是O(kN)，其中N是待排序元素个数，k是数字位数。注意这不是说这个时间复杂度一定优于O(n*log(n))，因为k的大小一般会受到n的影响。

搜索
------

B-tree
^^^^^^^^

In computer science, a B-tree is a tree data structure that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time. The B-tree is a generalization of a binary search tree in that a node can have more than two children. Unlike self-balancing binary search trees, the B-tree is optimized for systems that read and write large blocks of data. It is commonly used in databases and filesystems.

一棵m阶B树(balanced tree of order m)是一棵平衡的m路搜索树，它或者是空树，或者是满足下列性质的树:

#) 根节点至少有两个子女
#) 除根结点以外的所有结点(不包括失败结点)至少有[m/2]个子女
#) 所有的失败结点都位于同一层

.. image:: https://lh5.googleusercontent.com/-84zYMLNoclg/T4hczBuI--I/AAAAAAAAA_A/vL_qeSkfRe4/s573/b-tree.png

B+ tree
^^^^^^^^^

a B+ tree is a type of tree which represents sorted data in a way that allows for efficient insertion, retrieval and removal of records, each of which is identified by a key. It is a dynamic, multilevel index, with maximum and minimum bounds on the number of keys in each index segment (usually called a "block" or "node"). In a B+ tree, in contrast to a B-tree, all records are stored at the leaf level of the tree; only keys are stored in interior nodes.

红黑树
^^^^^^^

红黑树是一种自平衡二叉查找树，典型用途是实现关联数组。

红黑树是每个节点都带有颜色属性的二叉查找树，颜色为红色或黑色。在二叉查找树的一般要求以外，对于任何有效的红黑树增加了如下的额外要求：

#) 节点是红色或黑色
#) 根是黑色
#) 所有叶子都是黑色(叶子是NIL节点)
#) 每个红色节点的两个子节点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)
#) 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

.. image:: https://lh5.googleusercontent.com/-Et4W3BWMFFI/T4hiLMM4EKI/AAAAAAAAA_Y/zj4O0mbo9kA/s800/R-B-Tree.png

这些约束强制了红黑树的关键性质：从根到叶子的最长的可能路径不多于最短的可能路径的两倍长。结果是这棵树大致上是平衡的。因为操作比如插入、删除和查找某个值的最坏情况时间都要求与树的高度成比例，这个在高度上的理论上限允许红黑树在最坏情况下都是高效的，而不同于普通的二叉查找树。

AVL树
^^^^^^

在计算机科学中，AVL树是最先发明的自平衡二叉查找树。在AVL树中任何节点的两个子树的高度最大差别为一，所以它也被称为高度平衡树。查找、插入和删除在平均和最坏情况下都是O（log n）。增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。

节点的平衡因子是它的右子树的高度减去它的左子树的高度。带有平衡因子1、0或 -1的节点被认为是平衡的。带有平衡因子 -2或2的节点被认为是不平衡的，并需要重新平衡这个树。平衡因子可以直接存储在每个节点中，或从可能存储在节点中的子树高度计算出来。

AVL树的基本操作一般涉及运作同在不平衡的二叉查找树所运作的同样的算法。但是要进行预先或随后做一次或多次所谓的"AVL旋转"。

.. image:: https://lh3.googleusercontent.com/-1IhS4a0vUGw/T4hkkWCNA1I/AAAAAAAAA_s/fTWxRs0AH5Y/s720/Tree_Rebalancing.png

二叉查找树
^^^^^^^^^

二叉查找树(Binary Search Tree)，或者是一棵空树，或者是具有下列性质的二叉树:

#) 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
#) 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
#) 它的左、右子树也分别为二叉排序树。