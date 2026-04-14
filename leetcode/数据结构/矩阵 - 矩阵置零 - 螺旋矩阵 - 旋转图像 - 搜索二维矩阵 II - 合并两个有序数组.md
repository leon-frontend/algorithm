### _**矩阵置零**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给定一个 _`m`_ `x` _`n`_ 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **原地** 算法。
> 

> _**用第一行和第一列记录该行该列是否有 0**_
>
> - 对于第一行和第一列要设置一个标志位，为了防止自己这一行(一列)也有 _**0**_ 的情况
> 
> - 使用第一行第一列记录每一行每一列是否存在 _**0**_
>     
>     - 若不存在 _**0**_，位于第一行和第一列的元素不变
>     
>     - 若存在 _**0**_，则将第一行和第一列对应的元素置为 _**0**_，作为标记。
>         
>         - 既标记了此行此列，同时若此元素为 _**0**_，其对应的第一行和第一列的对应元素也需要被置 _**0**_
>          
> ```Java
> /**
>  * @param {number[][]} matrix
>  * @return {void} Do not return anything, modify matrix in-place instead.
>  */
> var setZeroes = function(matrix) {
>   // 使用二维矩阵的第一行和第一列表示此行或此列是否有 0，并用 0 标记
>   // 若 matrix[3][4]===0，则 matrix[0][4] = matrix[3][0] = 0
>   // 好处 1 => 标记了第三行和第四行存在 0
>   // 好处 2 => 直接将[0,4]和[3,0]的元素置零，后续的置零操作不用管这两个元素
>   // 此外，使用两个变量，标记第一行或第一列是否存在0
>   let rowZero = false, colZero = false
> 
>   // 遍历第一行，更新 rowZero 变量
>   // 若第一行存在零，则将 rowZero 设为 true => 用于后面将第一行置零
>   // 若第一行存在零，比如[0, 2]是 0，那第二列也应该置零
>   // 但 第二列置零 的操作会在第二次遍历矩阵时进行，不会用到 rowZero 变量
>   for (let col = 0; col < matrix[0].length; col++) {
>     if (matrix[0][col] === 0) {
>       rowZero = true
>       break
>     }
>   }
>   // 遍历第一列，更新 colZero 变量
>   for (let row = 0; row < matrix.length; row++) {
>     if (matrix[row][0] === 0) {
>       colZero = true
>       break
>     }
>   }
>   // 使用第一行第一列记录每一行每一列是否存在 0
>   for (let row = 1; row < matrix.length; row++) {
>     for (let col = 1; col < matrix[0].length; col++) {
>       if (matrix[row][col] === 0) matrix[row][0] = matrix[0][col] = 0
>     }
>   }
>   // 遍历矩阵，将相关元素置零
>   for (let row = 1; row < matrix.length; row++) {
>     for (let col = 1; col < matrix[0].length; col++) {
>       if (matrix[row][0] === 0 || matrix[0][col] === 0) matrix[row][col] = 0
>     }
>   }
>   // 通过 rowZero 变量判断是否将第一行置零
>   if (rowZero) {
>     for (let col = 0; col < matrix[0].length; col++) matrix[0][col] = 0
>   }
>   // 通过 colZero 变量判断是否将第一列置零
>   if (colZero) {
>     for (let row = 0; row < matrix.length; row++) matrix[row][0] = 0
>   }
> };
> ```

---

### _**螺旋矩阵**_

> _**问题描述**_
>
> - 给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。 

