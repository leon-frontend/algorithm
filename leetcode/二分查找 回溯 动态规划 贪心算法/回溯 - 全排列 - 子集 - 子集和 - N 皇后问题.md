### _**全排列**_

> _**Reference**_
>
> > [!important] **[Hello 算法 - 全排列问题](https://www.hello-algo.com/chapter_backtracking/permutations_problem/)**

> _**全排列情况 1 ⇒ 给定数组无重复元素**_
>
> > [!important] **[LeetCode - 无重复元素的全排列](https://leetcode.cn/problems/permutations/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 候选集合 **choices** 是输入数组中的所有元素（无重复元素）。状态 **state** 是直至目前已被选择的元素。==每个元素只允许被选择一次，因此== ==**state**== ==中的所有元素都应该是唯一的。==
> 
> - _**剪枝( 避免重复操作 ) ⇒**_ 引入一个布尔型数组 **selected**，其中`selected[i]`表示`choices[i]`是否已被选择，并基于它实现以下剪枝操作。在做出选择 **choice[i]** 后，就将 **selected[i]** 赋值为 **true** ，代表它已被选择。遍历选择列表 **choices** 时跳过所有已被选择的节点，即==剪枝==
> 
> ![[回溯 - 1.png|Untitled 83.png]]
>
> ```JavaScript
> var permute = function(nums) {
>   const res = []
>   backtrack([], nums, Array(nums.length).fill(false), res)
>   return res
> };
> 
> // state 表示问题的当前状态, {number[]}
> // choices 表示当前状态下可以做出的选择, {number[]}
> // selected[i] 表示 choices[i] 是否已被选择, {number[]}
> // res 表示结果数组
> function backtrack(state, choices, selected, res) {
>   // 当 当前状态的长度 等于 元素数量的长度 时，记录解
>   if (state.length === choices.length) {
> 	  // 不能使用 res.push(state)
>     // 这样会传递 state 的引用地址，state 改变时，res 中的 state 也会改变
>     res.push([...state])
> 	  return // 不再继续搜索
>   }
>   // 遍历所有选择
>   choices.forEach((choice, i) => {
>     // 通过 selected 数组进行 剪枝
>     if (!selected[i]) {
>       // 尝试：做出选择，更新状态
>       selected[i] = true
>       state.push(choice)
>       // 递归：进行下一轮选择
>       backtrack(state, choices, selected, res)
>       // 回退：撤销本次递归层的选择，恢复到之前的状态
>       selected[i] = false
>       state.pop()
> 	  }
>   })
> }
> ```

> _**全排列情况 2 ⇒ 给定数组存在重复元素**_
>
> > [!important] **[LeetCode - 存在重复元素的全排列](https://leetcode.cn/problems/permutations-ii/)**
>
> - 使用全排列 **1** 的方法处理存在重复元素的情况，会导致排列有一半是重复的。
> 
> ![[回溯 - 2.png|Untitled 1 54.png]]
>
> - _==**使用相等元素剪枝**== **⇒**_ ==在每一轮选择中，保证多个相等的元素仅被选择一次。==在每一轮选择中（每一层递归） 创建一个哈希表`duplicated`，用于记录 该轮 中已经尝试过的元素，并将重复元素剪枝。
> 
> - _**示例 ⇒**_ 在第一轮中，选择 **1** 或选择 **1‘** 是等价的，在这两个选择之下生成的所有排列都是重复的，因此应该把 **1’** 剪枝。在第一轮选择 **2** 之后，第二轮选择中的 **1** 和 **1’** 也会产生重复分支，因此也应将第二轮的 **1‘** 剪枝。
> 
> ![[回溯 - 3.png|Untitled 2 50.png]]
>
> ```JavaScript
> var permute = function(nums) {
>   const res = []
>   backtrack([], nums, Array(nums.length).fill(false), res)
>   return res
> };
> 
> // state 表示问题的当前状态, {number[]}
> // choices 表示当前状态下可以做出的选择, {number[]}
> // selected[i] 表示 choices[i] 是否已被选择, {number[]}
> // res 表示结果数组
> function backtrack(state, choices, selected, res) {
>   // 当 当前状态的长度 等于 元素数量的长度 时，记录解
>   if (state.length === choices.length) {
>     res.push([...state])
>     return // 不再继续搜索
>   }
>   // 使用 Set 过滤重复元素，Set 只记录当前递归访问过的元素
> 	const duplicated = new Set() 
> 	// 遍历所有选择
>   choices.forEach((choice, i) => {
>     // 剪枝：不允许选择重复元素 且 不允许重复选择相同元素
>     if (!selected[i] && !duplicated.has(choice)) {
>       // 尝试：做出选择，更新状态
> 			duplicated.add(choice)  // 将当前元素添加进 Set
>       selected[i] = true
>       state.push(choice)
>       // 递归：进行下一轮选择
>       backtrack(state, choices, selected, res)
>       // 回退：撤销本次递归层的选择，恢复到之前的状态
>       selected[i] = false
>       state.pop()
>     }
> 	})
> }
> ```

---

### _子集_

> _**问题描述**_
>
> > [!important] **[LeetCode - 子集](https://leetcode.cn/problems/subsets/?envType=study-plan-v2&envId=top-100-liked)**
>
> - _**无重复元素的情况 ⇒**_ 给你一个整数数组 `nums` ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。
> 
> ```JavaScript
> var subsets = function(nums) {
>   // state 表示问题的当前状态, {number[]}
>   // choices 表示当前状态下可以做出的选择, {number[]}
>   // res 表示结果数组
>   // start 表示遍历起始点
>   const start = 0
>   const res = []
>   const state = []
>   const backtrack = (choices, start) => {
>     // 记录所有的子集解
>     res.push([...state])
>     // 遍历所有元素
>     for (let i = start; i < choices.length; i++) {
>       // 尝试：做出选择，更改 start / state
>       state.push(choices[i])
>       // 递归：下一轮选择,注意 start 变成了 i + 1
>       backtrack(choices, i + 1)
>       // 回退：撤销选择
>       state.pop()
>     } 
>   }
>   backtrack(nums, start)
>   return res
> };
> ```

---

### _**子集和**_

> _**Reference**_
>
> > [!important] **[Hello 算法 - 子集和问题](https://www.hello-algo.com/chapter_backtracking/subset_sum_problem/)**

> _**子集和情况 1 ⇒ 无重复元素的情况**_
>
> - _**题目描述 ⇒**_ 输入集合 **{3,4,5}** 和目标整数 **9** ，解为 {3, 3, 9}，{4, 5}。输入集合中的元素可以被==无限次重复选取====。==子集不区分元素顺序，比如 **{4,5}** 和 **{5,4}** 是同一个子集。
> 
> - _**重复子集 ⇒**_ 如果使用全排列中的方法，会导致结果中出现不同元素顺序的相同子集。
>     
>     - 当第一轮和第二轮分别选择 **3** 和 **4** 时，会生成包含这两个元素的所有子集，记为**[3,4,…]**
>     
>     - 当第一轮选择 **4** 时，则第二轮应该跳过 **3** ，因为该选择产生的子集 **[4,3,…]** 和第 **1** 步中生成的子集完全重复
>     
> 
> - _==**剪枝规则**== **⇒**_ 给定输入数组 $[x_1,x_2,..., x_n]$ ，当本轮作出选择 $x_i$ 后，下一轮从 $x_i$ 的下一个元素开始遍历，保证子集唯一
> 
> - _**算法思想 ⇒**_ 首先，先将数组 **nums** 升序排列。在遍历所有选择时，当子集和超过 **target** 时直接结束循环，因为后边的元素更大，其子集和一定超过 **target** 。然后通过在 **target** 上执行减法来统计元素和，当 **target** 等于 **0** 时记录解。
> 
> ![[回溯 - 5.png|Untitled 4 29.png]]
>
> ```JavaScript
> // state 表示问题的当前状态, {number[]}
> // choices 表示当前状态下可以做出的选择, {number[]}
> // res 表示结果数组
> // start 表示遍历起始点
> function backtrack(state, target, choices, start, res) {
>     // 子集和等于 target 时，记录解
>     if (target === 0) {
>         res.push([...state]);
>         return;
>     }
>     // 遍历所有选择
>     // 剪枝二：从 start 开始遍历，避免生成重复子集
>     for (let i = start; i < choices.length; i++) {
>         // 剪枝一：若子集和超过 target ，则直接结束循环
>         // 这是因为数组已排序，后边元素更大，子集和一定超过 target
>         if (target - choices[i] < 0) break        
>         // 尝试：做出选择，更新 target, start
>         state.push(choices[i]);
>         // 进行下一轮选择，传进去参数是 i 而不是 i+1 ，看题目描述
>         backtrack(state, target - choices[i], choices, i, res);
>         // 回退：撤销选择，恢复到之前的状态
>         state.pop();
>     }
> }
> 
> function subsetSumI(nums, target) {
>     const state = []; // 状态（子集）
>     nums.sort((a, b) => a - b); // 对 nums 进行排序
>     const start = 0; // 遍历起始点
>     const res = []; // 结果列表（子集列表）
>     backtrack(state, target, nums, start, res);
>     return res;
> }
> ```

> _**子集和情况 2 ⇒ 存在重复元素的情况**_
>
> - 给定一个正整数数组 `nums` 和一个目标正整数 `target` ，请找出所有可能的组合，使得组合中的元素和等于 `target`。==定数组可能包含重复元素，每个元素只可被选择一次。==请以列表形式返回这些组合，列表中不应包含重复组合
> 
> - _**算法思想 ⇒**_ 产生重复子集是因为相等元素在某轮中被多次选择。因此需要限制相等元素在每一轮中只被选择一次。由于先将数组排序，因此相等元素都是相邻的。若当前元素与其左边元素相等，则说明它已经被选择过，因此直接跳过当前元素。另外四种剪枝方法如图。
> 
> ![[回溯 - 6.png|Untitled 5 23.png]]
>
> ```JavaScript
> /* 回溯算法：子集和 II */
> function backtrack(state, target, choices, start, res) {
>     // 子集和等于 target 时，记录解
>     if (target === 0) {
>         res.push([...state]);
>         return;
>     }
>     // 遍历所有选择
>     // 剪枝二：从 start 开始遍历，避免生成重复子集
>     // 剪枝三：从 start 开始遍历，避免重复选择同一元素
>     for (let i = start; i < choices.length; i++) {
>         // 剪枝一：若子集和超过 target ，则直接结束循环
>         // 这是因为数组已排序，后边元素更大，子集和一定超过 target
>         if (target - choices[i] < 0) {
>             break;
>         }
>         // 剪枝四：如果该元素与左边元素相等，说明该搜索分支重复，直接跳过
>         if (i > start && choices[i] === choices[i - 1]) continue        
>         // 尝试：做出选择，更新 target, start
>         state.push(choices[i]);
>         // 进行下一轮选择
>         backtrack(state, target - choices[i], choices, i + 1, res);
>         // 回退：撤销选择，恢复到之前的状态
>         state.pop();
>     }
> }
> /* 求解子集和 II */
> function subsetSumII(nums, target) {
>     const state = []; // 状态（子集）
>     nums.sort((a, b) => a - b); // 对 nums 进行排序
>     const start = 0; // 遍历起始点
>     const res = []; // 结果列表（子集列表）
>     backtrack(state, target, nums, start, res);
>     return res;
> }
> ```

---

### _**N 皇后问题**_

> _**Reference**_
>
> > [!important] **[Hello 算法 - N 皇后问题](https://www.hello-algo.com/chapter_backtracking/n_queens_problem/)**
> >
> > **[LeetCode - N 皇后问题](https://leetcode.cn/problems/n-queens/description/?envType=study-plan-v2&envId=top-100-liked)**

> _**N 皇后问题理解**_
>
> - 根据国际象棋的规则，皇后可以攻击与同处一行、一列或一条斜线上的棋子。给定 **n** 个皇后和一个 **n x n** 大小的棋盘。寻找使得所有皇后之间无法相互攻击的摆放方案。
> 
> - _**示例 ⇒**_ 当`**n=4**`时，共可以找到两个解。从回溯算法的角度看，**n x n** 大小的棋盘共有 $n^2$ 个格子，给出了所有的选择 **choices** 。在逐个放置皇后的过程中，棋盘状态在不断地变化，每个时刻的棋盘就是状态 **state。**
> 
> ![[回溯 - 7.png|Untitled 6 18.png]]

> _**约束条件 & 剪枝 & 代码实现**_
>
> - _**四个约束条件 ⇒**_ 多个皇后不能在同一行、同一列、同一条主 / 次对角线上
> 
> - _**逐行放置策略 & 行剪枝 ⇒**_ 皇后的数量和棋盘的行数都为 ==**n**====，棋盘每行====都允许且只允许====放置一个皇后。==先在第一行第一列放置元素，递归到下一行；然后在第一行第二列放置元素，依此类推。此策略起到了剪枝作用，它避免了同一行出现多个皇后的所有搜索分支。
> 
> - _**列剪枝 ⇒**_ 为了满足列约束，可以利用一个长度为 **n** 的布尔型数组 **cols** 记录每一列是否有皇后。在每次决定放置前，通过 **cols** 将已有皇后的列进行剪枝，并在回溯中动态更新 **cols** 的状态。
> 
> - _**主 / 次对角线剪枝 ⇒**_ 设棋盘中某个格子的行列索引为`**(row, col)**`。则==主对角线====上所有格子的====`**row - col**`====为恒定值其范围是==`**[-n+1, n-1]**`==。====次对角线====上所有格子的====`**row + col**`====为恒定值，其==范围是`**[0, 2n-2]**`。然后借助数组 **diags1** 记录主对角线上是否有皇后，**diags2** 记录次对角线上是否有皇后，由于主 / 次对角线的个数都为 **2n-1**，则 **diags1** 和 **diags2** 的数组大小都为 **2n-1** 。
> 
> ![[回溯 - 8.png|Untitled 7 14.png]]
>
> ```JavaScript
> /* 回溯算法：n 皇后 */
> function backtrack(row, n, state, res, cols, diags1, diags2) {
>   // 当放置完所有行时，记录解
>   if (row === n) {
> 		// 将 state（二维数组）中的每一行进行拷贝放入结果数组
> 		// map 返回二维数组（因为 state 是二维数组），res 是三维数组
>     res.push(state.map((row) => row.slice()));
> 		// 将最里层的一维数组转换为字符串，res 返回二维数组，[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
> 		// res.push(state.map(row => row.join('')))
>     return;
>   }
>   // 遍历所有列
>   for (let col = 0; col < n; col++) {
>     // 计算该格子对应的主对角线和次对角线 对应的 diags1 和 diags2 数组的下标
>     const diag1 = row - col + n - 1;
>     const diag2 = row + col;
>     // 剪枝：不允许该格子所在列、主对角线、次对角线上存在皇后
>     if (!cols[col] && !diags1[diag1] && !diags2[diag2]) {
>       // 尝试：将皇后放置在该格子
>       state[row][col] = 'Q';
>       cols[col] = diags1[diag1] = diags2[diag2] = true;
>       // 放置下一行
>       backtrack(row + 1, n, state, res, cols, diags1, diags2);
>       // 回退：将该格子恢复为空位
>       state[row][col] = '#';
>       cols[col] = diags1[diag1] = diags2[diag2] = false;
>     }
>   }
> }
> 
> /* 求解 n 皇后 */
> function nQueens(n) {
>   // 初始化 n*n 大小的棋盘，其中 'Q' 代表皇后，'#' 代表空位
> 	// Array.from({ length: n })相当于生成一个长度为 n 的数组，每个元素为 undefined
> 	// const state = Array.from({ length: n }).map(() => Array(n).fill('#'));
>   const state = Array.from({ length: n }, () => Array(n).fill('#'));
>   const cols = Array(n).fill(false); // 记录列是否有皇后
>   const diags1 = Array(2 * n - 1).fill(false); // 记录主对角线上是否有皇后
>   const diags2 = Array(2 * n - 1).fill(false); // 记录次对角线上是否有皇后
>   const res = [];
> 
>   backtrack(0, n, state, res, cols, diags1, diags2);
>   return res;
> }
> ```
