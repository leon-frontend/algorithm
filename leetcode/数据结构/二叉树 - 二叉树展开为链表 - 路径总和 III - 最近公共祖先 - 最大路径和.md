# 1. 二叉树展开为链表

> _**问题描述**_
>
> > [!important] **[LeetCode - 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你二叉树的根结点 `root` ，请你将它展开为一个单链表。展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。展开后的单链表应该与二叉树 **先序遍历** 顺序相同。>

> _**解法 1 ⇒ 前序遍历**_
>
> - 时间复杂度：$O(n)$，其中 $n$ 是二叉树的节点数。前序遍历的时间复杂度是 $O(n)$，前序遍历之后，需要对每个节点更新左右子节点的信息，时间复杂度也是 $O(n)$
> 
> - 空间复杂度：$O(n)$，其中 _**n**_ 是二叉树的节点数。空间复杂度取决于栈（递归调用栈或者迭代中显性使用的栈）和存储前序遍历结果的列表的大小，栈内的元素个数不会超过 _**n**_，前序遍历列表中的元素个数是 _**n**_。
> 
> ```Java
> public void flatten(TreeNode root) {
>     if (root == null) return;
>     List<TreeNode> res = new ArrayList<>();
>     preOrder(root, res);
>     int size = res.size();
>     for (int i = 1; i < size; i++) {
>         TreeNode pre = res.get(i - 1);
>         TreeNode cur = res.get(i);
>         pre.left = null;
>         pre.right = cur;
>     }
> }
> // 使用前序遍历保存节点的顺序
> public void preOrder(TreeNode root, List<TreeNode> res) {
>     if (root == null) return;
>     res.add(root);
>     preOrder(root.left, res);
>     preOrder(root.right, res);
> }
> ```

> _**解法 2 ⇒ 变异的后续遍历**_
>
> ```JavaScript
> var flatten = function(root) {
>   // 后续遍历是 左 => 右 => 根；变异的后续遍历是 右 => 左 => 根
>   // 使用变异的后续遍历遍历二叉树
>   // 揣摩该遍历方式的特点，并用一个指针记录 上一次遍历的节点
>   let pre = null
>   const order = (root) => {
>     if (!root) return
>     order(root.right)
>     order(root.left)
>     // 把当前节点的左孩子置空
>     root.left = null
>     // 把右子针指向上一个节点
>     root.right = pre
>     // 更新 pre
>     pre = root
>   }
>   order(root)
> };
> ```

> _**解法 3 ⇒ 拼接思想**_
>
> - 将左子树插入到右子树的地方，然后将原来的右子树接到左子树的最右边节点，考虑新的右子树的根节点，一直重复上边的过程，直到新的右子树为 _**null**_
> 
> ```Java
>     1
>    / \
>   2   5
>  / \   \
> 3   4   6
> 
> // 将 1 的左子树插入到右子树的地方
>     1
>      \
>       2         5
>      / \         \
>     3   4         6        
> //将原来的右子树接到左子树的最右边节点
>     1
>      \
>       2          
>      / \          
>     3   4  
>          \
>           5
>            \
>             6
>             
>  //将 2 的左子树插入到右子树的地方
>     1
>      \
>       2          
>        \          
>         3       4  
>                  \
>                   5
>                    \
>                     6   
>         
>  //将原来的右子树接到左子树的最右边节点
>     1
>      \
>       2          
>        \          
>         3      
>          \
>           4  
>            \
>             5
>              \
>               6         
>   
>   ......
> ```
>
> ```Java
> var flatten = function(root) {
>   // 在遍历过程中，每次找到 root 左子树的最右侧节点，记为 node
>   // 将 node.right 指向 root 的右子树
>   // 然后将 root.right = root.left，最后将 root.left = null
>   // 迭代 => root = root.right
>   while (root) {
>     // 如果左子树存在，则找到左子树的最右侧节点
>     if (root.left) {
>       // 声明一个变量，保存 root 的左子树节点
>       let node = root.left // 进入左子树
>       // 找到左子树的最右侧节点
>       while (node.right) node = node.right 
>       // 此时，node 指向左子树的最右侧节点
>       node.right = root.right
>       root.right = root.left
>       root.left = null
>     }
>     // 如果 root 没有左子树，直接迭代即可
>     // 迭代
>     root = root.right
>   }
> };
> ```

---

### _路径总和 III_

> _**问题描述**_
>
> > [!important] **[LeetCode - 路径总和 III](https://leetcode.cn/problems/path-sum-iii/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
> 
> ![[二叉树 - 2.png|Untitled 1 48.png]]

> _**前缀和 + HashMap + 回溯**_
>
> - 将左右子树中出现的路经总和递归加到 **root** 的 **count** 变量中，**count** 指路经总和出现的次数
> 
> - _**恢复状态的意义 ⇒**_ 当我们把一个节点的 前缀和 信息更新到 **map** 里时，它应当只对其子节点们有效。当我们遍历到最右方的节点 **6** 时，对于它来说，此时的前缀和为 **2** 的节点只该有 **B**。因为从 **A** 向下到不了节点 **6** ( **A** 并不是节点 **6** 的祖先节点 )。如果我们不做状态恢复，当遍历右子树时，左子树中 _**A**_ 的信息仍会保留在 **map** 中，那此时节点 **6** 就会认为 **A**, **B** 都是可追溯到的节点，从而产生错误。总结其作用为在遍历完一个节点的所有子节点后，将其从 **map** 中除去。
> 
> ```Java
> 	  	0
>      /  \
>     A:2  B:2
>    / \    \
>   4   5    6
>  / \   \
> 7   8   9
> ```
>
> ```JavaScript
> var pathSum = function(root, targetSum) {
>   // 使用 前缀和 思想和回溯算法，并使用 Map 关联 前缀和 和 出现的次数
>   let curSum = 0
>   let prefixMap = new Map()
>   prefixMap.set(0, 1) // 初始化，前缀和为 0，出现次数为 1
>   let count = 0
>   // 定义回溯算法
>   const findPath = (root) => {
>     if (!root) return
>     // 1. 尝试
>     // 记录当前的路径和，即 前缀和 + 当前值
>     curSum += root.val
>     // 若 prefixNow - prefixOld === targetSum 存在，则存在一个路径满足 targetSum
>     // 上述式子可以变换为 prefixNow - targetSum === prefixOld
>     // prefixOld 存在于之前的前缀和中，即 prefixMap 中
>     count += prefixMap.get(curSum - targetSum) || 0 // 若不存在，则赋值 0
>     // 将当前前缀和加入 prefixMap
>     prefixMap.set(curSum, (prefixMap.get(curSum) || 0) + 1)
>     // 2. 递归
>     findPath(root.left, curSum)
>     findPath(root.right, curSum)
>     // 3. 回退
>     prefixMap.set(curSum, (prefixMap.get(curSum)) - 1)
>     curSum -= root.val // 若 curSum 是回溯算法的局部变量，则不用将 curSum 回退
>   }
>   findPath(root)
>   return count
> };
> ```

---

### _**二叉树的最近公共祖先**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给定一个二叉树，找到该树中两个指定节点的最近公共祖先。百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大”。一个节点也可以是它自己的祖先。

> _**二叉树如何可以自底向上查找?**_
>
> - 使用回溯 ，二叉树回溯的过程就是从低到上。后序遍历就是天然的回溯过程，最先处理的一定是叶子节点。保证在回溯过程中，当前节点的左右孩子指向 **p, q** 或最近公共祖先或 **null。**

> _**如何判断一个节点是节点 q 和节点 p 的公共公共祖先？**_
>
> - _**情况 1 ⇒**_ 当节点 **p, q** 在节点 _**root**_ 的异侧时，节点 **root** 即为最近公共祖先，则向上返回 **root**
> 
> - _**情况 2 ⇒**_ `**p = root**` ，且 _**q**_ 在 _**root**_ 的左或右子树中
> 
> - _**情况 3 ⇒**_ ==`**q = root**`== ，且 _**p**_ 在 _**root**_ 的左或右子树中

> _**后序遍历（回溯）**_
>
> - _==**如果一个节点能够遍历到 p 或 q ，那么这个节点就可能是 最近公共祖先**==_
> 
> - 碰到图中的情况时，会直接返回 _**p**_，不会访问 _**q，**_即 _==**此方法有时不会访问整个树**==_
> 
> - 本次的设定是 给出的 _**p, q**_ 一定存在于树中，则若 _**3**_ 右子树返回 _**null**_，说明 _**p, q**_ 都存在于 _**3**_ 左子树中；且 p 为 _**5**_，则最近公共祖先一定是 _**p**_
> 
> - _**确定终止条件 ⇒**_ 如果遇到空节点，就返回 **null**；如果找到了 节点 **p** 或者 **q** _**，**_就返回 **root**
> 
> - _**处理返回值 ⇒ 1.**_ 如果 **left** 为空，说明所有 **p** 和 **q** 都不在左子树中，返回 **right**；_**2.**_ 如果 **right** 为空，说明所有 **p** 和 **q** 都不在右子树中，返回 **left；**_**3.**_ 如果 **left** 和 **right** 都不为空，说明 **p** 和 **q** 分别在当前节点的两侧，那么当前节点就是他们的最近公共祖先，返回当前节点。
> 
> ![[二叉树 - 4.png|Untitled 3 32.png]]
>
> ```Java
> var lowestCommonAncestor = function(root, p, q) {
>   // 使用后序遍历搜索 p , q，重点在于处理返回值
>   // root 的值有三种情况 => 1. null; 2. p; 3. p
>   // 1. 若为 null，说明当前搜索的（左或右）子树不包含 p,q
>   if (!root || root ===  p || root === q) return root
>   let left = lowestCommonAncestor(root.left, p, q)
>   let right = lowestCommonAncestor(root.right, p, q)
>   // 如果左子树返回 null，说明 p 或 q 存在于右子树中，直接返回右子树即可
>   if (!left) return right
>   // 如果右子树返回 null，说明 p 或 q 存在于左子树中，直接返回左子树即可
>   if (!right) return left
>   // 如果左右子树的返回值都不为空，说明当前节点是最近公共祖先
>   return root
> };
> ```

---

### 二叉树中的最大路径和

> _**问题描述**_
>
> > [!important] **[leetCode - 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 二叉树中的路径被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中至多出现一次 。该路径至少包含一个节点，且不一定经过根节点。路径和是路径中各节点值的总和。给你一个二叉树的根节点 `root` ，返回其最大路径和 。

> _**节点的最大贡献值**_
>
> - 对于二叉树中的任意一个节点而言，如果 以该节点为终点的路径 存在最大和，这个最大和称为该节点的“最大贡献值”。
> 
> - 以 当前节点 为终点的路径 意味着 只能从 左子树 或者 右子树 到达该节点
> 
> - 如果子节点的贡献值小于 0，则应当忽略该子节点的贡献值。即 `当前节点的值 > 当前节点的值 + 子节点的贡献值` 。
> 
> - `当前节点的最大贡献值 = curVal + Math.max(左孩子的最大贡献值，右孩子的最大贡献值)`

> _**最大路径和的计算**_
>
> - 最大路径和的产生分为 四种情况：
>     1. 该节点单独构成路径
>     2. 该节点加上 左子树的最大贡献值 构成路径
>     3. 该节点加上 右子树的最大贡献值 构成路径
>     4. 该节点加上 左右子树的最大贡献值 构成路径，即通过当前节点连接构成路径
> 
> - 如果 **左子树或者右子树的最大贡献值** 小于 0，则将其贡献值设置为 0，避免判断操作，即忽略小于 0 的贡献值。也可以理解为，不选择贡献值小于 0 的路径

> _**后序遍历求解**_
>
> ```JavaScript
> var maxPathSum = function(root) {
>   // 声明变量记录最大路径和
>   let maxPath = -Infinity
>   const afterOrder = (root) => {
>     if (!root) return 0
>     // 后序遍历，并记录当前节点的左右子树的最大贡献值，把小于 0 的贡献值设置为 0
>     // 左子树的最大贡献值
>     let leftGrain = Math.max(afterOrder(root.left), 0)
>     // 右子树的最大贡献值
>     let rightGrain = Math.max(afterOrder(root.right), 0)
>     // 计算最大路径和，并保存最大路径和
>     maxPath = Math.max(maxPath, leftGrain + rightGrain + root.val)
>     // 将当前节点的最大贡献值返回给父节点
>     return root.val + Math.max(leftGrain, rightGrain)
>   }
>   // 调用函数
>   afterOrder(root)
>   return maxPath
> };
> ```