> _**模拟循环遍历**_
>
> - “从左向右、从上向下、从右向左、从下向上” 四个方向循环打印。
> 
> - 根据边界打印，即将元素按顺序添加至列表 _**res**_ 尾部。
> 
> - 边界向内收缩 1 （代表已被打印）。
> 
> - ==判断边界是否相遇（是否打印完毕），若打印完毕则跳出==_==**（整个循环退出条件）**==_
> 
> ![[矩阵 - 3.png|Untitled 2 37.png|327]]
>
> ![[矩阵 - 4.png|Untitled 3 27.png|350]]
>
> ```Java
> /**
>  * @param {number[][]} matrix
>  * @return {number[]}
>  */
> var spiralOrder = function(matrix) {
>   const res = []
> 
>   // “从左向右、从上向下、从右向左、从下向上” 四个方向循环打印
>   // 声明 4 个变量表示四个方向的边界
>   let left = 0, right = matrix[0].length - 1, 
>       top = 0, bottom = matrix.length - 1
>   
> 
>   // 从左向右 => 达到右边界时，则 从上向下 遍历，且上边界向下收缩，即 top++
>   // 从上向下 => 达到下边界时，则 从右向左 遍历，且右边界向左收缩，即 right--
>   // 从右向左 => 达到左边界时，则 从下向上 遍历，且下边界向上收缩，即 bottom--
>   // 从下向上 => 达到上边界时，则 从左向右 遍历，且左边界向右收缩，即 left++
>   // 退出循环条件 1 => 上边界 top 的值 > 下边界 bottom 的值
>   // 退出循环条件 2 => 右边界 right 的值 < 左边界 left 的值
>   // 退出循环条件 3 => 下边界 bottom 的值 < 上边界 top 的值
>   // 退出循环条件 4 => 左边界 left 的值 > 右边界 right 的值
>   while (true) {
>     // 从左向右
>     for (let i = left; i <= right; i++) res.push(matrix[top][i])
>     if (++top > bottom) break  // top 先加 1，再判断
> 
>     // 从上向下
>     for (let i = top; i <= bottom; i++) res.push(matrix[i][right])
>     if (--right < left) break
> 
>     // 从右向左
>     for (let i = right; i >= left; i--) res.push(matrix[bottom][i])
>     if (--bottom < top) break
> 
>     // 从下向上
>     for (let i = bottom; i >= top; i--) res.push(matrix[i][left])
>     if (++left > right) break
>   }
> 
>   return res
> };
> ```

---

### _**旋转图像**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 旋转图像](https://leetcode.cn/problems/rotate-image/description/?envType=study-plan-v2&envId=top-100-liked)**
>
> - 给定一个 _n_ × _n_ 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 _**90**_ 度。你必须在 **原地** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。
> 
> ![[矩阵 - 5.png|Untitled 4 22.png]]

> _**解法 1 ⇒ 观察旋转前后行和列的变化**_
>
> - 第 _**i**_ 行会变成第 _**n - i - 1**_ 列；第 _**j**_ 列会变成第 _**j**_ 行
> 
> ```Java
> public void rotate(int[][] matrix) {
>     int n = matrix.length;
>     // 创建辅助数组
>     // 防止在转换过程中 matrix 数组中的元素被覆盖
>     int[][] tmp = new int[n][];
>     for (int i = 0; i < n; i++) tmp[i] = matrix[i].clone();
>     // 旋转数组
>     for (int i = 0; i < n; i++) {
>         for (int j = 0; j < n; j++) {
>             matrix[j][n - i - 1] = tmp[i][j];
>         }
>     }
> }
> ```

