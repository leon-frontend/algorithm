### _**排序链表**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 排序链表](https://leetcode.cn/problems/sort-list/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。
> 

> _**自底向上归并排序**_
>
> - 插入排序 的时间复杂度是 $O(n^2)$，其中 _**n**_ 是链表的长度
> 
> - 题目的进阶问题要求达到 $O(nlog⁡n)$ 的时间复杂度和 O(1) 的空间复杂度
> 
> - 时间复杂度是 $O(nlog⁡n)$ 的排序算法包括 归并排序、堆排序和快速排序（快速排序的最差时间复杂度是$O(n^2)$)，其中==最适合链表的排序算法是归并排序==
>     
>     - **划分阶段**：可以使用“迭代”替代“递归”来实现链表划分工作，从而省去递归使用的栈帧空间。
>     
>     - **合并阶段**：在链表中，节点增删操作仅需改变引用（指针）即可实现，因此合并阶段（将两个短有序链表合并为一个长有序链表）无须创建额外链表。
>     
> 
> - 归并排序基于 分治算法
>     
>     - 最容易想到的实现方式是 自顶向下 的递归实现，考虑到递归调用的栈空间，自顶向下归并排序的空间复杂度是 $O(log⁡n)$
>     
>     - ==如果要达到 O(1) 的空间复杂度，则需要使用== ==自底向上== ==的实现方式==
>     
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
> var sortList = function(head) {
>   // 使用归并排序
>   // 划分阶段 => 使用迭代，使得空间复杂度为 O(1)
>   // 合并阶段 => 两个有序链表的合并
> 
>   if (!head || !head.next) return head
> 
>   // 定义 两个有序链表的合并 方法
>   const merge = (list1, list2) => {
>     let dummy = new ListNode(-1)
> 
>     let p1 = list1, p2 = list2, p = dummy
> 
>     while (p1 && p2) {
>       if (p1.val < p2.val) {
>         p.next = p1
>         p1 = p1.next // 迭代
>       }else {
>         p.next = p2
>         p2 = p2.next // 迭代
>       }
>       p = p.next // 迭代
>     }
> 
>     p.next =  p1 ? p1 : p2
> 
>     return dummy.next
>   }
> 
>   // -------------- 划分阶段 ----------------
>   // 划分环节本质上是通过二分法得到链表最小节点单元，再通过多轮合并得到排序结果
> 
>   let dummy = new ListNode(-1, head)
>   let lenp = head
>   let len = 0
> 
>   // 先计算链表的总长度
>   while (lenp) {
>     len++
>     lenp = lenp.next
>   }
> 
>   // 通过迭代划分，并使用 for 循环控制每次合并时的长度，即 subLen 的值
>   // subLen <<= 1 表示左移一位，即每次 乘以 2
>   for (let subLen = 1; subLen < len; subLen <<= 1) {
>     // pre 用于链接合并之后的链表，p 用于遍历合并之前的链表
>     let p = dummy.next, pre = dummy
>     while (p) {
>       // ----------------------- 循环的拆下两个需要合并的链表 -------------------------
>       // 声明第一个链表的头节点
>       let head1 = p
> 
>       // 找到第一个链表的尾节点
>       // 由于第一个链表的长度可能小于 subLen（比如最后一组链表中的第一个链表）
>       // 则在寻找尾节点时需要判断 p 和 p.next 是否为 null
>       // 如果 p 为空或者p.next为空（意味着没有足够的节点去构成第二个链表）
>       for (let i = 1; i < subLen && p && p.next; i++) p = p.next
> 
>       // 经过上面的循环，此时 p 指向第一个链表的尾节点
>       // 则可以通过 p 找到第二个链表的头节点
>       let head2 = p.next
> 
>       // 找到第二个链表的头节点后，将第一个链表从原链表上拆下来
>       p.next = null
> 
>       // 开始找第二个链表的尾节点
>       // 由于第二个链表的长度可能小于subLen，
>       // 则在寻找尾节点时需要判断 p 和 p.next 是否为 null
>       p = head2
>       for (let i = 1; i < subLen && p && p.next; i++) p = p.next
>       
>       // 经过上面的循环，此时 p 指向第二个链表的尾节点
>       // 声明一个指针，记录剩余链表的头节点
>       let next = null
>       // 如果 p 非空，next 记录剩余链表的头节点，如果为空，则没有剩余链表
>       if (p) {
>         next = p.next
>         // 将第二个链表从原链表上拆下来
>         p.next = null
>       }
>       // ----------------------- 循环的拆下两个需要合并的链表 -------------------------
> 
>       // 合并两个有序链表
>       let merged = merge(head1, head2)
> 
>       // 使用 pre 连接合并之后的链表
>       pre.next = merged
> 
>       // 找到合并后链表的尾节点，以连接下一次合并的结果
>       while (pre.next) pre = pre.next
> 
>       // 迭代
>       p = next
>     }
>   }
> 
>   return dummy.next
> };
> ```

---

### _**LRU 缓存**_

> _**问题描述**_
>
> > [!important] **[LeetCode - LRU 缓存](https://leetcode.cn/problems/lru-cache/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 请你设计并实现一个满足  [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。
> 
> - 实现 `LRUCache` 类：_**1.**_ `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存。_**2.**_ `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `1` 。_**3.**_ `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。
> 
> - 函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

