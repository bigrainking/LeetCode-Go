## [题目]()



```
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值 target，返回 [-1, -1]。
进阶：
你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？
 

示例 1：
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]

示例 2：
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]

示例 3：
输入：nums = [], target = 0
输出：[-1,-1]
```





## 题解：

>  没做起，看的官方题解

找target的前后界限可以转换成找：

1. 起点：nums中 ≥target的第一个数 
2. 终点：nums中＞target的第一个数，终点 = 该数字下标-1

如：

[1, 2, 8, 8, 9] 起点为2， 终点 = (找到索引4 ) - 1 = 3



**如何找>target的第一个数字？**

二分法找

- 进入左右区间的条件：
   - mid > target 向左 right = mid -1， 并用一个ans 记住当前mid, ans = mid : 可以直接right = mid
   - mid < target 向右 left = mid+1
- 终止条件：不需要左右对比就可以判断是否满足条件，选用模板1



#### 代码1：不调用函数

```go
func searchRange(nums []int, target int) []int {
    left := searchBinary(nums, target, true)
    right := searchBinary(nums, target, false) - 1 //返回的是终点数字后面的数
    // 判断是否nums中不存在target
    fmt.Println(left, right)
    if left <= right && right < len(nums) && nums[left] == target && nums[right] == target {
        return []int{left, right}
    }
    return []int{-1, -1}
}

// 查找满足条件的target的最小位置
// 条件1 flag = true：nums中>=target的最小数
// 条件2 flag = false: nums中> target的最小数
func searchBinary(nums []int, target int, flag bool) int {
    left, right := 0, len(nums)-1
    ans := len(nums) // 如果没有满足条件的数字，则返回lens
    for left <= right {
        mid := left + (right - left) / 2
        if (target < nums[mid] || (flag && target <= nums[mid])) {
            right = mid - 1
            ans = mid
        }else { //向右
            left = mid + 1
        }
    }
    return ans
}
```



#### 代码：调用函数

- 用 `sort.SearchInts()` 写

- 关于 `sort.SearchInts(a []int, x int) int`的使用

   - 在排序int数组a中寻找数字x，并返回x的索引值。 如果数组中不存在x这个值，则返回x能够插入的位置使得插入后数组仍然有效

      [参考](https://pkg.go.dev/sort#SearchInts)

   - 例子：

      ```go
      func main() {
      	a := []int{1, 2, 3, 4, 6, 7, 8}
      
      	x := 2
      	i := sort.SearchInts(a, x)
      	fmt.Printf("found %d at index %d in %v\n", x, i, a)
      
      	x = 5
      	i = sort.SearchInts(a, x)
      	fmt.Printf("%d not found, can be inserted at index %d in %v\n", x, i, a)
      }
      -------------------
      Output:
      
      found 2 at index 1 in [1 2 3 4 6 7 8]
      5 not found, can be inserted at index 4 in [1 2 3 4 6 7 8]
      ```

      

- 本题答案：

   [官方答案见](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/zai-pai-xu-shu-zu-zhong-cha-zhao-yuan-su-de-di-3-4/)

   ```go
   func searchRange(nums []int, target int) []int {
       left := sort.SearchInts(nums, target)
       // 如果数组中不存在此数字
       if left == len(nums) || nums[left] != target {// 数组中无此数，选择插入到队尾 or 插入到队中间某个index 都无效
          //先判断left是否越界，后判断是否index对应数字不是target
          // 如[1,2,2,3]target=4，则插入的index=4
          
           return []int{-1, -1}
       }
       right := sort.SearchInts(nums, target+1) - 1
       // if left
       return []int{left, right}
   }
   ```

   