> _**解法 2 ⇒ 原地算法**_
>
> - 只要分别以矩阵左上角 _**1/4**_ 的各元素为起始点执行以上旋转操作，即可完整实现矩阵旋转。
> 
> - 因为会存在 _**n**_ 等于奇数或偶数的情况，所以行取 _**n/2**_ ；列取 _**(n + 1) / 2**_
> 
> - 重点在于如何根据旋转后的坐标求出旋转前的坐标，避免覆盖
> 
> - 若旋转前坐标是`**(i, j)**`，则第 **i** 行变为第 **n - i - 1** 列；第 **j** 列变为第 **j** 行==`**(i,j)=>(j,n-i-1)**`==
> 
> - 若旋转后坐标是 `**(i, j)**`，则==`**(n-j-1, i) => (i, j)**`==
> 
> ![[矩阵 - 6.png|Untitled 5 19.png]]
>
> ```Java
> /**
>  * @param {number[][]} matrix
>  * @return {void} Do not return anything, modify matrix in-place instead.
>  */
> var rotate = function(matrix) {
>   // 分别以矩阵左上角 1/4 的各元素为起始点执行以上旋转操作，即可完整实现矩阵旋转
>   // 当 n 为偶数时，rows = cols = n / 2 
>   // 当 n 为奇数时，由于中心的位置经过旋转后位置不变，rows = n / 2, cols = (n+1) / 2
>   // 合并上述两种情况 => rows = n / 2; cols = (n + 1) / 2
>   const rows = Math.floor(matrix.length / 2), 
>         cols = Math.floor((matrix.length + 1) / 2),
>         n = matrix.length
> 
>   // 分析旋转操作前后，数组下标的对应关系
>   // 旋转前 [i, j] => 旋转后 [j, n-i-1]
>   // 为了防止元素覆盖，现将旋转后的元素设置为[i, j]，重点在于推出旋转的坐标[?, ?]
>   // [x, y] => [y, n-x-1] 可得 n-x-1=j => x=n-j-1
>   // 旋转前 [n-j-1, i]  => 旋转后 [i, j]
>   for (let i = 0; i < rows; i++) {
>     for (let j = 0; j < cols; j++) {
>       const tmp = matrix[i][j]
>       matrix[i][j] = matrix[n-j-1][i]
>       // 旋转前 [n-i-1, n-j-1]  => 旋转后 [n-j-1, i]
>       matrix[n-j-1][i] = matrix[n-i-1][n-j-1]
>       // 旋转前 [j, n-i-1]  => 旋转后 [n-i-1, n-j-1] 
>       matrix[n-i-1][n-j-1] = matrix[j][n-i-1]
>       // 旋转前 [i, j]  => 旋转后 [n-i-1, n-j-1] 
>       matrix[j][n-i-1] = tmp
>     }
>   }
> };
> ```

---

### _**搜索二维矩阵 II**_

> _**问题描述**_
>
> - 编写一个高效的算法来搜索 _`m`_ `x` _`n`_ 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：每行的元素从左到右升序排列；每列的元素从上到下升序排列。 

> _**解法 1 ⇒ 二分查找**_
>
> - _==**数组必须是有序的**==_
> 
> - 使用双指针确定查找范围，使用 _**mid**_ 指针指向查找范围的中间位置
> 
> - 若 _**target**_ 小于 _**mid**_ 指向的元素，则重新给 _**high**_ 赋值，缩小查找范围，即`**high = mid - 1**`
> 
> - 若 _**target 大**_于 _**mid**_ 指向的元素，则重新给 _**low**_ 赋值，缩小查找范围，即`**low = mid + 1**`
> 
> - 时间复杂度 **O(mlog⁡n)**。对一行使用二分查找的时间复杂度为 _**O(log⁡n)**_，最多需要进行 _**m**_ 次二分查找。
> 
> - 空间复杂度 **O(1)**
> 
> ```Java
> /**
>  * @param {number[][]} matrix
>  * @param {number} target
>  * @return {boolean}
>  */
> var searchMatrix = function(matrix, target) {
>   const rows = matrix.length, cols = matrix[0].length
>   let left = 0, right = rows * cols - 1, mid
>   while (left <= right) {
>     mid = left + Math.floor((right - left) / 2)
>     // 将 mid 转换为二维数组的下标，转换过程只与 cols 有关
>     const x = matrix[Math.floor(mid / cols)][mid % cols]
>     if (x < target) left = mid + 1
>     else if (x > target) right = mid - 1
>     else return true
>   }
>   return false
> };
> ```

