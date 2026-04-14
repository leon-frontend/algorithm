### _**和为 K 的子数组**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 该数组中和为 `k` 的连续子数组的个数 。子数组是数组中元素的连续非空序列。

> **解法 1 ⇒ 使用前缀和数组**
>
> - `**前缀和数组的长度 = 存放数据的数组长度 + 1**`，因此要注意下标的偏移
> 
> - 重点在于如何求 **left** 到 **right** 之间的和 =>
>     
>     - **left** 到 **right** 之间的和 = **right + 1** 的前缀和 - **left** 的前缀和
>     
>     - **left** 到 **right** 之间的和 = **0** 到 **right** 的和 - **0** 到 **left - 1** 的和
>     
> 
> - 时间复杂度：$O(N^2)$ ，这里 _N_ 是数组的长度；空间复杂度：$O(1)$
> 
> ```JavaScript
> /**
>  * @param {number[]} nums
>  * @param {number} k
>  * @return {number}
>  */
> var subarraySum = function(nums, k) {
>   // 声明返回结果
>   let count = 0
>   // 使用 前缀和 数组
> 	// preSum[i] 对应 前 i 个元素的前缀和
> 	// preSum[i] 对应 nums 中 0 ~ i-1 个元素的和
>   const preSum = Array(nums.length + 1)
>   // 前缀和 数组初始化
>   preSum[0] = 0
>   // 遍历更新 前缀和 数组 => preSum[i] 对应 前 i 个元素的和
>   for (let i = 0; i < nums.length; i++) {
>     preSum[i + 1] = preSum[i] + nums[i]
>   }
>   // 遍历 nums 数组 => 求 区间 [left...right] 的和
>   for (let left = 0; left < nums.length; left++) {
>     for (let right = left; right < nums.length; right++) {
>       // nums 中 0 ~ 2 的和 为 preSum[3]
>       // left 到 right 之间的和 = 0 到 right 的和 - 0 到 left - 1 的和
>       // => preSum[right + 1] - preSum[left]
>       if (preSum[right + 1] - preSum[left] === k) count++
>     }
>   }
>   return count
> };
> ```

> **解法 2 ⇒ 前缀和 + 哈希表优化**
>
> - 由于只关心次数，不关心具体的解，我们可以使用哈希表加速运算
> 
> - ==**和两数之和的思想类似 ⇒**== 两数之和为`**a+b=k**`，遍历数组，若 **a** 已加入哈希表，对于 **b**，看`**k-b**`是不是在哈希表中。而在本题中问题转化为两数之差，为 `**a-b=k**`；并引出了前缀和的思想
> 
> - ==**重点 1 ⇒**== ==知道区间下界，通过前缀和确定====区间上界==
> 
> - ==**重点 2 ⇒**== 数组元素可能存在 负数，所以相同的前缀和可能存在多次
> 
> - 时间复杂度 **O(n)**，这里 N 是数组的长度；空间复杂度 **O(n)**
> 
> ```Java
> /**
>  * @param {number[]} nums
>  * @param {number} k
>  * @return {number}
>  */
> var subarraySum = function(nums, k) {
>   // 声明返回结果
>   let count = 0
>   // 创建 Map ，Key 用于存储前缀和，Value 用于存储前缀和出现的次数
>   const map = new Map()
>   // 初始化 map => 对于数组中的第一个元素，其前缀和为 0，出现次数为 1
>   map.set(0, 1)
>   let preSum = 0; // 记录前缀和
>   nums.forEach(num => {
>     // 从第一个元素开始记录从 0 到 num 的和 ，这个和就是下个元素的前缀和
>     preSum += num
>     // k === 0 ~ right 的和 - 0 ~ left - 1 的和
>     // preSum 记录了 0 ~ right 的和 => 0 ~ right 的和 - k === 0 ~ left - 1 的和
>     // 即要找 0 ~ left - 1 的和
>     if (map.has(preSum - k)) count += map.get(preSum - k)
>     map.set(preSum, (map.get(preSum) || 0) + 1)
>   })
>   return count
> };
> ```

---

### _**滑动窗口最大值**_

> _**问题描述**_
>
> > [!important] **[LeetCode -滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。返回滑动窗口中的最大值 。

> _**使用单调递减队列**_
>
> - _**情况 1 ⇒**_ 滑动窗口在移动过程中，丢失了最左侧的最大值
> 
> - _**情况 2 ⇒**_ 滑动窗口在移动过程中，最右侧的新值是最大值
> 
> - _**重点 ⇒**_ 要根据最右侧的新值不断维护单调递减的队列
> 
> ```JavaScript
> var maxSlidingWindow = function(nums, k) {
>   // 使用单调队列存储数组中的索引
>   const res = [], queue = []
>   for (let i = 0; i < nums.length; i++) {
>     // 1. 如果单调队列非空，且单调队列中的最大值已丢失（上一次最左边的值）
>     // i-k 表示窗口最左侧的索引，queue[0]是最大值的索引
>     if (queue.length && queue[0] <= i - k) queue.shift()
>     // 2. 每个元素是在队尾入队，需要实时维护单调队列的单调递减特性 => 将比当前元素小的元素出队
>     while (queue.length && nums[queue[queue.length - 1]] < nums[i]) queue.pop()
>     queue.push(i) // 队尾入队
>     if (i >= k - 1) res.push(nums[queue[0]])
>   }
>   return res
> };
> ```
