# ACM风格

```java
// 一个一个读
BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
// 一个一个读数字
StreamTokenizer in = new StreamTokenizer(bufferedReader);
PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

while (in.nextToken() != StreamTokenizer.TT_EOF) {
    int n = (int) in.nval;
    in.nextToken();
    int m = (int) in.nval;

    out.println(mySolution(m, n));
}
out.flush();
out.close();
// 一行一行读
String line;
String[] parts;

BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

while ((line = in.readLine()) != null) {
    parts = line.split(" ");
    ArrayList<Integer> list = new ArrayList<>();
    for (String part: parts) {
        list.add(Integer.parseInt(part));
    }
    out.println(mySolution(list));
}
out.flush();
in.close();
out.close();
```



# 多线程

| 多线程问题类型 |                                                              |
| -------------- | ------------------------------------------------------------ |
| 执行屏障       | [1114.顺序打印](https://leetcode.cn/problems/print-in-order/) [1115.交替打印](https://leetcode.cn/problems/print-foobar-alternately/) |
| 死锁问题       | [1226.哲学家进餐](https://leetcode.cn/problems/the-dining-philosophers/) |



# 一些必备知识

| 必备知识                          |                                                              |
| --------------------------------- | ------------------------------------------------------------ |
| 最大公约数Greatest Common Divisor | (a > b)`gcd(a, b){return b == 0 ? a : gcd(b, a % b);}`       |
| 最小公倍数Least Common Multiple   | (a > b)`lcm(a, b){return a / gcd(a, b) * b;}`                |
| 同余原理                          |                                                              |
| 乘法快速幂                        | log(n)求x^n^ [50.Pow(x, n)](https://leetcode.cn/problems/powx-n/) |
| 矩阵快速幂                        | 求一个矩阵A的n次方                                           |
| 矩阵快速幂1维k阶                  |                                                              |
| 矩阵快速幂k维1阶                  | [509.斐波那契数](https://leetcode.cn/problems/fibonacci-number/) [70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/) [1137.第N个斐波那契数](https://leetcode.cn/problems/n-th-tribonacci-number/) [790.多米诺和托米诺平铺](https://leetcode.cn/problems/domino-and-tromino-tiling/) [1220.统计元音字母序列的数目](https://leetcode.cn/problems/count-vowels-permutation/) [552.学生出勤记录II](https://leetcode.cn/problems/student-attendance-record-ii/) |



# 排序

| 排序相关问题 | [912.排序数组](https://leetcode.cn/problems/sort-an-array/)  |
| ------------ | ------------------------------------------------------------ |
| 归并排序     | [88.合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/) [327.区间和的个数](https://leetcode.cn/problems/count-of-range-sum/) [315.右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/) [493.翻转对](https://leetcode.cn/problems/reverse-pairs/) [LCR170.逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/) |
| 快速排序     | 链表的快速排序 [215.数组中的第K大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/) |
| 堆排序       |                                                              |
| 基数排序     |                                                              |
| 链表排序     | [148.排序链表](https://leetcode.cn/problems/sort-list/)      |



# 位运算

异或的一个性质: x^y=s --> s^y=x

异或的常用操作: `n&(n-1)`去除n的最后一个1; `n&(~n+1) <==> n&(-n)`获取n最后一个1的状态

| 位运算 |                                                              |
| ------ | ------------------------------------------------------------ |
| 位运算 | [268.缺失的数字](https://leetcode.cn/problems/missing-number/) [136.出现一次的数](https://leetcode.cn/problems/single-number/) [137.出现一次的数II](https://leetcode.cn/problems/single-number-ii/) [260.出现一次的数III](https://leetcode.cn/problems/single-number-iii/) [191.位1的个数](https://leetcode.cn/problems/number-of-1-bits/) [231.2的幂](https://leetcode.cn/problems/power-of-two/) [201.数字范围按位与](https://leetcode.cn/problems/bitwise-and-of-numbers-range/) |
|        | [190.颠倒二进制位](https://leetcode.cn/problems/reverse-bits/) [461.汉明距离](https://leetcode.cn/problems/hamming-distance/) [29.位运算实现加减乘除](https://leetcode.cn/problems/divide-two-integers/) |
| 位图   | [2166.设计位集](https://leetcode.cn/problems/design-bitset/) |



# 基本数据结构

| 基本数据结构的应用 |                                                              |
| ------------------ | ------------------------------------------------------------ |
| 栈的应用           | [150.逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/) [388.文件的最长绝对路径](https://leetcode.cn/problems/longest-absolute-file-path/) |
| 队列的应用         | [362.敲击计数器](https://labuladong.online/algo/problem-set/queue/#_362-敲击计数器) [933.最近的请求次数](https://leetcode.cn/problems/number-of-recent-calls/) [346.数据流中的移动平均值](https://labuladong.online/algo/problem-set/queue/#_346-数据流中的移动平均值) |
| 哈希表的应用       | 复制(克隆)问题 [🔒1485.克隆含随机指针的二叉树](https://labuladong.online/algo/problem-set/binary-tree-divide-i/#_1485-克隆含随机指针的二叉树) [🔒1490.克隆N叉树](https://labuladong.online/algo/problem-set/binary-tree-divide-i/#_1490-克隆-n-叉树) 异位词问题 [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/) [49.字母异位词分组](https://leetcode.cn/problems/group-anagrams/) 其他应用 [1.两数之和](https://leetcode.cn/problems/two-sum/) [387.字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/) |
| 堆的应用           | 合并k个有序序列 [23.合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/) |
|                    | 寻找第k大元素 [373.查找和最小的K对数字](https://leetcode.cn/problems/find-k-pairs-with-smallest-sums/) [378.有序矩阵中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/) |
|                    | [703.数据流的第K大元素](https://leetcode.cn/problems/kth-largest-element-in-a-stream/) [1845.座位预约管理系统](https://leetcode.cn/problems/seat-reservation-manager/) [870.优势洗牌田忌赛马](https://leetcode.cn/problems/advantage-shuffle/) [1834.单线程CPU](https://leetcode.cn/problems/single-threaded-cpu/) |
|                    | [347.前K个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/) [451.根据字符出现频率排序](https://leetcode.cn/problems/sort-characters-by-frequency/) [692.前K个高频单词](https://leetcode.cn/problems/top-k-frequent-words/) |
| 计算器             | [计算器(一)](https://www.nowcoder.com/practice/9b1fca7407954ba5a6f217e7c3537fed) |



# 数据结构设计

| 设计数据结构       |                                                              |
| ------------------ | ------------------------------------------------------------ |
| 频率相关的数据结构 | [895.最大频率栈](https://leetcode.cn/problems/maximum-frequency-stack/) [432.全 O(1) 的数据结构](https://leetcode.cn/problems/all-oone-data-structure/) |
| LRU与LFU           | [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/) [460. LFU 缓存](https://leetcode.cn/problems/lfu-cache/) |
| stack              | [155.最小栈](https://leetcode.cn/problems/min-stack/)        |
| queue              | [622.设计循环队列](https://leetcode.cn/problems/design-circular-queue/) [641.设计循环双端队列](https://leetcode.cn/problems/design-circular-deque/) [1670.设计前中后队列](https://leetcode.cn/problems/design-front-middle-back-queue/) |
|                    | [353.贪吃蛇](https://labuladong.online/algo/problem-set/ds-design)  [379. 电话目录管理系统](https://labuladong.online/algo/problem-set/ds-design) |
| heap               | [355.设计推特](https://leetcode.cn/problems/design-twitter/) |
| 设计迭代器         | [284. 窥视迭代器](https://leetcode.cn/problems/peeking-iterator/) [251. 展开二维向量](https://labuladong.online/algo/problem-set/ds-design/#_251-展开二维向量) |
| O(1)删除数组元素   | [380.O(1)插入, 删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/) [381.O(1)插入, 删除和获取随机元素II](https://leetcode.cn/problems/insert-delete-getrandom-o1-duplicates-allowed/) [710.避开黑名单的随机数](https://leetcode.cn/problems/random-pick-with-blacklist/) |
| 中位数             | [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/) |



# 二叉树

## 问题类型

| 二叉树问题类型 | 题目                                                         |
| -------------- | ------------------------------------------------------------ |
| 序列化         | [297.二叉树序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/) [449.BST序列化和反序列化](https://leetcode.cn/problems/serialize-and-deserialize-bst/) [652.寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/) |
| 迭代遍历       | [迭代遍历二叉树框架](##迭代遍历二叉树)                       |
| 最近公共祖先   | [235.BST公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/) [236.公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) [1644.公共祖先II](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree-ii/) [1650.公共祖先III](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree-iii/) [1676.公共祖先IV](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree-iv/) [865.具有所有最深节点的最小子树](https://leetcode.cn/problems/smallest-subtree-with-all-the-deepest-nodes/) |

```java
// 二叉树遍历时每个结点都要访问3次, 入栈的时候一次, 遍历完一个子树去另一个子树的时候一次(没有右子树则没有这次), 出栈的时候一次
public List<Integer> postorderTraversal(TreeNode root) {

    // 指向上一次遍历完的子树根节点,
    TreeNode visited = new TreeNode(-1); // 一开始指向一个不相关的结点
    pushLeftBranch(root);

    while (!stack.isEmpty()) {
        // p游离在树上, 不要把他看作游离在栈上
        TreeNode p = stack.peek();

        // 每个栈中的结点的左子树天生就是考虑过的, 因此直接判断右子树
        if (p.right != null && p.right != visited) { // 有右子树且没有被访问过
            // 去遍历 p 的右子树
            pushLeftBranch(p.right);
        } else { // 没有右子树, 或者右子树被访问过了, 出栈
            visited = stack.pop();
            ans.add(p.val);
        }
    }
    return ans;
}

private void pushLeftBranch(TreeNode p) {
    while (p != null) {
        stack.push(p);
        p = p.left;
    }
}
```



## BST二叉搜索树

| BST问题类型 | 题目                                                         |
| ----------- | ------------------------------------------------------------ |
| BST的遍历   | [99.恢复BST](https://leetcode.cn/problems/recover-binary-search-tree/) [270.最接近的二叉搜索树值](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii/#_270-最接近的二叉搜索树值) |
| BST操作     | [538.BST转换累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/) [230.BST第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/) [701插入](https://leetcode.cn/problems/insert-into-a-binary-search-tree/), [450.删除](https://leetcode.cn/problems/delete-node-in-a-bst/), [98.验证](https://leetcode.cn/problems/validate-binary-search-tree/) [173.BST迭代器](https://leetcode.cn/problems/binary-search-tree-iterator/) [1305.BST合并](https://labuladong.online/algo/problem-set/bst2/#_1305-两棵二叉搜索树中的所有元素) |
| BST的构造   | [96.n个结点构成多少不同BST](https://leetcode.cn/problems/unique-binary-search-trees/) [95.n个结点构成多少不同BST II](https://leetcode.cn/problems/unique-binary-search-trees-ii/) |
| BST的重构   | [🔒426.BST转化为排序的双向链表](https://labuladong.online/algo/problem-set/binary-tree-divide-i) [669.修剪BST](https://leetcode.cn/problems/trim-a-binary-search-tree/) [776.拆分二叉搜索树](https://labuladong.online/algo/problem-set/bst1) [1008.前序遍历构造BST](https://leetcode.cn/problems/construct-binary-search-tree-from-preorder-traversal/) [108.有序数组转换为BST](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/) [109.有序链表转换二叉搜索树](https://leetcode.cn/problems/convert-sorted-list-to-binary-search-tree/) |
|             | [1373. BST子树的最大键值和](https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/) |
|             | [🔒285.BST中的中序后继](https://labuladong.online/algo/problem-set/bst1) [🔒510. BST中的中序后继 II](https://labuladong.online/algo/problem-set/bst1) |

[671. 二叉树中第二小的节点](https://leetcode.cn/problems/second-minimum-node-in-a-binary-tree/)

[🔒1214. 查找两棵BST之和](https://labuladong.online/algo/problem-set/bst1)

[938. BST的范围和](https://leetcode.cn/problems/range-sum-of-bst/)



## 题型

| 二叉树题型                 | 题目                                                         |
| -------------------------- | ------------------------------------------------------------ |
| 二叉树的特性               | [958.验证完全二叉树](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/) [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/) [331. 验证二叉树的前序序列化](https://leetcode.cn/problems/verify-preorder-serialization-of-a-binary-tree/) |
| 路径问题(根节点到叶子结点) | [257.路径](https://leetcode.cn/problems/binary-tree-paths/) [129.路径的和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/) [1022.路径的和](https://leetcode.cn/problems/sum-of-root-to-leaf-binary-numbers/) [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/) [🔒298.最长连续路径](https://labuladong.online/algo/problem-set/binary-tree-traverse-i/#_298-二叉树最长连续序列) [988.最小字典序的路径](https://leetcode.cn/problems/smallest-string-starting-from-leaf/) [1457.伪回文路径数量](https://leetcode.cn/problems/pseudo-palindromic-paths-in-a-binary-tree/) [437.和为k的子路径个数](https://leetcode.cn/problems/path-sum-iii/) [1448.好节点的个数](https://leetcode.cn/problems/count-good-nodes-in-binary-tree/) |
| 跨层操作(在根节点用孩      | [404.左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/) [623.二叉树增加一行](https://leetcode.cn/problems/add-one-row-to-tree/) [971.翻转二叉树以匹配先序序列](https://leetcode.cn/problems/flip-binary-tree-to-match-preorder-traversal/) [🔒1469.寻找所有的独生节点](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii/#_1469-寻找所有的独生节点) |
| 亲戚问题                   | [993.判断是否为堂兄弟](https://leetcode.cn/problems/cousins-in-binary-tree/) [1315. 爷爷为偶数的节点总和](https://leetcode.cn/problems/sum-of-nodes-with-even-valued-grandparent/) [🔒1602.找到最近的右侧节点(找下一个兄弟)](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii) |
| 相同子树                   | [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/) [1367.二叉树中的链表](https://leetcode.cn/problems/linked-list-in-binary-tree/) [951. 翻转等价二叉树](https://leetcode.cn/problems/flip-equivalent-binary-trees/) [100.相同的树](https://leetcode.cn/problems/same-tree/) [572.另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/) [LCR 143. 子结构判断](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/) |
| 分解问题                   | [104.最大深度(高度)](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) [543.直径](https://leetcode.cn/problems/diameter-of-binary-tree/) [897. BST拉平为链表](https://leetcode.cn/problems/increasing-order-search-tree/) [114.(拉平)展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/) [LCR155.BST拉平为排序双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/) [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/) [112.路径总和](https://leetcode.cn/problems/path-sum/) [113.路径总和II](https://leetcode.cn/problems/path-sum-ii/) |
| 嵌套列表                   | [341. 扁平化嵌套列表迭代器](https://leetcode.cn/problems/flatten-nested-list-iterator/) [🔒339. 嵌套列表加权和](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view) [🔒364. 嵌套列表加权和 II](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view) |
| 后序                       | [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/) [🔒250. 统计同值子树](https://labuladong.online/algo/problem-set/binary-tree-post-order-i/#_250-统计同值子树) [🔒366. 寻找二叉树的叶子节点](https://labuladong.online/algo/problem-set/binary-tree-post-order-i) [508. 出现次数最多的子树元素和](https://leetcode.cn/problems/most-frequent-subtree-sum/) [563. 二叉树的坡度](https://leetcode.cn/problems/binary-tree-tilt/) |
| 后序                       | [🔒663. 均匀树划分](https://labuladong.online/algo/problem-set/binary-tree-post-order-ii/#_663-均匀树划分) [1339. 分裂二叉树的最大乘积](https://leetcode.cn/problems/maximum-product-of-splitted-binary-tree/) [979. 在二叉树中分配硬币](https://leetcode.cn/problems/distribute-coins-in-binary-tree/) [2049. 统计最高分的节点数目](https://leetcode.cn/problems/count-nodes-with-the-highest-score/) |
| 后序返回值多个值           | [865. 具有所有最深节点的最小子树](https://leetcode.cn/problems/smallest-subtree-with-all-the-deepest-nodes/) [🔒333. 最大 BST 子树](https://labuladong.online/algo/problem-set/binary-tree-post-order-i) [🔒549. 二叉树中最长的连续序列](https://labuladong.online/algo/problem-set/binary-tree-post-order-i) [1026. 节点与其祖先之间的最大差值](https://leetcode.cn/problems/maximum-difference-between-node-and-ancestor/)  [🔒1120. 子树的最大平均值](https://labuladong.online/algo/problem-set/binary-tree-post-order-ii/#_1120-子树的最大平均值) [1372. 二叉树中的最长交错路径](https://leetcode.cn/problems/longest-zigzag-path-in-a-binary-tree/) |
| 后序参数多个值             | [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/) [1080. 根到叶路径上的不足节点](https://leetcode.cn/problems/insufficient-nodes-in-root-to-leaf-paths/) [687. 最长同值路径](https://leetcode.cn/problems/longest-univalue-path/) |
| 二叉树的构造               | [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/) [998. 最大二叉树 II](https://leetcode.cn/problems/maximum-binary-tree-ii/) [105.前序与中序构造二叉树 ](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) [106. 从中序与后序遍历序列构造二叉](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) [889.前序和后序构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) |
| 二叉树的重构               | [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/) [🔒1660. 纠正二叉树](https://labuladong.online/algo/problem-set/binary-tree-divide-i/#_1660-纠正二叉树) [1325. 删除给定值的叶子节点](https://leetcode.cn/problems/delete-leaves-with-a-given-value/) [814. 二叉树剪枝](https://leetcode.cn/problems/binary-tree-pruning/) [1110.删点成林](https://leetcode.cn/problems/delete-nodes-and-return-forest/) |
| 二叉树的排列               | [894.所有可能的真二叉树](https://leetcode.cn/problems/all-possible-full-binary-trees/) |
| 层序遍历                   | [116.结点指向右兄弟](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/) [117.结点指向右兄弟 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/) [1302.层数最深节点的和](https://leetcode.cn/problems/deepest-leaves-sum/) [103.锯齿层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/) [🔒431. 将 N 叉树编码为二叉树](https://labuladong.online/algo/problem-set/binary-tree-level-ii) |
| 树抽象为图的BFS            | [863. 二叉树中所有距离为 K 的结点](https://leetcode.cn/problems/all-nodes-distance-k-in-binary-tree/) [🔒742. 二叉树最近的叶节点](https://labuladong.online/algo/problem-set/binary-tree-level-ii) [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/) |
| 垂序遍历                   | [987. 二叉树的垂序遍历](https://leetcode.cn/problems/vertical-order-traversal-of-a-binary-tree/) |
| 编号问题                   | [1104. 二叉树寻路](https://leetcode.cn/problems/path-in-zigzag-labelled-binary-tree/) [662. 二叉树最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/) [666. 路径总和 IV](https://labuladong.online/algo/problem-set/binary-tree-traverse-iii) [1261. 在受污染的二叉树中查找元素](https://leetcode.cn/problems/find-elements-in-a-contaminated-binary-tree/) |
| 位运算的使用               | [1457. 二叉树中的伪回文路径](https://leetcode.cn/problems/pseudo-palindromic-paths-in-a-binary-tree/) |
| 多叉树                     | [🔒1245. 树的直径](https://labuladong.online/algo/problem-set/binary-tree-post-order-ii/#_1245-树的直径) |

[🔒582. 杀掉进程](https://labuladong.online/algo/problem-set/binary-tree-level-ii/#_582-杀掉进程)

[🔒536. 从字符串生成二叉树](https://labuladong.online/algo/problem-set/binary-tree-level-ii/#_536-从字符串生成二叉树)

[1379. 找出克隆二叉树中的相同节点](https://leetcode.cn/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/)

[1430. 判断给定的序列是否是二叉树从根到叶的路径](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view/#_1430-判断给定的序列是否是二叉树从根到叶的路径)



# Trie前缀树

Trie == 前缀树 == 字典树

| Trie              |                                                              |
| ----------------- | ------------------------------------------------------------ |
| Trie的实现        | [208.实现Trie](https://leetcode.cn/problems/implement-trie-prefix-tree/) [1804.实现Trie II](https://labuladong.online/algo/problem-set/trie) |
| Trie基本API的应用 | [648.单词替换](https://leetcode.cn/problems/replace-words/) [211.添加与搜索单词](https://leetcode.cn/problems/design-add-and-search-words-data-structure/) [677.键值映射](https://leetcode.cn/problems/map-sum-pairs/) |
| Trie解决问题      | [421.两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/) [212.单词搜索II](https://leetcode.cn/problems/word-search-ii/) |



# 链表问题

| 链表的8类问题   | 题号                                                         |
| :-------------- | ------------------------------------------------------------ |
| 链表反转        | [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/) [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/) [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/) [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/) |
| 链表合并        | [21. 合并两个有序链表 ](https://leetcode.cn/problems/merge-two-sorted-lists/) [23.合并K个有序链表 ](https://leetcode.cn/problems/merge-k-sorted-lists/) [丑数问题](##丑数问题) [378. 有序矩阵中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/) [373. 查找和最小的 K 对数字](https://leetcode.cn/problems/find-k-pairs-with-smallest-sums/) |
| 链表第k结点问题 | [19. 删除链表的倒数第N个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)   [876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/) |
| 链表环问题      | [141. 判断链表是否有环](https://leetcode.cn/problems/linked-list-cycle/description/) [142.判断链表环的起点 ](https://leetcode.cn/problems/linked-list-cycle-ii/description/) |
| 链表相交问题    | [160. 返回两个相交链表的相交结点 ](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/) |
| 链表分隔        | [86.分隔链表PartitionList](https://leetcode.cn/problems/partition-list/) |
| 丑数问题        | [丑数问题相关题目](##丑数问题)                               |



# 前缀和 & 差分

| 前缀和问题            |                                                              |
| --------------------- | ------------------------------------------------------------ |
| 前缀和                | [303.区域和检索](https://leetcode.cn/problems/range-sum-query-immutable/) |
| 前缀积                | [238.除自身以外数组的乘积 ](https://leetcode.cn/problems/product-of-array-except-self/) |
| 整除(模为0)问题       | [523.和为k的倍数的子数组](https://leetcode.cn/problems/continuous-subarray-sum/) [1590.使数组和能被P整除](https://leetcode.cn/problems/make-sum-divisible-by-p/) [974.和可被K整除的子数组的个数](https://leetcode.cn/problems/subarray-sums-divisible-by-k/) |
| 位运算表示状态        | [1371.每个元音包含偶数次的最长子字符串](https://leetcode.cn/problems/find-the-longest-substring-containing-vowels-in-even-counts/) |
|                       | (求最长或最短长度)[525. 含有相同数量1和0的最长子数组](https://leetcode.cn/problems/contiguous-array/) [325.和为k的最长子数组](https://labuladong.online/algo/problem-set/perfix-sum) [1124.好的最长时间段](https://leetcode.cn/problems/longest-well-performing-interval/) |
|                       | (求子数组个数)[560.和为K的子数组的个数 ](https://leetcode.cn/problems/subarray-sum-equals-k/) [437.路径总和III](https://leetcode.cn/problems/path-sum-iii/) |
| 一维差分              | [1094.拼车](https://leetcode.cn/problems/car-pooling/) [1109.航班预订统计](https://leetcode.cn/problems/corporate-flight-bookings/) [🔒370.区间加法](https://leetcode.cn/problems/range-addition/) |
| 二维前缀和            | [304.二维区域和检索](https://leetcode.cn/problems/range-sum-query-2d-immutable/) [1314.矩阵区域和](https://leetcode.cn/problems/matrix-block-sum/) [1139.最大的以1为边界的正方形](https://leetcode.cn/problems/largest-1-bordered-square/) |
| 二维差分              | (扫描线问题)[LCP74.最强祝福力场](https://leetcode.cn/problems/xepqZ5/) |
| 二维前缀和 + 二维差分 | [2132.用邮票贴满网格图](https://leetcode.cn/problems/stamping-the-grid/) |



# 滑动窗口

| 滑动窗口问题类型 | 题目                                                         |
| ---------------- | ------------------------------------------------------------ |
| 求最值           | [209.和为k的最短子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/) [1658.x减到0的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/) [1004.最大连续1的个数III](https://leetcode.cn/problems/max-consecutive-ones-iii/) |
| 求个数           | [713.积小于K的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/) [992.K个不同数子数组](https://leetcode.cn/problems/subarrays-with-k-different-integers/) |
| 添加约束         | [395.每个字符重复K次以上的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/) |
| 字符串字串问题   | [438.字符串中所有异位词的位置](https://leetcode.cn/problems/find-all-anagrams-in-a-string/) [567.字符串排列](https://leetcode.cn/problems/permutation-in-string/) [424.替换后的重复字符](https://leetcode.cn/problems/longest-repeating-character-replacement/) |
|                  | [3.无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) [219.存在重复元素II](https://leetcode.cn/problems/contains-duplicate-ii/) |
|                  | [76.最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/) [1234.替换最小子串使其平衡](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/) |
| TreeMap为窗口    | [220.存在重复元素III](https://leetcode.cn/problems/contains-duplicate-iii/) |
| 其他             | [134.加油站](https://leetcode.cn/problems/gas-station/)      |

滑动窗口灵魂三问:

1. 什么时候应该扩大窗口?
2. 什么时候应该缩小窗口?
3. 什么时候得到一个合法的答案?



# 双指针

| 双指针问题类型       |                                                              |
| -------------------- | ------------------------------------------------------------ |
| 简单问题             | [27.移除元素](https://leetcode.cn/problems/remove-element/) [283.移动零](https://leetcode.cn/problems/move-zeroes/) [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/) [🔒360.有序转化数组](https://leetcode.cn/problems/sort-transformed-array) |
| 反转, 回文问题       | [151.反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/) [344.反转字符串](https://leetcode.cn/problems/reverse-string/) [5.最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/) [906.超级回文数](https://leetcode.cn/problems/super-palindromes/) |
| 发货思想             | [922.奇偶排序数组II](https://leetcode.cn/problems/sort-array-by-parity-ii/) |
| 重复问题             | [287.寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/) [26.有序数组去重](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/) [83.排序链表去重](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/) |
| 接雨水问题(单调问题) | [11.盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/) [42.接雨水](https://leetcode.cn/problems/trapping-rain-water/) |
| 贪心思想             | [881.救生艇](https://leetcode.cn/problems/boats-to-save-people/) [475.供暖器](https://leetcode.cn/problems/heaters/) |
|                      | [41.缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/) |
| 异或                 | [389.找不同](https://leetcode.cn/problems/find-the-difference/) |
| 映射                 | [645.错误的集合](https://leetcode.cn/problems/set-mismatch/) [448.数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/) [442.数组中重复的数据](https://leetcode.cn/problems/find-all-duplicates-in-an-array/) |



# 单调栈

单调栈灵魂三问:

1. 大压小 还是 小压大? 
   - 如果求更小元素, 大压小; 如果求更大元素, 小压大.
2. 什么时候要求严格单调, 什么时候不用严格单调?
   - 找下(上)一个更小(大) 或等于 元素时, 需要严格单调
3. 出栈时得到答案 还是入栈时得到答案?
   - 出栈时
   - ~~入栈时得到的答案是上一个更小(大)元素; 出栈时得到的答案是下一个更小(大)元素~~

| 单调栈                         | 单调栈用来找到左右两侧离当前位置最近的更小(更大)的数.        |
| ------------------------------ | ------------------------------------------------------------ |
| 单调栈的本体                   | [牛客单调栈](https://www.nowcoder.com/practice/2a2c00e7a88a498693568cef63a4b7bb) |
| 下一个更大元素                 | [496.下一个更大元素I](https://leetcode.cn/problems/next-greater-element-i/) [739.每日温度](https://leetcode.cn/problems/daily-temperatures/) [1019.链表下一个更大节点](https://leetcode.cn/problems/next-greater-node-in-linked-list/) |
| 下一个更小元素                 | [1475.商品折扣后的价格](https://leetcode.cn/problems/final-prices-with-a-special-discount-in-a-shop/) |
| 上一个更小元素                 | [901.股票价格跨度](https://leetcode.cn/problems/online-stock-span/) |
| 单调栈的应用                   | [1944.队列可以看到的人数](https://leetcode.cn/problems/number-of-visible-people-in-a-queue/) [402.移掉K位数字](https://leetcode.cn/problems/remove-k-digits/) |
| 需要枚举所有子数组(字串)的问题 | [828.所有子串中出现一次的字符个数](https://leetcode.cn/problems/count-unique-characters-of-all-substrings-of-a-given-string/) [907.所有子数组最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/) [1856.所有子数组得分的最大值](https://leetcode.cn/problems/maximum-subarray-min-product/) [2104.所有子数组的最大差值和](https://leetcode.cn/problems/sum-of-subarray-ranges/) [2281. 所有子数组得分的和](https://leetcode.cn/problems/sum-of-total-strength-of-wizards/) |
| 柱状图中最大的矩形             | [84.柱状图的最大矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/) [85.最大矩形](https://leetcode.cn/problems/maximal-rectangle/)(压缩数组, 二维压成一维) |
| 单调思想                       | [962.最大宽度坡](https://leetcode.cn/problems/maximum-width-ramp/) |
|                                | [316.字符串去重复](https://leetcode.cn/problems/remove-duplicate-letters/) |
| 大鱼吃小鱼问题                 | [2289.使数组递增的操作步数](https://leetcode.cn/problems/steps-to-make-array-non-decreasing/) |
|                                | [1504.统计全1子矩形](https://leetcode.cn/problems/count-submatrices-with-all-ones/) |



# 单调队列

| 单调队列            |                                                              |
| ------------------- | ------------------------------------------------------------ |
| 单调队列的本体      | [239.滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/) [LCR184.设计自助结算系统](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/) [918.环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/) [1499.点的最大得分](https://leetcode.cn/problems/max-value-of-equation/) |
| 单调队列 + 滑动窗口 | [1438.绝对差不超过限制的最长连续子数组](https://leetcode.cn/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/) [862.和至少为K的最短子数组](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/) |
| 单调队列 + 动态规划 | [1696.跳跃游戏VI](https://leetcode.cn/problems/jump-game-vi/) [1425.带限制的子序列和](https://leetcode.cn/problems/constrained-subsequence-sum/) [🔒1429.第一个唯一数字](https://labuladong.online/algo/problem-set/monotonic-queue/#_1429-第一个唯一数字) |



# 二分答案法

| 问题类型             | 题目                                                         |
| -------------------- | ------------------------------------------------------------ |
| 二分搜索的本体       | [704.二分查找](https://leetcode.cn/problems/binary-search/) [34.排序数组查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/) [LCR172.统计目标成绩的出现次数](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/) |
| 二分搜索的变体       | [33.搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/) [81.搜索旋转排序数组II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/) [LCR173.寻找缺失的数](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/) [852.数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/) [162.寻找峰值](https://leetcode.cn/problems/find-peak-element/) |
| 运用到二分搜索的题目 | [658.找到K个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/) [1201.丑数III](https://leetcode.cn/problems/ugly-number-iii/) [878.第N个神奇数字](https://leetcode.cn/problems/nth-magical-number/) |
| 二分答案法           | [875.珂珂吃香蕉](https://leetcode.cn/problems/koko-eating-bananas/) [719.找出第K小的数对距离](https://leetcode.cn/problems/find-k-th-smallest-pair-distance/) |
| (画匠问题)           | [410.分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/) [1011.在D天内送达包裹的能力](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/) |
|                      | [2141.同时运行N台电脑的最长时间](https://leetcode.cn/problems/maximum-running-time-of-n-computers/) |
| 田忌赛马             | [870.优势洗牌田忌赛马](https://leetcode.cn/problems/advantage-shuffle/) [2071.可以安排的最多任务数目](https://leetcode.cn/problems/maximum-number-of-tasks-you-can-assign/) |

二分答案法的灵魂三步:

1. 估计最终答案的范围
2. 确定 单调函数 f(答案) = 求出的要求
3. 在答案的范围上二分, 找到一个答案, 使得 f(答案) = 要求



# 回溯算法

| 问题类型               | 题目                                                         |
| ---------------------- | ------------------------------------------------------------ |
| 运用位运算             | [51.N 皇后](https://leetcode.cn/problems/n-queens/) [52.N皇后II](https://leetcode.cn/problems/n-queens-ii/) |
| 排列问题               | [46.全排列](https://leetcode.cn/problems/permutations/) [47.全排列II](https://leetcode.cn/problems/permutations-ii/) |
| 组合问题(类似nSum问题) | [39.组合总和](https://leetcode.cn/problems/combination-sum/) [40.组合总和II](https://leetcode.cn/problems/combination-sum-ii/) [216.组合总和III](https://leetcode.cn/problems/combination-sum-iii/) |
| 子集问题               | [78.子集](https://leetcode.cn/problems/subsets/) [90.子集II](https://leetcode.cn/problems/subsets-ii/) |
| 集合划分问题           | [698.划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/) |



子集/组合问题: 每次循环都从指定的位置开始遍历后半部分, 可复选递归调用时位置不变.

排列问题: (全排列和普通排列收集结果时机不同)每次循环都从头到尾遍历全部, 可复选不需要isUsed[], 

| 问题类型                                             | 题目                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| DFS                                                  | [980.不同路径 III](https://leetcode.cn/problems/unique-paths-iii/) [79.单词搜索](https://leetcode.cn/problems/word-search/) [1219.黄金矿工](https://leetcode.cn/problems/path-with-maximum-gold/) |
|                                                      | [93.复原IP](https://leetcode.cn/problems/restore-ip-addresses/) [17.电话字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/) [2850.分散石头](https://leetcode.cn/problems/minimum-moves-to-spread-stones-over-grid/) [332.重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/) |
| 洪水覆盖算法                                         | [200.岛屿数量](https://leetcode.cn/problems/number-of-islands/) [1254.封闭岛屿数量](https://leetcode.cn/problems/number-of-closed-islands/) [1020.岛面积](https://leetcode.cn/problems/number-of-enclaves/) [ 695.岛屿最大面积](https://leetcode.cn/problems/max-area-of-island/) [1905.统计子岛屿](https://leetcode.cn/problems/count-sub-islands/) [694.不同岛屿数量](https://labuladong.online/algo/frequency-interview/island-dfs-summary) [130.被围绕区域](https://leetcode.cn/problems/surrounded-regions/) [827.最大人工岛](https://leetcode.cn/problems/making-a-large-island/) [803.打砖块](https://leetcode.cn/problems/bricks-falling-when-hit/) |
| 无重不可复选                                         | [784.字母大小写全排列](https://leetcode.cn/problems/letter-case-permutation/) |
| 有重不可复选(排序+(nums[i]==nums[i - 1]) 或 HashSet) | [491.非递减子序列](https://leetcode.cn/problems/non-decreasing-subsequences/) [1079.活字印刷](https://leetcode.cn/problems/letter-tile-possibilities/) [996.平方数组数目](https://leetcode.cn/problems/number-of-squareful-arrays/) |
| 可复选                                               | [967.连续差相同的数字](https://leetcode.cn/problems/numbers-with-same-consecutive-differences/) [638.大礼包](https://leetcode.cn/problems/shopping-offers/) |
| 划分k个子集                                          | [698. 划分k个子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/) [473. 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/) |
| 字符串拆分                                           | [1849.字符串拆为递减连续值](https://leetcode.cn/problems/splitting-a-string-into-descending-consecutive-values/) [1593.唯一子字符串的最大数目](https://leetcode.cn/problems/split-a-string-into-the-max-number-of-unique-substrings/) [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/) |
| 球盒问题                                             | [526.优美排列](https://leetcode.cn/problems/beautiful-arrangement/) [1723.完成所有工作的最短时间](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/) [2305.公平分发饼干](https://leetcode.cn/problems/fair-distribution-of-cookies/) |
|                                                      | [🔒293.翻转游戏](https://labuladong.online/algo/problem-set/backtrack-iii/#_293-翻转游戏) [🔒294.翻转游戏II](https://labuladong.online/algo/problem-set/backtrack-iii/#_294-翻转游戏-ii) [🔒291.单词规律II](https://labuladong.online/algo/problem-set/backtrack-iii/#_291-单词规律-ii) [🔒267.回文排列II](https://labuladong.online/algo/problem-set/backtrack-iii/#_267-回文排列-ii) [🔒254.因子组合](https://labuladong.online/algo/problem-set/backtrack-iii/#_254-因子的组合) |

剪枝方法汇总:

1. 状态为矩阵时, 不能等到矩阵上全放完了才判断是否是一个结果, 而是需要放之前就判断能不能放, 如n皇后问题, pdd笔试题2
2. 



# BFS算法

| 问题类型         | 题目                                                         |
| ---------------- | ------------------------------------------------------------ |
| 例题             | [752.打开转盘锁](https://leetcode.cn/problems/open-the-lock/) [773.滑动谜题](https://leetcode.cn/problems/sliding-puzzle/) |
| 序列上的BFS      | [841.钥匙和房间](https://leetcode.cn/problems/keys-and-rooms/) [1306.跳跃游戏III](https://leetcode.cn/problems/jump-game-iii/) [433.最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/) [127.单词接龙](https://leetcode.cn/problems/word-ladder/) |
| 矩阵的BFS        | [1926.迷宫出口](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/) [1091.最短路径](https://leetcode.cn/problems/shortest-path-in-binary-matrix/) [542.01矩阵](https://leetcode.cn/problems/01-matrix/) [2850.分散石头](https://leetcode.cn/problems/minimum-moves-to-spread-stones-over-grid/) [417.水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/) |
|                  | [🔒286.墙与门](https://labuladong.online/algo/problem-set/bfs-ii/#_286-墙与门) [🔒490.迷宫](https://labuladong.online/algo/problem-set/bfs-ii/#_490-迷宫) [🔒505.迷宫 II](https://labuladong.online/algo/problem-set/bfs-ii/#_505-迷宫-ii) [🔒499. 迷宫III](https://labuladong.online/algo/problem-set/bfs-ii/#_499-迷宫-iii) |
| 图的BFS          | [924.恶意软件传播](https://leetcode.cn/problems/minimize-malware-spread/) [2101.引爆最多的炸弹](https://leetcode.cn/problems/detonate-the-maximum-bombs/) [399.除法求值](https://leetcode.cn/problems/evaluate-division/) |
|                  | [365.水壶问题](https://leetcode.cn/problems/water-and-jug-problem/) [721.账户合并](https://leetcode.cn/problems/accounts-merge/) |
| 多源BFS          | [994.腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/) [1162.地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/) |
| 0-1BFS           | [2290.到达角落移除障碍物最小数目](https://leetcode.cn/problems/minimum-obstacle-removal-to-reach-corner/) [1368.到达角落的最小代价](https://leetcode.cn/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/) |
| 将邻接表作为容器 | [691.贴纸拼词](https://leetcode.cn/problems/stickers-to-spell-word/) |
| BFS+堆           | [407.接雨水II](https://leetcode.cn/problems/trapping-rain-water-ii/) |
| BFS的所有路径    | [126.单词接龙II](https://leetcode.cn/problems/word-ladder-ii/)(HashSet代替队列, adjList存所有路径) |
| 双向BFS          | [127.单词接龙](https://leetcode.cn/problems/word-ladder/) [1755.最接近目标值的子序列和](https://leetcode.cn/problems/closest-subsequence-sum/) |



# 并查集

| 问题类型 | 题目                                                         |
| -------- | ------------------------------------------------------------ |
|          | [990.等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/) [839.相似字符串组](https://leetcode.cn/problems/similar-string-groups/) [947.移除同行或同列石头](https://leetcode.cn/problems/most-stones-removed-with-same-row-or-column/) |
|          | [2092.找出知晓秘密的所有专家](https://leetcode.cn/problems/find-all-people-with-secret/) |
|          | [765.情侣牵手](https://leetcode.cn/problems/couples-holding-hands/) [2421.好路径的数目](https://leetcode.cn/problems/number-of-good-paths/) |
|          | [928.恶意软件传播II](https://leetcode.cn/problems/minimize-malware-spread-ii/) |
|          | [🔒323.无向图中连通分量的数目](https://labuladong.online/algo/data-structure/union-find/) [684.冗余连接](https://leetcode.cn/problems/redundant-connection/) [🔒261.以图判树](https://labuladong.online/algo/data-structure/kruskal/) |



# 图

**链式前向星建图**

- 将每个结点的边为一条链表, 遍历所有边时用 **头插法** 添加边
- `int head[];` head[i] 表示结点i的第一条边
- `int next[];` next[i] 表示第i条边的下一条边
- `int to[];` to[i] 表示第i条边的尾结点
- `int count = 1;` count表示当前遍历到的边的编号
- `int weight[];` weight[i] 表示第i条边的权重



| 问题类型                      | 题目                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| 拓扑排序                      | [207.课程表](https://leetcode.cn/problems/course-schedule/) [210.课程表II ](https://leetcode.cn/problems/course-schedule-ii/) [LCR114.火星词典](https://leetcode.cn/problems/Jf1JuT/) [936.戳印序列](https://leetcode.cn/problems/stamping-the-sequence/) |
| 拓扑排序带信息                | 拓扑排序的路径数量 [851.喧闹和富有](https://leetcode.cn/problems/loud-and-rich/) [2050.并行课程III](https://leetcode.cn/problems/parallel-courses-iii/) [2127.参加会议的最多员工数](https://leetcode.cn/problems/maximum-employees-to-be-invited-to-a-meeting/) |
| 最小生成树问题(Kruskal, Prim) | [1584.连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/) [🔒1135.联通城市](https://labuladong.online/algo/data-structure/kruskal/) [1697.查询边长度限制的路径](https://leetcode.cn/problems/checking-existence-of-edge-length-limited-paths/) |
| 最短路径问题                  | [743.Dijkstra](https://leetcode.cn/problems/network-delay-time/) [1631.路径为最大差](https://leetcode.cn/problems/path-with-minimum-effort/) [1514.Dijkstra求最长路径](https://leetcode.cn/problems/path-with-maximum-probability/) [778.路径为路上结点的最大值](https://leetcode.cn/problems/swim-in-rising-water/) |
|                               | Bellman-Ford与SPFA(可以有负边) [787.K站中转内最便宜的航班](https://leetcode.cn/problems/cheapest-flights-within-k-stops/) |
|                               | Floyd求所有结点相互之间的最短路径(可以有负边)                |
|                               | A*求源结点到目标结点最短路径, 堆按 源点到当前结点距离+当前点到终点的预估距离 排序 |
| 分层图最短路(扩点最短路)      | [864.获取所有钥匙的最短路径](https://leetcode.cn/problems/shortest-path-to-get-all-keys/) [LCP35.电动车游城市](https://leetcode.cn/problems/DFPeFJ/) |
| 名人问题                      | [🔒277. 搜寻名人](https://labuladong.online/algo/frequency-interview/find-celebrity/) |
| 二分图问题                    | [785.判断二分图](https://leetcode.cn/problems/is-graph-bipartite/) [886.可能的二分法](https://leetcode.cn/problems/possible-bipartition/) |

**Kruskal算法思想**: 将边按权重从小到大排序, 从小到大遍历边, 如果边的两头结点不连通, 将边的两头结点用并查集连通

**Prim算法思想**: 从任意一个结点开始作为树根, 选一条最小的边加入当前的树, 直到所有结点都加入当前的树.

**Dijkstra算法: ** 

1. 维护一个dist[n]存储指定结点到i的最短距离; 
2. 以指定结点为根开始BFS遍历, 每遍历到一个结点就将dist[n]更新为更小的. 
3. BFS遍历用优先级队列, 优先级队列的元素是一个新的类(结点, 根到结点的距离)
4. 当 从==队列中取出的元素== 到根的距离 大于 当前dist[]中对应的值 就不需要遍历该结点后面的路径了, 直接去取下一个值. 
   1. 为什么 等于 的时候还要遍历? 因为等于的时候说明刚从队里出来, 它的邻居可能还没入队; 大于的时候至少是第二次到这个结点了, 他的邻居已经入过队了

5. 当 ==结点的邻居== 到根的距离(按当前路径到达邻居) 大于 当前dist[]中邻居对应的值 就不需要将路径入队



# 还没分类

| 解题方法               | 题目                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Rabin Karp字符匹配算法 |                                                              |
| 二维数组               | [48.旋转矩阵](https://leetcode.cn/problems/rotate-image/) [54.螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/) [59.螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/) |
| nSum问题               | [167.两数之和II](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/) [15.三数之和](https://leetcode.cn/problems/3sum/) [18.四数之和](https://leetcode.cn/problems/4sum/) [421.两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/) [16.最接近的三数之和](https://leetcode.cn/problems/3sum-closest/) |
| 区间问题               | [1288.删除被覆盖区间](https://leetcode.cn/problems/remove-covered-intervals/) [56.合并区间](https://leetcode.cn/problems/merge-intervals/) [986.区间列表的交集](https://leetcode.cn/problems/interval-list-intersections/) |
| 随机问题               | [528.按权重随机选择](https://leetcode.cn/problems/random-pick-with-weight/) |
| 考场座位分配           | [855.考场就座](https://leetcode.cn/problems/exam-room/)      |



# 动态规划

| 基础DP | 题目                                                         |
| ------ | ------------------------------------------------------------ |
| 一维DP | [983.最低票价](https://leetcode.cn/problems/minimum-cost-for-tickets/) [91.解码方法](https://leetcode.cn/problems/decode-ways/) [639.解码方法II](https://leetcode.cn/problems/decode-ways-ii/) |
|        | 子数组问题 [32.最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/) |
|        | 需要消除重复的问题 [467.环绕字符串](https://leetcode.cn/problems/unique-substrings-in-wraparound-string/) [940.不同子序列II](https://leetcode.cn/problems/distinct-subsequences-ii/) |
| 二维DP | [64.最小路径和](https://leetcode.cn/problems/minimum-path-sum/) [329.矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/) |
|        | [1143.最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/) [115.不同的子序列](https://leetcode.cn/problems/distinct-subsequences/) [72.编辑距离](https://leetcode.cn/problems/edit-distance/) |
|        | [516.最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/) |
|        | 是否问题 [97.交错字符串](https://leetcode.cn/problems/interleaving-string/) |
| 三维DP | [474.一和零](https://leetcode.cn/problems/ones-and-zeroes/) [879.盈利计划](https://leetcode.cn/problems/profitable-schemes/) [688.骑士在棋盘上的概率](https://leetcode.cn/problems/knight-probability-in-chessboard/) [2435.矩阵中和能被K整除的路径数](https://leetcode.cn/problems/paths-in-matrix-whose-sum-is-divisible-by-k/) |
|        | 是否问题 [87.扰乱字符串](https://leetcode.cn/problems/scramble-string/) |

子数组问题:

1. dp[i]是以s[i]结尾的字串答案
2. dp[i]是必须含有且结尾为s[i]的答案, 也就是s[i]往前最多推多远的合法子串
3. 最终的答案要遍历dp[]



需要消除重复的问题:

- dp[] = new int[26]; dp[i]表示以char('a'+i)结尾的子串答案



子序列问题的两种方法:

- 第一种方法: dp[i]表示以i结尾的子序列答案, 时间复杂度O(n^2)
- 第二种方法:
  - 对于2个串: dp[i]\[j\]表示s1[0...i]和s2[0...j]的答案
  - 对于1个串: dp[i]\[j\]表示s[i...j]的答案



| DP基础       | 题目                                                         |
| ------------ | ------------------------------------------------------------ |
| 记忆化搜索   | [139.单词拆分](https://leetcode.cn/problems/word-break/) [140.单词拆分II](https://leetcode.cn/problems/word-break-ii/) |
| 爬楼梯问题   | [70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/) [746.爬楼梯最小花费](https://leetcode.cn/problems/min-cost-climbing-stairs/) [377.组合总和Ⅳ](https://leetcode.cn/problems/combination-sum-iv/) [2466.构造好字符串方案数](https://leetcode.cn/problems/count-ways-to-build-good-strings/) [2266.打字方案数](https://leetcode.cn/problems/count-number-of-texts/) [🔒2533.好二进制字符串数量](https://leetcode.cn/problems/number-of-good-binary-strings/) |
| 打家劫舍问题 | [198.打家劫舍](https://leetcode.cn/problems/house-robber/) [213.打家劫舍II](https://leetcode.cn/problems/house-robber-ii/) [337.打家劫舍III](https://leetcode.cn/problems/house-robber-iii/) [2320.放置房子的方案数](https://leetcode.cn/problems/count-number-of-ways-to-place-houses/) [2560.打家劫舍IV](https://leetcode.cn/problems/house-robber-iv/) [740.删除获得点数](https://leetcode.cn/problems/delete-and-earn/) [3186.施咒的最大总伤害](https://leetcode.cn/problems/maximum-total-damage-with-spell-casting/) |
| 子数组问题   | [2606.子串的最大开销](https://leetcode.cn/problems/find-the-substring-with-maximum-cost/) [1749.子数组和的绝对值的最大值](https://leetcode.cn/problems/maximum-absolute-sum-of-any-subarray/)  [1191.K次串联后子数组最大和](https://leetcode.cn/problems/k-concatenation-maximum-sum/) [2321.拼接数组的最大分数](https://leetcode.cn/problems/maximum-score-of-spliced-array/) |



## 子数组问题

| 子数组问题          |                                                              |
| ------------------- | ------------------------------------------------------------ |
|                     | [53.最大子数组和](https://leetcode.cn/problems/maximum-subarray/) [198.打家劫舍](https://leetcode.cn/problems/house-robber/) [152.乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/) |
| 二分答案法+打家劫舍 | [2560.打家劫舍IV](https://leetcode.cn/problems/house-robber-iv/) |
|                     | [689.三个无重叠子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-3-non-overlapping-subarrays/) |
| 环形子数组问题      | [918.环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/) [213.打家劫舍II](https://leetcode.cn/problems/house-robber-ii/) |
| 子矩阵问题          | (压缩数组)[面试题17.24.最大子矩阵](https://leetcode.cn/problems/max-submatrix-lcci/) |



## 子序列问题

| 序列问题            |                                                              |
| ------------------- | ------------------------------------------------------------ |
| LCS                 | [115.不同子序列](https://leetcode.cn/problems/distinct-subsequences/) [1143.最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/) |
| 编辑距离问题        | [583.两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/) [712.两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/) [72.编辑距离](https://leetcode.cn/problems/edit-distance/) |
| LIS                 | [300.最长递增子序列 ](https://leetcode.cn/problems/longest-increasing-subsequence/) [354.信封问题](https://leetcode.cn/problems/russian-doll-envelopes/) [2111.使数组K递增的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-make-the-array-k-increasing/) |
| 回文子序列          | [516.最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/) [1312.字符串成为回文串的最少插入次数](https://leetcode.cn/problems/minimum-insertion-steps-to-make-a-string-palindrome/) |
| end更新和查询的分离 | [646.最长数对链](https://leetcode.cn/problems/maximum-length-of-pair-chain/) |



## 背包DP

| 背包问题    |                                                              |
| ----------- | ------------------------------------------------------------ |
| 0-1背包问题 | 不可复选, 0-1背包问题本体是求可以不装满的最大价值            |
|             | 求最大 [474.一和零](https://leetcode.cn/problems/ones-and-zeroes/) [bytedance-006.夏季特惠](https://leetcode.cn/problems/tJau2o/) [1049.最后一块石头的重量II](https://leetcode.cn/problems/last-stone-weight-ii/) |
|             | 恰好装满 [416.分割数组](https://leetcode.cn/problems/partition-equal-subset-sum/) [2915.和为目标值的最长子序列](https://leetcode.cn/problems/length-of-the-longest-subsequence-that-sums-to-target/) [494.目标和](https://leetcode.cn/problems/target-sum/)  [2787.数字表示成幂的和](https://leetcode.cn/problems/ways-to-express-an-integer-as-sum-of-powers/) |
|             | 最接近: 一个变量记录大于的情况, 求不超过容量的最大, 两者取近的 [1774.最接近目标价格的甜点成本](https://leetcode.cn/problems/closest-dessert-cost/) |
|             | [3290.最高乘法得分](https://leetcode.cn/problems/maximum-multiplication-score/) |
| 分组背包    | 一组内的物品只能或者最多选1个 [2218.从栈中取出K个硬币的最大面值和](https://leetcode.cn/problems/maximum-value-of-k-coins-from-piles/) |
| 完全背包    | 可复选 [10.正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/) [44.通配符匹配](https://leetcode.cn/problems/wildcard-matching/) [322.凑零钱](https://leetcode.cn/problems/coin-change/) [518.凑零钱II](https://leetcode.cn/problems/coin-change-ii/) [279.完全平方数](https://leetcode.cn/problems/perfect-squares/) [1449.固定成本的最大数字](https://leetcode.cn/problems/form-largest-integer-with-digits-that-add-up-to-target/) |
| 多重背包    | 可复选, 有个数限制                                           |

## 区间DP

| 区间DP                 |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| 基于两侧端点讨论       | [1312.让字符串成为回文串的最少插入次数](https://leetcode.cn/problems/minimum-insertion-steps-to-make-a-string-palindrome/) |
|                        | [486.预测赢家](https://leetcode.cn/problems/predict-the-winner/) |
| 基于范围上划分点的讨论 | [1039.多边形三角剖分的最低得分](https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/) [1547.切棍子的最小成本](https://leetcode.cn/problems/minimum-cost-to-cut-a-stick/) |
|                        | [312.戳气球](https://leetcode.cn/problems/burst-balloons/) [面试题08.14.布尔运算](https://leetcode.cn/problems/boolean-evaluation-lcci/) |
| 两种方法综合           | [664.奇怪的打印机](https://leetcode.cn/problems/strange-printer/) |
| 带信息递归             | [546.移除盒子](https://leetcode.cn/problems/remove-boxes/)   |
|                        | [1000.合并石头的最低成本](https://leetcode.cn/problems/minimum-cost-to-merge-stones/) [730.统计不同回文子序列](https://leetcode.cn/problems/count-different-palindromic-subsequences/) |

## 树形DP

| 树形DP                       |                                                              |
| ---------------------------- | ------------------------------------------------------------ |
| 二叉树DP                     | [1373.树的BST子树的最大键值和](https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/) |
|                              | [543.二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/) [979.在二叉树中分配硬币](https://leetcode.cn/problems/distribute-coins-in-binary-tree/) [337.打家劫舍III](https://leetcode.cn/problems/house-robber-iii/) |
|                              | [968.监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/) [437.路径总和III](https://leetcode.cn/problems/path-sum-iii/) |
| 多叉树DP                     | [2477.到达首都的最少油耗](https://leetcode.cn/problems/minimum-fuel-cost-to-report-to-the-capital/) [2246.相邻字符不同的最长路径](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/) |
| dfn序(Depth First Numbering) | [2458.移除子树后的二叉树高度](https://leetcode.cn/problems/height-of-binary-tree-after-subtree-removal-queries/) [2322.从树中删除边的最小分数](https://leetcode.cn/problems/minimum-score-after-removals-on-a-tree/) |

| 状态压缩DP                          |                                                              |
| ----------------------------------- | ------------------------------------------------------------ |
|                                     | [464.我能赢吗](https://leetcode.cn/problems/can-i-win/)      |
| 分割等和子集(状态为1维)             | [473.火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/) [698.划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/) |
| TSP(Traveling Salesman Problem)问题 |                                                              |
| 状态为2维                           | [1434.每个人戴不同帽子的方案数](https://leetcode.cn/problems/number-of-ways-to-wear-different-hats-to-each-other/) [1994.好子集的数目](https://leetcode.cn/problems/the-number-of-good-subsets/) [1655.分配重复整数](https://leetcode.cn/problems/distribute-repeating-integers/) |

| 状态机DP     |                                                              |
| ------------ | ------------------------------------------------------------ |
| 买卖股票问题 | [121.买卖股票1次](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) [122.买卖股票无限次](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/) [309.买卖股票无限次含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/) [714.买卖股票无限次含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) [123.买卖股票2次](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/) [188.买卖股票k次](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/) |
|              | [903.DI序列的有效排列](https://leetcode.cn/problems/valid-permutations-for-di-sequence/) |
|              | [1235.规划兼职工作](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/) |
|              | [629.K个逆序对数组](https://leetcode.cn/problems/k-inverse-pairs-array/) |
|              | [514.自由之路](https://leetcode.cn/problems/freedom-trail/)  |

| 数位DP                                                       |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [902.最大为N的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/) | [357. 统计各位数字都不同的数字个数](https://leetcode.cn/problems/count-numbers-with-unique-digits/) |
| [2376.统计特殊整数](https://leetcode.cn/problems/count-special-integers/) | [2719.统计整数数目](https://leetcode.cn/problems/count-of-integers/) |
| [1012.至少有1位重复的数字](https://leetcode.cn/problems/numbers-with-repeated-digits/) | [600.不含连续1的非负整数](https://leetcode.cn/problems/non-negative-integers-without-consecutive-ones/) |
| [233.数字1的个数](https://leetcode.cn/problems/number-of-digit-one/) |                                                              |

| 技巧             |                                                              |
| ---------------- | ------------------------------------------------------------ |
| 得到具体方案技巧 | [1125.最小的必要团队](https://leetcode.cn/problems/smallest-sufficient-team/) |
| 根据数据量猜解法 | [1187.使数组严格递增](https://leetcode.cn/problems/make-array-strictly-increasing/) |



| 矩阵上的DP |                                                              |
| ---------- | ------------------------------------------------------------ |
| 路径和     | [LCR166.最大路径和](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/) [64.最小路径和](https://leetcode.cn/problems/minimum-path-sum/) [931.下降最小路径和](https://leetcode.cn/problems/minimum-falling-path-sum/) [120.三角形下降最小路径和](https://leetcode.cn/problems/triangle/) [2304.下降最小路径和](https://leetcode.cn/problems/minimum-path-cost-in-a-grid/) [1289.下降最小路径和II](https://leetcode.cn/problems/minimum-falling-path-sum-ii/) |
| 路径数量   | [62.路径数](https://leetcode.cn/problems/unique-paths/) [63.路径数加障碍II](https://leetcode.cn/problems/unique-paths-ii/) |
| 路径积     | [152.子数组最大积](https://leetcode.cn/problems/maximum-product-subarray/) [1594.最大路径积](https://leetcode.cn/problems/maximum-non-negative-product-in-a-matrix/) |
|            | [174.地下城游戏](https://leetcode.cn/problems/dungeon-game/) |
|            | [2684.矩阵中移动的最大次数](https://leetcode.cn/problems/maximum-number-of-moves-in-a-grid/) |
|            | [741.摘樱桃](https://leetcode.cn/problems/cherry-pickup/)    |



# 贪心

| 贪心类型题目                                                 |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 排序                                                         | [179.最大数](https://leetcode.cn/problems/largest-number/) [1029.两地调度](https://leetcode.cn/problems/two-city-scheduling/) [1553.吃掉N个橘子的最少天数](https://leetcode.cn/problems/minimum-number-of-days-to-eat-n-oranges/) |
| 堆: 返回heap.size()                                          | [630.课程表III](https://leetcode.cn/problems/course-schedule-iii/) |
| 解锁的进入堆                                                 | [1353.最多可以参加的会议数目](https://leetcode.cn/problems/maximum-number-of-events-that-can-be-attended/) [502.IPO](https://leetcode.cn/problems/ipo/) |
|                                                              | [343.整数拆分](https://leetcode.cn/problems/integer-break/) [LCR132.砍竹子II](https://leetcode.cn/problems/jian-sheng-zi-ii-lcof/) 数n分成k份的最大乘积 |
|                                                              | [871.最低加油次数](https://leetcode.cn/problems/minimum-number-of-refueling-stops/) |
|                                                              | [581.最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/) |
|                                                              | [45.跳跃游戏II](https://leetcode.cn/problems/jump-game-ii/) [1326.灌溉花园的最少水龙头数目](https://leetcode.cn/problems/minimum-number-of-taps-to-open-to-water-a-garden/) |
| 有序表TreeSet                                                | [632.最小区间](https://leetcode.cn/problems/smallest-range-covering-elements-from-k-lists/) (奇偶性)[1675.数组的最小偏移量](https://leetcode.cn/problems/minimize-deviation-in-array/) |
| HashMap                                                      | [781.森林中的兔子](https://leetcode.cn/problems/rabbits-in-forest/) |
|                                                              | [2449.使数组相似的最少操作次数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-arrays-similar/) |
|                                                              | [517.超级洗衣机](https://leetcode.cn/problems/super-washing-machines/) |
|                                                              | [1921.消灭怪物的最大数量](https://leetcode.cn/problems/eliminate-maximum-number-of-monsters/) |
| [2384.最大回文数字](https://leetcode.cn/problems/largest-palindromic-number/) | [1792.最大平均通过率](https://leetcode.cn/problems/maximum-average-pass-ratio/) |
| [857.雇佣K名工人的最低成本](https://leetcode.cn/problems/minimum-cost-to-hire-k-workers/) |                                                              |



# 丑数问题

| 题号                                                         | 解决方法                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [204.统计质数](https://leetcode.cn/problems/count-primes/) 计算小于n的质数的数量 | 从2开始, 2\*2不是质数, 2\*3不是质数..., 3\*2不是质数, 3*3不是质数, ... |
| [263.丑数 ](https://leetcode.cn/problems/ugly-number/) 判断一个数是否是丑数(丑数是只有2, 3, 5为因子的数) |                                                              |
| [264.丑数 II](https://leetcode.cn/problems/ugly-number-ii/) 返回第n个丑数 |                                                              |
| [313.超级丑数](https://leetcode.cn/problems/super-ugly-number/) 返回第n个超级丑数(超级丑数是只有指定数组内的数为因子的数) |                                                              |
| [1201.丑数 III](https://leetcode.cn/problems/ugly-number-iii/) |                                                              |



# 质数问题

| 质数问题类型                  |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| 判断一个数是否是质数          |                                                              |
| 质数筛: 计算小于n的质数的数量 | [204.统计质数](https://leetcode.cn/problems/count-primes/) 埃氏筛和欧拉筛 |
| 质因子                        | 找出所有的质数因子 [952.按公因数计算最大组件大小](https://leetcode.cn/problems/largest-component-size-by-common-factor/) |



# 括号问题

| 括号问题       |                                                              |
| -------------- | ------------------------------------------------------------ |
| 有效括号的性质 | [20.有效的括号](https://leetcode.cn/problems/valid-parentheses/) [921.使括号有效的最少添加](https://leetcode.cn/problems/minimum-add-to-make-parentheses-valid/) [1541.平衡括号字符串的最少插入次数](https://leetcode.cn/problems/minimum-insertions-to-balance-a-parentheses-string/) [22.括号生成](https://leetcode.cn/problems/generate-parentheses/) [301.删除无效括号](https://leetcode.cn/problems/remove-invalid-parentheses/) [32.最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/) |
| 括号嵌套问题   | [思路](https://leetcode.cn/problems/longest-valid-parentheses/description/#)[224.基本计算器](https://leetcode.cn/problems/basic-calculator/) [227.基本计算器 II](https://leetcode.cn/problems/basic-calculator-ii/) [772.基本计算器 III🔒](https://leetcode.cn/problems/basic-calculator-iii/) |
|                | [394.字符串解码](https://leetcode.cn/problems/decode-string/) [726. 原子的数量](https://leetcode.cn/problems/number-of-atoms/) |



# 树状数组

| 树状数组问题 | 题目                                                         |
| ------------ | ------------------------------------------------------------ |
|              | [307.区域和检索-数组可修改](https://leetcode.cn/problems/range-sum-query-mutable/) |



| 摩尔投票 |                                                              |
| -------- | ------------------------------------------------------------ |
|          | [169.多数元素](https://leetcode.cn/problems/majority-element/) |



# KMP算法

1. next[]是什么? `next[i]`是 ==s2[0] ~ s2[i-1] 的前后缀最大匹配长度==(不包含s2[i] 这个字符, 也不包括整体字符), `next[0] = -1, next[1] = 0`
2. 怎么通过 next[] 加速匹配过程? 
   - 当遇到不匹配的字符时: p1指针不动; p2指针回到next[p2]
   - (p2+1和p1+1匹配失败时, next[p2+1] = val)
     1. 为什么p1 ~ p1-val 这部分开始不可能匹配成功?
        1. p2和p1匹配失败时, s1[0, p1] == s2[0, p2]
        2. 如果存在 0 ~ p1-val 之间的位置 k 匹配成功, 则三部分相等: s1[k, p1] = s2[k, p1] = s2[0, p1-k], 因为 k < p1 - val, 所以这三个相等的部分的长度 > val了, 矛盾
     2. 为什么从s2的next[p2]位置开始匹配, s2的next[p2]前面的不管?
        1. p2和p1匹配失败时, s1[0, p1] == s2[0, p2]整体相等, s2[0, p2]的前val个字符 和 s2[0, p2]的后val个字符 相等
        2. 重新开始匹配时, s2的前val个字符不需要匹配了, 直接从第val+1个字符开始



# PDD笔试题11.10

> 1. 有一个长度为n的数组(9<=n<=13), 要从数组中选出9个数放入3*3的矩阵中, 求是否存在一个方案, 使得矩阵中的 行, 列, 对角线上的和都相等, 返回true或false
>    1. 关键剪枝: 每做出一个选择就验证一次
> 2. 每个旅行有三个参数, 优先级, 首次出发时间, 下次出发的间隔时间. 优先级高的必须先旅行, 每个旅行耗费1天且1天只能旅行一次. 求完成所有旅行的最少天数
> 3. 一个带权无向图, 按结点分割成两个图, 求分割后的两个图中的权重最大边的最小.



# 面试题

字节 [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

字节 题目：给定一个数n如23121;给定一组数字a如[2 4 9]，求由a中元素组成的小于n的最大数

字节 题目：输入两颗二叉树, 判断B是否是A的子结构约定空树不是任意树的子结构

携程: 约瑟夫环

微派: 相交链表带环问题

美团题: 20亿的int存在磁盘, 怎么对文件的数据排序?这些数据没有重复, 怎么做到O(n)?



