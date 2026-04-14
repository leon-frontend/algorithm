### _**两两交换链表中的节点**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。 

> _**使用三个指针解题，注意 迭代 条件**_
>
> ```Java
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val, next) {
>  *     this.val = (val===undefined ? 0 : val)
>  *     this.next = (next===undefined ? null : next)
>  * }
>  */
> /**
>  * @param {ListNode} head
>  * @return {ListNode}
>  */
> var swapPairs = function(head) {
>   if (!head || !head.next) return head
> 
>   // 声明空的头节点
>   let dummy = new ListNode(-1, head)
> 
>   // 声明两个指针
>   let pre = dummy, p = dummy.next
> 
>   while (p && p.next) {
>     // 两辆交换
>     pre.next = p.next
>     p.next = p.next.next
>     pre.next.next = p
> 
>     // 迭代
>     pre = p
>     p = p.next
>   }
> 
>   return dummy.next
> };
> ```

---

### _**K 个一组翻转链表**_

> _**问题描述**_
>
> > [!important] **[LeetCode - K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。
> 
> - `k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。
> 
> - 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。 
>
> ```Java
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val, next) {
>  *     this.val = (val===undefined ? 0 : val)
>  *     this.next = (next===undefined ? null : next)
>  * }
>  */
> /**
>  * @param {ListNode} head
>  * @param {number} k
>  * @return {ListNode}
>  */
> var reverseKGroup = function(head, k) {
>   // 定义反转链表函数
>   const reverse = (head) => {
>     if (!head || !head.next) return head
> 
>     let pre = null, p = head
> 
>     while (p) {
>       let next = p.next
>       p.next = pre
> 
>       // 迭代
>       pre = p
>       p = next
>     }
>     
>     return pre
>   }
> 
>   // 声明空的头节点，并指向 head
>   let dummy = new ListNode(-1, head)
> 
>   // startPre 指向每 k 个节点一组的开始的前一个节点
>   // end 指向每 k 个节点一组的结尾节点
>   let startPre = dummy, end = dummy
> 
>   // 用 flag 标记该组节点是否需要反转链表
>   let flag = false
> 
>   while (end) {
>     // 遍历 end，如果 end 为空，说明从 end 开始节点数少于 k，该组节点不反转
>     for (let i = 0; i < k; i++) {
>       end = end.next
>       if (!end) {
>         flag = true
>         break
>       }
>     }
>     
>     if (flag) break
> 
>     // 保存 end 的后一个节点，并将 end.next = null
>     let endNext = end.next
>     end.next = null
> 
>     // 开始反转 k 个节点
>     startPre.next = reverse(startPre.next)
>     // 找到该组链表的结尾
>     for (let i = 0; i < k; i++) startPre = startPre.next
>     // 将反转后的该组链表和剩余未反转的链表连接
>     startPre.next = endNext
> 
>     // 迭代
>     end = startPre
>   }
> 
>   return dummy.next
> };
> ```

---

### _**随机链表的复制**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。构造这个链表的 **深拷贝**。 
> 
> - 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。**复制链表中的指针都不应指向原链表中的节点** 。
> 
> - 例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。返回复制链表的头节点。
> 
> - 用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：
>     
>     - `val`：一个表示 `Node.val` 的整数。
>     
>     - `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。
>     
> 
> - 你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。 

> _**方法 1 ⇒ 使用 HashMap 关联 原链表 中的节点和 新链表 中的对应节点**_
>
> - _**时间复杂度 O(N) 。**_两轮遍历链表，使用 _**O(N)**_ 时间
> 
> - _**空间复杂度 O(N)。**_哈希表 _**dic**_ 使用线性大小的额外空间
> 
> ```Java
> /**
>  * // Definition for a Node.
>  * function Node(val, next, random) {
>  *    this.val = val;
>  *    this.next = next;
>  *    this.random = random;
>  * };
>  */
> 
> /**
>  * @param {Node} head
>  * @return {Node}
>  */
> var copyRandomList = function(head) {
>   if (!head) return null
> 
>   // 使用 Map 存储随机节点的指向
>   let map = new Map()
> 
>   // 声明两个指针，同时遍历新链表和旧链表
>   let p = head
> 
>   // 先遍历一边 head
>   // 只构建源节点和新节点的对应关系，不构建 next 和 random
>   while (p) {
>     map.set(p, new Node(p.val))
>     p = p.next
>   }
> 
>   // 第二次遍历 head，构建 next 和 random
>   p = head
>   while (p) {
>     if (p.next) {
>       map.get(p).next = map.get(p.next) // 设置新节点的next指针
>     }
>     if (p.random) {
>       map.get(p).random = map.get(p.random) // 设置新节点的random指针
>     }
>     p = p.next
>   }
> 
>   return map.get(head)
> };
> ```

> _**方法 2 ⇒ 拼接 + 拆分思想**_
>
> - 复制各节点，构建拼接链表：设原链表为 node1→node2→⋯，构建的拼接链表如下所
>     
>     ![[链表 - 4.png|Untitled 3 29.png]]
>     
> 
> - 拆分原 / 新链表
> 
> - 返回新链表的头节点 `res` 即可
> 
> - 时间复杂度 _**O(N)。**_三轮遍历链表，使用 O(N) 时间
> 
> - 空间复杂度 _**O(1)。**_节点引用变量使用常数大小的额外空间
> 
> ```Java
> public Node copyRandomList(Node head) {
>     if (head == null) return null;
> 
>     Node cur = head;
> 
>     // 拼接链表
>     while (cur != null) {
>         Node tmp = new Node(cur.val);
>         tmp.next = cur.next;
>         cur.next = tmp;
>         cur = tmp.next;
>     }
> 
>     cur = head;
>     // 复制 random 指针
>     while (cur != null) {
>         if (cur.random != null) cur.next.random = cur.random.next;
>         cur =  cur.next.next;
>     } 
> 
>     // 拆分两个链表
>     Node pre = head;
>     cur = head.next;
>     Node res = head.next;
>     while (cur.next != null) {
>         pre.next = pre.next.next;
>         cur.next = cur.next.next;
>         pre = pre.next;
>         cur = cur.next;
>     }
> 
>     pre.next = null; // 单独处理原链表尾节点
>     
>     return res;
> }
> ```