> _**HashMap + 双向链表**_
>
> - 双向链表按照被使用的顺序存储了这些键值对，靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。
> 
> - 哈希表即为普通的哈希映射（_**HashMap**_），通过缓存数据的键映射到其在双向链表中的位置。==用 HashMap 建立 key 和节点之间的联系==
> 
> ```Java
> class LRUCache {
>     // 使用内部类定义双向链表
>     class DLinkedNode {
>         int key;
>         int value;
>         DLinkedNode prev;
>         DLinkedNode next;
>         public DLinkedNode() {}
>         public DLinkedNode(int _key, int _value) {
>             key = _key; 
>             value = _value;
>         }
>     }
> 
>     private Map<Integer, DLinkedNode> cache = new HashMap<>();
>     private int size;
>     private int capacity;
>     private DLinkedNode head, tail;
> 
>     public LRUCache(int capacity) {
>         this.size = 0;
>         this.capacity = capacity;
>         // 使用伪头部和伪尾部节点
>         head = new DLinkedNode();
>         tail = new DLinkedNode();
>         head.next = tail;
>         tail.prev = head;
>     }
>     
>     public int get(int key) {
>         DLinkedNode node = cache.get(key);
>         if (node == null) return -1;
>         // 若 key 存在，则移到头部
>         moveToHead(node);
>         return node.value;
>     }
> 
>     public void put(int key, int value) {
>         DLinkedNode node = cache.get(key);
>         if (node == null) {
>             // 如果 key 不存在，创建一个新的节点
>             DLinkedNode newNode = new DLinkedNode(key, value);
>             // 添加进哈希表
>             cache.put(key, newNode);
>             // 添加至双向链表的头部
>             addToHead(newNode);
>             ++size;
>             if (size > capacity) {
>                 // 如果超出容量，删除双向链表的尾部节点
>                 DLinkedNode tail = removeTail();
>                 // 删除哈希表中对应的项
>                 cache.remove(tail.key);
>                 --size;
>             }
>         }
>         else {
>             // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
>             node.value = value;
>             moveToHead(node);
>         }
>     }
> 
> 		// 将某个节点添加到双向链表头部
>     private void addToHead(DLinkedNode node) {
>         node.prev = head;
>         node.next = head.next;
>         head.next.prev = node;
>         head.next = node;
>     }
> 
> 		// 移除某个节点
>     private void removeNode(DLinkedNode node) {
>         node.prev.next = node.next;
>         node.next.prev = node.prev;
>     }
> 
> 		// 将某个节点移动到链表头部
>     private void moveToHead(DLinkedNode node) {
>         removeNode(node);
>         addToHead(node);
>     }
> 
> 		// 移除尾节点
>     private DLinkedNode removeTail() {
>         DLinkedNode res = tail.prev;
>         removeNode(res);
>         return res;
>     }
> }
> ```
