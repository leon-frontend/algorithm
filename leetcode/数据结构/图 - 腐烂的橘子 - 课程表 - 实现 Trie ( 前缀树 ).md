### _**腐烂的橘子 & 广度优先遍历**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：值 `0` 代表空单元格；值 `1` 代表新鲜橘子；值 `**2**` 代表腐烂的橘子。每分钟，腐烂的橘子周围 **4** 个方向上相邻的新鲜橘子都会腐烂。返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1` 。

> _**图的广度优先遍历**_
>
> - 广度优先遍历是一种 由近及远 的遍历方式。从某个节点出发，始终优先访问距离最近的顶点，并一层层向外扩张。**BFS** 可以看成是层序遍历。
> 
> - 从某个结点出发，**BFS** 首先遍历到 距离为 _**1**_ 的结点，然后是距离为 _**2、3、4……**_ 的结点。对于 距离源节点路径相同的节点，其实它们在广度优先搜索上是等价于同一层的节点。
> 
> - ==因此，====**BFS**== ==可以用来求====权值相等的图的最短路径问题==
> 
> > [!important] **[Hello 算法 - 图的遍历](https://www.hello-algo.com/chapter_graph/graph_traversal/)**

> _**使用图的广度优先遍历**_
>
> - 本题实际上就是算广度优先遍历的最少遍历次数，即最短路径问题。对于距离源节点路径相同的节点，其实它们在广度优先搜索上是等价于同一层的节点。==注意====记录当前层的元素总数====，否则遍历总次数会出错。==由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。
> 
> ```Java
> /**
>  * @param {number[][]} grid
>  * @return {number}
>  */
> var orangesRotting = function(grid) {
>   // 使用图的广度优先遍历
>   // 1. 记录新鲜橘子的数量
>   // 2. 使用队列进行广度优先遍历
> 
>   // 声明队列，用于存储腐烂橘子
>   const queue = new Array()
> 
>   let freshOrange = 0 // 记录新鲜橘子的数量
>   let minute = 0 // 记录最小分钟数
> 
>   // 遍历二维数组记录新鲜橘子总数，并将腐烂橘子入队
>   for (let row = 0; row < grid.length; row++) {
>     for (let col = 0; col < grid[0].length; col++) {
>       // 如果为新鲜橘子，则 freshOrange++
>       if (grid[row][col] === 1) freshOrange++
>       // 如果为腐烂橘子，则将其坐标入队
>       if (grid[row][col] === 2) queue.push([row, col])
>     }
>   }
> 
>   // 开始广度优先遍历
>   while (queue.length && freshOrange > 0) {
>     // 进行一次广度优先遍历，就会使用一分钟
>     minute++
>     // 记录当前层需要遍历的元素数量
>     let size = queue.length
>     // 开始遍历本层的每个元素
>     for (let i = 0; i < size; i++) {
>       // 出队
>       let orange = queue.shift()
>       // 解构出出队元素的坐标
>       let [r, c] = orange
>       // ------------- 根据坐标开始入队及相关操作 ---------------
>       // 判断上方相邻的是否是新鲜橘子，判断有效范围
>       if (r - 1 >= 0 && grid[r - 1][c] == 1) {
>         // 将新鲜橘子变为腐烂橘子
>         grid[r - 1][c] = 2
>         // 将腐烂橘子的坐标入队
>         queue.push([r-1, c])
>         // 并让新鲜橘子总数减一
>         freshOrange--
>       }
> 
>       // 判断下方相邻的是否是新鲜橘子，判断有效范围
>       if (r + 1 < grid.length && grid[r + 1][c] == 1) {
>         // 将新鲜橘子变为腐烂橘子
>         grid[r + 1][c] = 2
>         // 将腐烂橘子的坐标入队
>         queue.push([r + 1, c])
>         // 并让新鲜橘子总数减一
>         freshOrange--
>       }
> 
>       // 判断左方相邻的是否是新鲜橘子，判断有效范围
>       if (c - 1 >= 0 && grid[r][c - 1] == 1) {
>         // 将新鲜橘子变为腐烂橘子
>         grid[r][c - 1] = 2
>         // 将腐烂橘子的坐标入队
>         queue.push([r, c - 1])
>         // 并让新鲜橘子总数减一
>         freshOrange--
>       }
> 
>       // 判断右方相邻的是否是新鲜橘子，判断有效范围
>       if (c + 1 < grid[0].length && grid[r][c + 1] == 1) {
>         // 将新鲜橘子变为腐烂橘子
>         grid[r][c + 1] = 2
>         // 将腐烂橘子的坐标入队
>         queue.push([r, c + 1])
>         // 并让新鲜橘子总数减一
>         freshOrange--
>       }
>     }
>   }
> 
>   if (freshOrange) return -1
>   else return minute
> };
> ```

---

### _**课程表**_

> _**问题描述**_
>
> - 这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 必须 先学习课程  `bi` 。
> 
> - 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。
> 
> ![[图 - 6.png|Untitled 1 51.png]]

> _**广度优先遍历**_
>
> - 统计课程安排图中每个节点的入度，生成 入度表 **indegrees**
> 
> - 创建邻接表（二维数组），记录受前置课程影响的所有课程。比如 ==`**adjacency[b]**`==表示以 _**b**_ 为前置课程的所有课程
> 
> - _**计算每个节点的入度 ⇒**_ 二维数组中的每一个小数组的第一个元素代表一个入度。因为如果想学当前课程，必须有前置课程，即入度。
> 
> - 借助一个队列 _**queue**_，将所有入度为 _**0**_ 的节点入队
> 
> - 当 **queue** 非空时，依次将队首节点出队，在课程安排图中删除此节点 **pre**_。_并不是真正从邻接表中删除此节点 **pre**，而是将此节点对应所有邻接节点 **cur** 的入度 **−1**，即 `**indegrees[cur] -= 1**`。当入度 **−1** 后邻接节点 **cur** 的入度为 **0**，说明 **cur** 所有的前驱节点已经被 “删除”，此时将 **cur** 入队。
> 
> - 在每次 **pre** 出队时，执行==`**numCourses--;**`====。==若整个课程安排图是有向无环图（即可以安排），则所有节点一定都入队并出队过，即完成拓扑排序。换个角度说，若课程安排图中存在环，一定有节点的入度始终不为 **0**。因此，拓扑排序出队次数等于课程个数，返回`**numCourses == 0**`判断课程是否可以成功安排。
> 
> ```Java
> /**
>  * @param {number} numCourses
>  * @param {number[][]} prerequisites
>  * @return {boolean}
>  */
> var canFinish = function(numCourses, prerequisites) {
>   // 本质上是在检测有向图中是否存在环。
>   // 如果存在环，则不可能完成所有课程的学习；如果不存在环，则可能完成所有课程的学习
> 
>   // 借助一个队列 queue，将所有入度为 0 的节点入队
>   const queue = []
>   // 统计课程安排图中每个节点的入度，生成 入度表 inDegrees
>   // inDegrees 的下标代表课程
>   const inDegrees = Array(numCourses).fill(0)
>   // 创建邻接表(二维数组)，记录受前置课程影响的所有课程
>   // 邻接表用于元素出队后，将相应元素的入度减一
>   // adjacency[b] 表示只有学了 b 课程才能学 adjacency[b] 数组中的课程
>   // 或理解为以 b 为前置课程的所有课程
>   const adjacency = Array.from({ length: numCourses }, () => [])
>   // 对于 prerequisites[i] 中的元素[a, b], a 的入度为 1
>   // 通过 prerequisites 数组更新 inDegrees 和 adjacency
>   for (let [a, b] of prerequisites) {
>     // 更新 inDegrees
>     inDegrees[a]++
>     // 更新 adjacency，adjacency[b] 表示以 b 为前置课程的所有课程
>     adjacency[b].push(a)
>   }
>   // 将所有入度为 0 的节点入队，inDegrees 的下标代表课程
>   inDegrees.forEach((inDegree, course) => {
>     if (inDegree === 0) queue.push(course)
>   })
>   // 使用广度优先遍历进行拓扑排序
>   while (queue.length) {
>     // 将入度为 0 的节点出队
>     let course = queue.shift()
>     // 一次出队过程表示一次学习过程
>     numCourses--
>     // 元素出队后，将相应元素的入度减一，并将入度为 0 的元素入队
>     // adjacency[course] 表示以 course 为前置课程的所有课程
>     for (let nextCourse of adjacency[course]) {
>       // 将相应元素的入度减一
>       inDegrees[nextCourse]--
>       // 将入度为 0 的元素入队
>       if (!inDegrees[nextCourse]) queue.push(nextCourse)
>     }
>   }
>   return numCourses === 0
> };
> ```

---

### _**实现 Trie ( 前缀树 )**_

> _**问题描述**_
>
> > [!important]
> >
> > **[LeetCode - 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构。用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查
> 
> ![[图 - 7.png|Untitled 2 47.png]]

> _**请你实现 Trie 类**_
>
> - `Trie()` 初始化前缀树对象。
> 
> - `void insert(String word)` 向前缀树中插入字符串 `word` 。
> 
> - `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
> 
> - `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。
> 
> ![[图 - 8.png|Untitled 3 34.png]]

> _**声明树节点，每个节点包含如下三个值**_
>
> > [!important] **[掘金 - JS 实现 Trie 字典树](https://juejin.cn/post/6844903941918949383)**
>
> - 每个节点有一个值
> 
> - 每个节点有 _**26**_ 个孩子节点，用`**{}**`对象表示，其中每个对象存的是 _**TreeNode**_
> 
> - 每个节点有 _**isWord**_ 属性，用来存 当前有没有以当前节点的 _**val**_ 字符结尾的单词
> 
> ```JavaScript
> class TreeNode{
>   constructor(val) {
>     this.val = val      // children 的 key 就是 value
>     this.chilrden = {}
> 		this.isWord = false  // 可以通过对象新增属性的方式添加
>   }
> }
> ```

> _**字典树构造函数**_
>
> - 初始化字典树，只有根元素，根元素也是一个节点，所以用`**TreeNode()**`
> 
> - 同时根元素没有 **val**，故 **TreeNode** 不需要传参数
> 
> - 暂时没有子元素，所以它的 **children** 属性暂时是： `**{}**`空对象
> 
> - **isWord** 属性也是默认的 **false**
> 
> ```JavaScript
> var Trie = function() {
>   this.root = new TreeNode()
> };
> ```

> _==**声明树节点 / 字典树构造函数 ⇒ 合并成一个函数**==_
>
> - 把嵌套对象理解成树结构。把对象中的 **key** 理解为当前节点的值，对象类型的 **value** 理解为子节点。
> 
> ```JavaScript
> var Trie = function() {
>     // children 中的 key 是 26 个字母
>     // value 是一个 对象，记录孩子节点
>     this.children = {}
> };
> ```

> _**代码实现**_
>
> ```JavaScript
> var Trie = function() {
>     // children 中的 key 是 26 个字母
>     // value 是一个 对象，记录孩子节点
>     this.children = {}
> };
> 
> /** 
>  * @param {string} word
>  * @return {void}
>  */
> Trie.prototype.insert = function(word) {
>     let curNode = this.children // 从根节点开始遍历
>     for (const ch of word) {
>         // 看当前节点（对象）是否包含当前字母 ch
>         // 若不包含，则以当前字母为 key，空对象为 value，给当前对象新增属性
>         if (!curNode[ch]) curNode[ch] = {}
>         // 若包含，则继续向下一个字母迭代
>         curNode = curNode[ch]
>     }
>     // 遍历完整个单词后，curNode 指向最后一个字母（节点）
>     // 给最后一个节点添加 isEnd 属性，表示该节点是该字符串的结尾
>     curNode.isEnd = true
> };
> 
> /** 
>  * @param {string} prefix
>  * @return {boolean}
>  */
> Trie.prototype.searchPrefix = function(prefix) {
>     let curNode = this.children // 从根节点开始遍历
>     for (const ch of prefix) {
>         // 看当前节点（对象）是否包含当前字母 ch
>         // 若不包含，则不存在该前缀，返回 false
>         if (!curNode[ch]) return false
>         // 若包含，则继续向下一个字母迭代
>         curNode = curNode[ch]
>     }
>     // 若遍历完单词，则说明 前缀树 中存在 prefix
>     // 返回 prefix 中的最后一个字母，用于后续处理
>     return curNode
> };
> 
> /** 
>  * @param {string} word
>  * @return {boolean}
>  */
> Trie.prototype.search = function(word) {
>     const lastNode = this.searchPrefix(word)
>     // lastNode !== undefiend 表示 word 存在于前缀树中
>     // lastNode.isEnd !== undefined 表示前缀树中存在 word 单词
>     return lastNode !== undefined && lastNode.isEnd !== undefined
> };
> 
> /** 
>  * @param {string} prefix
>  * @return {boolean}
>  */
> Trie.prototype.startsWith = function(prefix) {
>     // searchPrefix 要么返回 false;要么返回一个 对象，若对象存在，则返回 true
>     return this.searchPrefix(prefix)
> };
> 
> /**
>  * Your Trie object will be instantiated and called as such:
>  * var obj = new Trie()
>  * obj.insert(word)
>  * var param_2 = obj.search(word)
>  * var param_3 = obj.startsWith(prefix)
>  */
> ```
