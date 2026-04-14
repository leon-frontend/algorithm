### _**合并两个有序链表**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

> _**使用迭代求解**_
>
> - 时间复杂度 **O(n+m)**，其中 _**n**_ 和 _**m**_ 分别为两个链表的长度。因为每次循环迭代中，**l1** 和 **l2** 只有一个元素会被放进合并链表中， 因此 **while** 循环的次数不会超过两个链表的长度之和。所有其他操作的时间复杂度都是常数级别的，因此总的时间复杂度为 **O(n+m)**
> 
> - 空间复杂度 **O(1)**。我们只需要常数的空间存放若干变量
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
>  * @param {ListNode} list1
>  * @param {ListNode} list2
>  * @return {ListNode}
>  */
> var mergeTwoLists = function(list1, list2) {
>   let dummy = new ListNode(-1)
>   let res = dummy, p1 = list1, p2 = list2
> 
>   while (p1 && p2) {
>     if (p1.val < p2.val) {
>       res.next = p1
>       p1 = p1.next
>     }else {
>       res.next = p2
>       p2 = p2.next
>     }
>     res = res.next
>   }
> 
>   res.next = p1 ? p1 : p2
>   return dummy.next
> };
> ```

> _**使用递归求解**_
>
> - 空间复杂度为 **O(m+n)**；时间复杂度 **O(m+n)**
> 
> ```Java
> public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
>     if (l1 == null) {
>         return l2;
>     }
>     else if (l2 == null) {
>         return l1;
>     }
>     else if (l1.val < l2.val) {
>         l1.next = mergeTwoLists(l1.next, l2);
>         return l1;
>     }
>     else {
>         l2.next = mergeTwoLists(l1, l2.next);
>         return l2;
>     }
> 
> }
> ```

---

### _**两数相加**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 两数相加](https://leetcode.cn/problems/add-two-numbers/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。请你将两个数相加，并以相同形式返回一个表示和的链表。你可以假设除了数字 0 之外，这两个数都不会以 0 开头。 

> _**解题思想**_
>
> - 注意 进位 和 最后一位进位 时的处理情况
> 
> - 在较短的链表后面补零，较短链表的数值不变，因为是逆序。因此，可以看成是 两个同样长度 的链表进行遍历
> 
> - 时间复杂度：_**O(max⁡(m,n))**_，其中 _**m**_ 和 _**n**_ 分别为两个链表的长度。我们要遍历两个链表的全部位置，而处理每个位置只需要 _**O(1)**_ 的时间
> 
> - 空间复杂度：_**O(1)。**_注意返回值不计入空间复杂度。
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
>  * @param {ListNode} l1
>  * @param {ListNode} l2
>  * @return {ListNode}
>  */
> var addTwoNumbers = function(l1, l2) {
>   // 情况 1 => 可能存在 l1, l2 不等长的情况，由于逆序存储，可以在较短的链表后面补 0
>   // 情况 2 => 可能会产生进位的情况，要记得处理最高位的进位情况
> 
>   // 声明返回结果
>   const dummy = new ListNode()
>   
>   // carry 表示进位情况
>   let p = dummy, p1 = l1, p2 = l2, carry = 0
> 
>   while (p1 || p2) {
>     // 获取 p1 , p2 的值，并处理情况 1
>     let v1 = p1 ? p1.val : 0
>     let v2 = p2 ? p2.val : 0
>     let sum = v1 + v2 + carry // 计算和
> 
>     // 由于每个节点只能存储 一位 数字，因为进行如下处理
>     // 注意 要先算 carry ，再算 sum
>     carry = Math.floor(sum / 10)
>     sum %= 10
>     
> 
>     // 创建新节点，加入结果链表
>     p.next = new ListNode(sum)
> 
>     // 迭代过程
>     p = p.next
>     if (p1) p1 = p1.next
>     if (p2) p2 = p2.next
>   }
> 
>   // 处理最高位的进位情况
>   if (carry) p.next = new ListNode(carry)
> 
>   return dummy.next
> };
> ```

---

### _**删除链表的倒数第 N 个结点**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。 

> _**双指针法**_
>
> - 在对链表进行操作时，一种常用的技巧是添加一个哑节点 ( **dummy node** )，它的 _**next**_ 指针指向链表的头节点，这样就不需要对第一个节点进行特殊的判断了。
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
>  * @param {number} n
>  * @return {ListNode}
>  */
> var removeNthFromEnd = function(head, n) {
>   // 声明空的头节点，指向 head => 这样一来，我们就不需要对头节点进行特殊的判断了
>   let dummy = new ListNode(-1, head)
>   // 使用双指针
>   let pre = dummy, p = dummy  
>   for (let i = 0; i < n; i++) p = p.next
>   // 同时遍历
>   // pre 最终会指向 倒数第 n+1 个节点
>   while (p && p.next) {
>     p = p.next
>     pre = pre.next
>   }
>   // 删除倒数第 n 个节点
>   pre.next = pre.next.next
>   return dummy.next
> };
> ```
