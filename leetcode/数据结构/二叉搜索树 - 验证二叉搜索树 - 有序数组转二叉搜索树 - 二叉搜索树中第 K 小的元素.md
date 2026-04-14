### _**二叉搜索树**_

> _**二叉搜索树（二叉排序树）**_
>
> - 二叉搜索树的中序遍历是升序序列
> 
> - 给定二叉搜索树的中序遍历，是否可以唯一地确定二叉搜索树？答案是 否定 的。如果没有要求二叉搜索树的高度平衡，则任何一个数字都可以作为二叉搜索树的根节点，因此可能的二叉搜索树有多个。
> 
> - 增加一个限制条件，即要求二叉搜索树的高度平衡，是否可以 唯一 地确定二叉搜索树？答案仍然是 否定 的。

---

### _**验证二叉搜索树**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。有效二叉搜索树定义如下：节点的左子树只包含 **小于** 当前节点的数。节点的右子树只包含 **大于** 当前节点的数。所有左子树和右子树自身必须也是二叉搜索树。 

> _**解法 1 ⇒ 递归**_
>
> - 设计一个递归函数 _**helper(root, lower, upper)**_ 来递归判断。函数表示考虑以 _**root**_ 为根的子树，判断子树中所有节点的值是否都在 `**(l,r)**` 的范围内（注意是开区间）。如果 _**root**_ 节点的值 _**val**_ 不在 `**(l,r)**` 的范围内说明 不满足 条件直接返回，否则我们要继续递归调用检查它的左右子树是否满足，如果都满足才说明这是一棵二叉搜索树
> 
> ```Java
> var isValidBST = function(root) {
>   const isBST = (root, minimum, maximum) => {
>     // 此时遍历完了最有一个节点，说明之前的节点都是成立的
>     if (!root) return true
>     if (root.val >= maximum || root.val <= minimum) return false
>     // 只有在符合二叉搜索树的条件时，进行递归
>     return isBST(root.left, minimum, root.val) && isBST(root.right, root.val, maximum)
>   } 
>   let minimum = -Infinity, maximum = Infinity
>   return isBST(root, minimum, maximum)
> };
> ```

> _**解法 2 ⇒ 使用栈实现**_
>
> - 二叉搜索树的中序遍历是升序序列。检查当前节点的值是否大于前一个中序遍历到的节点的值即可。如果均大于说明这个序列是升序的，整棵树是二叉搜索树，否则不是。
> 
> ```Java
> public boolean isValidBST(TreeNode root) {
>     Deque<TreeNode> stack = new LinkedList<>();
>     long pre = Long.MIN_VALUE;
>     // 中序遍历
>     while (!stack.isEmpty() || root != null) {
>         // 访问左子树，找到最左下角的节点, 并将经过的节点全部压入 stack
>         while (root != null) {
>             stack.push(root);
>             root = root.left;
>         }
>         // 将最后一个元素出栈
>         root = stack.pop();
>         // 如果中序遍历得到的节点的值小于等于前一个 pre，说明不是二叉搜索树
>         if (root.val <= pre) return false;
>         // 更新 pre 的值
>         pre = root.val;
>         // 访问右子树
>         root = root.right;
>     }
>     return true;
> }
> ```

---

### _**将有序数组转换为二叉搜索树**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给一个整数数组 `nums` ，其中元素已经按升序排列，请你将其转换为一棵高度平衡二叉搜索树。**高度平衡**二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。 

> _**解法 ⇒ 总是选择中间位置左边的数字作为根节点**_
>
> - 选择中间数字作为二叉搜索树的根节点。这样分给左右子树的数字个数相同或只相差 **1**，可以使得树保持平衡。如果数组长度是奇数，则根节点的选择是唯一的。如果数组长度是偶数，则可以选择中间位置左边的数字作为根节点或者选择中间位置右边的数字作为根节点。选择不同的数字作为根节点则创建的平衡二叉搜索树也是不同的。
> 
> - 确定平衡二叉搜索树的根节点之后，其余的数字分别位于平衡二叉搜索树的左子树和右子树中。左子树和右子树分别也是平衡二叉搜索树。因此可以通过递归的方式创建平衡二叉搜索树。
> 
> ```Java
> var sortedArrayToBST = function(nums) {
>   if (!nums.length) return null
>   // 总是选择数组中间的元素作为根节点
>   const build = (nums, left, right) => {
>     if (left > right) return null
>     // 找到数组中间的元素作为根节点
>     let mid = Math.floor(left + (right - left) / 2)
>     // 创建根节点
>     let root = new TreeNode(nums[mid])
>     // 递归
>     root.left = build(nums, left, mid - 1)
>     root.right = build(nums, mid + 1, right)
>     return root
>   }
>   return build(nums, 0, nums.length - 1)
> };
> ```

---

### _**二叉搜索树中第 K 小的元素**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 个最小元素（从 1 开始计数）。 

> _**解法 1 ⇒ 递归**_
>
> ```JavaScript
> var kthSmallest = function(root, k) {
>   // 中序遍历二叉搜索树会得到一个升序列表
>   // 执行中序遍历，并用一个变量记录，此时访问到了第几个节点
>   let count = 0, res
>   // 中序遍历
>   const inOder = (root) => {
>     if (!root) return
>     inOder(root.left)
>     // 处理当前元素
>     if (++count === k) {
>       res = root.val
>       return 
>     }
>     inOder(root.right)
>   }
>   inOder(root) // 调用函数
>   return res
> };
> ```

> _**解法 2 ⇒ 使用栈实现递归**_
>
> - 因为 **stack** 的出栈顺序是 升序，则执行 **k** 次出栈操作，就是第 **k** 小的元素出栈
> 
> ```JavaScript
> var kthSmallest = function(root, k) {
>   // 中序遍历二叉搜索树会得到一个升序列表
>   // 使用栈模拟中序遍历
>   const stack = []
>   // 若 root 为空，但 stack 不空，则出栈
>   while (root || stack.length) {
>     while (root) {
>       stack.push(root) // 入栈
>       root = root.left // 优先访问左孩子节点，实现中序遍历
>     }
>     // 此时左孩子访问完了，stack出栈，模拟访问元素
>     root = stack.pop()
>     // 先将 k 减一，再判断是否为 0，若为 0，则 root 指向第 k 小的元素
>     if (!(--k)) return root.val
>     // 若 root 不是指向第 k 小的元素，则继续中序遍历，遍历右孩子节点
>     root = root.right
>   }
> };
> ```
