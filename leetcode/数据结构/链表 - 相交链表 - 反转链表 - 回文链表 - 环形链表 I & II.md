### _**相交链表**_

> _**题目描述**_
>
> > [!important] **[LeetCode - 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。
> 
> - 图示两个链表在节点 `c1` 开始相交
>     
> ![[链表 - 9.png|Untitled 68.png]]
>
>
>

> _**解题**_
>
> - 如果两个链表相交，那么相交点之后的长度是相同的
> 
> - 我们需要做的事情是，让两个链表从 同距离末尾同等距离 的位置开始遍历
> 
> - 这个位置只能是较短链表的头结点位置。 为此，我们必须消除两个链表的长度差
>     
>     - 指针 _**pA**_ 指向 _**A**_ 链表，指针 _**pB**_ 指向 _**B**_ 链表，依次往后遍历
>     
>     - 如果 _**pA**_ 到了末尾，则 `**pA = headB**` 继续遍历
>     
>     - 如果 _**pB**_ 到了末尾，则 `**pB = headA**` 继续遍历
>     
>     - 比较长的链表指针指向较短链表 _**head**_ 时，长度差就消除了
>     
>     - 如此，只需要将最短链表遍历两次即可找到位置
>     
> 
> - 若不相交，则会在某一个时间点同时指向 _**NULL**_ ，并返回 _**NULL**_
> 
> ```Java
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
> 
> /**
>  * @param {ListNode} headA
>  * @param {ListNode} headB
>  * @return {ListNode}
>  */
> var getIntersectionNode = function(headA, headB) {
>   // 定义两个指针pa,pb，同时遍历两个链表
>   // pa 如果遍历完 a 链表，则立刻从 b 链表的表头开始遍历 b 链表
>   // pb 如果遍历完 b 链表，则立刻从 a 链表的表头开始遍历 a 链表
>   // 直到 pa 和 pb 指向的节点相同，停止循环
>   // 若不相交，则会在某一个时间点同时指向 NULL ，并返回 NULL
> 
>   if (headA === null || headB === null) return null
>   let pa = headA, pb = headB
>   while (pa != pb) {
>     pa = pa === null ? headB : pa.next
>     pb = pb === null ? headA : pb.next
>   }
> 
>   return pa
> };
> ```

---

### _**反转链表**_

> _**题目描述**_
>
> > [!important] **[LeetCode - 反转链表](https://leetcode.cn/problems/reverse-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

> _**方法 1 ⇒ 迭代**_
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
> var reverseList = function(head) {
>   if (head === null || head.next === null) return head
> 
>   // 声明两个指针，一个指针指向当前遍历的节点，另一个指向前一个节点
>   let pre = null, p = head
> 
>   while (p != null) {
>     let next = p.next
>     p.next = pre
>     pre = p
>     p = next
>   }
> 
>   return pre
> };
> ```

> _**方法 2 ⇒ 递归**_
>
> - 时间复杂度：_**O(n)**_，其中 _**n**_ 是链表的长度，需要对链表的每个节点进行反转操作
> 
> - 空间复杂度：_**O(n)**_，其中 _**n**_ 是链表的长度，空间复杂度主要取决于递归调用的栈空间，最多为 _**n**_ 层
> 
> ```Java
> public ListNode reverseList(ListNode head) {
>     if (head == null || head.next == null) return head;
>     // 实现递归
>     ListNode newHead = reverseList(head.next);
>     head.next.next = head;
>     head.next = null;
>     // 将反转后的链表的头节点返回给调用者
>     return newHead;
> }
> ```

---

### _回文链表_

> _**问题描述**_
>
> - 给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

> _**快慢指针思想**_
>
> - 慢指针一次走一步，快指针一次走两步，快慢指针同时出发。
> 
> - 当快指针移动到链表的末尾时，慢指针恰好到链表的中间。
> 
> - 通过慢指针将链表分为两部分
> 
> - _**整个流程可以分为以下五个步骤 ⇒ 1.**_ 找到前半部分链表的尾节点。_**2.**_ 反转后半部分链表。_**3.**_ 判断是否回文。_**4.**_ 恢复链表。_**5.**_ 返回结果。
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
>  * @return {boolean}
>  */
> var isPalindrome = function(head) {
>   // 1. 使用快慢指针找到前半段的最后一个节点
> 	// 2. 反转后半段
>   // 3. 同时遍历前半段和后半段，比较值
>   
>   // 1. 使用快慢指针找到前半段的最后一个节点
>   const findfirstHalfLast = (head) => {
>     let slow = head, fast = head // 快慢指针
> 
>     // fast.next !== null 用于只有一个元素的情况
>     while (fast.next != null && fast.next.next !== null) {
>       fast = fast.next.next
>       slow = slow.next
>     }
> 
>     // 此时 slow 指向前半段链表的最后一个节点
>     // 若节点数是奇数，则指向最中间的节点
> 		/*
> 	    偶數: 1 -> 2 -> 2 -> 1 -> null
> 	             slow  fast 
> 	    奇數: 1 -> 2 -> 3 -> 2 -> 1 -> null
> 	                   slow      fast      
> 	  */
>     return slow
>   }
> 
>   // 2. 反转后半段
>   const reverse = (head) => {
>     if (head === null || head.next === null) return head
> 
>     let pre = null, p = head
>     while (p !== null) {
>       let next = p.next
>       p.next = pre
>       pre = p
>       p = next
>     }
> 
>     return pre
>   }
> 
>   // 3. 同时遍历前半段和后半段，比较值
>   // 找到前半段的最后一个节点
>   let firstHalfEnd = findfirstHalfLast(head)
> 
>   // 反转后半段
>   let secondHalfStart = reverse(firstHalfEnd.next)
> 
>   // 声明两个指针，同时遍历前半段和后半段链表
>   let p1 = head, p2 = secondHalfStart
>   while (p2 !== null) {
>     if (p2.val !== p1.val) return false
>     p2 = p2.next
>     p1 = p1.next
>   }
> 
>   return true
> };
> ```

---

### _**环形链表**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 环形链表](https://leetcode.cn/problems/linked-list-cycle/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个链表的头节点 `head` ，判断链表中是否有环。
> 
> - 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：**`**pos**` **不作为参数进行传递** 。仅仅是为了标识链表的实际情况。
> 
> - _如果链表中存在环_ ，则返回 `true` 。 否则，返回 `false` 。

> _**快慢指针思想**_
>
> - 当一个链表有环时，快慢指针都会陷入环中进行无限次移动，然后变成了追及问题
> 
> - 想象一下在操场跑步的场景，只要一直跑下去，快的总会追上慢的
> 
> - 当两个指针都进入环后，每轮移动使得慢指针到快指针的距离增加一，同时快指针到慢指针的距离也减少一，只要一直移动下去，快指针总会追上慢指针
> 
> ```Java
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
> 
> /**
>  * @param {ListNode} head
>  * @return {boolean}
>  */
> var hasCycle = function(head) {
>   // 声明快慢指针，一直遍历链表，若相遇则存在环
>   // 若不存在环，则快慢指针都会指向 null
>   if (head === null || head.next === null) return false
> 
>   let fast = head, slow = head // 声明快慢指针
> 
>   do {
>     if (!fast || !fast.next) return false
>     fast = fast.next.next
>     slow = slow.next
>   }while (fast !== slow)
> 
>   return true
> };
> ```

---

### _**环形链表 II**_

> _**问题描述**_
>
> - 给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 _如果链表无环，则返回 `null`。_
> 
> - 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：**`**pos**` **不作为参数进行传递**，仅仅是为了标识链表的实际情况。
> 
> - **不允许修改** 链表。

> _**双指针的第一次相遇**_
>
> - 设两指针 _**fast**_，_**slow**_ 指向链表头部 _**head ，**_令 _**fast**_ 每轮走 _**2**_ 步，_**slow**_ 每轮走 _**1**_ 步
> 
> - 执行以上两步后，可能出现两种结果 ⇒ _**1.**_ 第一种结果：_**fast**_ 指针走过链表末端，说明链表无环，此时直接返回 _**null**_ 。_**2.**_ 第二种结果： 当`**fast === slow**`时， 表示链表存在环，则双指针一定会进行第一次相遇。因为每走 _**1**_ 轮，_**fast**_ 与 _**slow**_ 的间距 _**+1**_，_**fast**_ 一定会追上 _**slow**_

> _**分析此时 fast 与 slow 走过的步数关系**_
>
> - 设链表共有 _**a+b**_ 个节点，其中 链表头部到链表入口 有 _**a**_ 个节点（不计链表入口节点）， 链表环 有 _**b**_ 个节点
>     
>     - 这里需要注意，_**a**_ 和 _**b**_ 是未知数，例如图解上链表 a=4 , b=5
>     
> 
> - 设两指针分别走了 _**f，s**_ 步，则有
>     
>     - _**fast**_ 走的步数是 _**slow**_ 步数的 _**2**_ 倍，即 ==`**f = 2s**`==
>         
>         - 解析：_**fast**_ 每轮走 _**2**_ 步
>         
>     
>     - _**fast**_ 比 _**slow**_ 多走了 _**n**_ 个环的长度，即 `**f = s + nb**`
>         
>         - 双指针都走过 _**a**_ 步，然后在环内绕圈直到重合，重合时 _**fast**_ 比 _**slow**_ 多走 环的长度整数倍
>         
>     
>     - 以上两式相减得到`**f = 2nb，s = nb**`，即 _**fast**_ 和 _**slow**_ 指针分别走了 _**2n，n**_ 个环的周长
>     

> _**通过**`**s = nb**`**得到链表入口节点的位置**_
>
> - 如果让指针从链表头部一直向前走并统计步数 _**k**_，那么所有 走到链表入口节点时的步数 是：==`**k = a + nb**`==
>     
>     - 即先走 _**a**_ 步到入口节点，之后每绕 _**1**_ 圈环（ _**b**_ 步）都会再次到入口节点
>     
>     - 而目前 _**slow**_ 指针走了 _**nb**_ 步，则 _**slow**_ 指针再走 _**a**_ 步就会到达入口节点
>     
> 
> - 因此，我们只要想办法让 _**slow**_ 再走 _**a**_ 步停下来，就可以到环的入口
>     
>     - 由于， _**head**_ 走 _**a**_ 步也会到达入口节点
>     
>     - 虑构建一个指针，此指针和 _**slow**_ 一起向前走 _**a**_ 步后，两者在入口节点重合
>     

> _**双指针第二次相遇**_
>
> - 令 _**fast**_ 重新指向链表头部节点。此时`**f = 0，s = nb**`
> 
> - _**slow**_ 和 _**fast**_ 同时每轮向前走 _**1**_ 步
> 
> - 当 _**fast**_ 指针走到 ==`**f = a**`== 步时，_**slow**_ 指针走到 ==`**s = a + nb**`== 步。
>     
>     - 此时两指针重合，并同时指向链表环入口，返回 _**slow**_ 指向的节点即可
>     

> _**代码实现**_
>
> - 时间复杂度 _**O(N) ⇒**_ 第二次相遇中，慢指针须走步数 `**a < a + b**`。第一次相遇中，慢指针须走步数 ==`**a + b − x < a + b**`==，其中 _**x**_ 为双指针重合点与环入口距离。因此总体为线性复杂度
> 
> - 空间复杂度 _**O(1) ⇒**_ 双指针使用常数大小的额外空间
> 
> ```Java
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
> 
> /**
>  * @param {ListNode} head
>  * @return {ListNode}
>  */
> var detectCycle = function(head) {
>   if (!head || !head.next) return null
> 
>   // 声明快慢指针
>   let fast = head, slow = head
> 
>   // 若链表中有环，则快慢指针第一次相遇
>   do {
>     if (!fast || !fast.next) return null
>     fast = fast.next.next
>     slow = slow.next
>   }while (fast !== slow)
> 
>   // fast = head，让 fast 和 slow 每次都只前进一步
>   fast = head
>   while (fast !== slow) {
>     slow = slow.next
>     fast = fast.next
>   }
> 
>   return slow
> };
> ```