> _**解法 2 ⇒ Z 字形查找**_
>
> - 将矩阵左旋 45° 类似于一颗排序二叉树。==从矩阵== ==**matrix**== ==的右上角====`**(0,n−1)**`====进行搜索。==在每一步的搜索过程中，如果我们位于位置 **(x,y)**，那么我们希望在以 **matrix** 的左下角为左下角、以 **(x,y)** 为右上角的矩阵中进行搜索即行的范围为`**[x,m−1]**`，列的范围为`**[0,y]**`
> 
> - 如果`**matrix[x,y]=target**`，说明搜索完成
> 
> - 如果==`**matrix[x,y]>target**`==，由于每一列的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于第 _**y**_ 列的元素都是严格大于 _**target**_ 的，因此我们可以将它们全部忽略，即将 _**y**_ 减少 _**1**_
> 
> - 如果 `**matrix[x,y]<target**`，由于每一行的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于第 _**x**_ 行的元素都是严格小于 _**target**_ 的，因此我们可以将它们全部忽略，即将 _**x**_ 增加 _**1**_
> 
> - 在搜索的过程中，如果我们超出了矩阵的边界，那么说明矩阵中不存在 _**target**_
> 
> - 时间复杂度 **O(m+n)**。如果我们没有找到 _**targe**_，那么我们要么将 **y** 减少 **1**，要么将 **x** 增加 **1**_**。**_由于 **(x,y)** 的初始值分别为 **(0,n−1)**，因此 **y** 最多能被减少 **n** 次，**x** 最多能被增加 _**m**_ 次，总搜索次数为 **m+n**。在这之后，**x** 和 **y** 就会超出矩阵的边界。
> 
> - 空间复杂度 **O(1)**
> 
> ```Java
> /**
>  * @param {number[][]} matrix
>  * @param {number} target
>  * @return {boolean}
>  */
> var searchMatrix = function(matrix, target) {
>   // 从矩阵右上角开始搜索
>   // 每行的元素从左到右升序排列 => 右边的元素一定比左边大
>   // 每列的元素从上到下升序排列 => 下面的元素一定比上面大
>   // 比当前元素小时就往左边搜索，比当前元素大时就往下边搜索
>   // 和当前元素相等相等时就返回 true  
>   let row = 0, col = matrix[0].length - 1
>   while (row < matrix.length && col >= 0) {
>     if (matrix[row][col] === target) return true
>     else if ( target > matrix[row][col]) row++
>     else col--
>   }
>   return false
> };
> ```

---

### _**合并两个有序数组**_

> _**问题描述**_
>
> > [!important] **[LeetCode - 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)**
>
> - 给你两个按 **非递减顺序** 排列的整数数组`nums1`和`nums2`，另有两个整数`m`和`n`，分别表示`nums1`和`nums2`中的元素数目。请你**合并**`nums2`到`nums1`中，使合并后的数组同样按**非递减顺序**排列。==注意最终合并后的数组不应由函数返回，而是存储在数组====`nums1`====中。==为了应对这种情况，`nums1`的初始长度为`m + n`，其中前`m`个元素表示应合并的元素，后`n`个元素为`0` ，应忽略。`nums2`的长度为`n` 。
>
> ```Java
> /**
>  * @param {number[]} nums1
>  * @param {number} m
>  * @param {number[]} nums2
>  * @param {number} n
>  * @return {void} Do not return anything, modify nums1 in-place instead.
>  */
> var merge = function(nums1, m, nums2, n) {
>   // 从后往前遍历数组，避免元素被覆盖
>   let p1 = m - 1, p2 = n - 1, p = nums1.length - 1
> 
>   while (p1 >= 0 && p2 >= 0) {
>     if (nums1[p1] > nums2[p2]) nums1[p--] = nums1[p1--]
>     else nums1[p--] = nums2[p2--]
>   }
> 	
>   while (p2 >= 0) nums1[p--] = nums2[p2--]
> };
> ```
