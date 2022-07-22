## [题目](https://leetcode-cn.com/problems/find-peak-element/)



```
寻找峰值
峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞ 。

你必须实现时间复杂度为 O(log n) 的算法来解决此问题。

 

示例 1：

输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
示例 2：

输入：nums = [1,2,1,3,5,6,4]
输出：1 或 5 
解释：你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。

作者：力扣 (LeetCode)
链接：https://leetcode-cn.com/leetbook/read/binary-search/xem7js/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



## 题解：

需要使用mid的右邻居来判断是否满足条件，使用模板2

首先需要明确根据题意必有峰值：

- 单调递增or递减则峰值在边界
- 否则一旦转弯则定有峰值

<img src="pic/%5Bclass%5D%E5%AF%BB%E6%89%BE%E5%B3%B0%E5%80%BC.assets/image-20220317092205206.png" alt="image-20220317092205206" style="zoom: 50%;" />



#### 方法1：二分爬坡(模板2)

**key: 上坡必然有顶。**

复杂度`O(logn)`

那么就是说，当 target < target + 1 峰值一定在右边，则往右 同时left = mid + 1

target > target + 1, 峰值可能是自己or自己的左边，则往左 同时right = mid  (为什么不是mid+1? 因为target可能是最大的元素，也可能target-1是最大的元素，因此需要再左区间[left, mid]中寻找峰值)

- 为什么用模板2：仅与右邻居比较来确定向哪个区间进

```go
func findPeakElement(nums []int) int {
    lens := len(nums)
    left, right := 0, lens 
    for left < right {
        mid := left + (right - left)/2
        if nums[mid] > nums[mid+1] {
            right = mid //mid满足条件，继续向左看是否mid>mid-1
        }else { // 没有成为峰值的潜能
            left = mid + 1
        }
    }
    return left
}
```



#### 方法2：直接迭代爬坡

方法1中的爬坡方法是用二分爬坡， 还可以直接选择一个位置开始爬坡

> 该方法更简单，复杂度为O(n)

爬坡方法：

- 如果是峰值， 直接返回
- 如果不是峰值
   - 如果 nums[index -1] < nums[index] 向右：这样当index在山谷时，我们规定向右爬
   - nums[index -1] > nums[index]  向左

```go
func findPeakElement(nums []int) int {
    Get := func(index int) int {
    if index == -1 || index == len(nums) { 
        // 当index在边界，如果n[0] > n[1], 则n[0]是峰值
        // 为了使边界的判定与其他位置一样，当index = -1 or len(nums)时， 返回最小值，使边界值任何时候都大于最小值
        return math.MinInt64
    }
    return nums[index]
    }

    // 爬坡迭代
    index := 0
    for !(Get(index-1) < Get(index) && Get(index) > Get(index+1)) { 
        if Get(index) > Get(index-1) {
            index++
        }else{
            index--
        }
    }
    return index
}

// 获取index对应的数值，

```







#### 方法3：直接二分(模板3)

> 思路简单，代码相比模板2复杂 
>
> 复杂度：O(logn) 

**思路：**

- 排除只有一个元素的情况 （进入模板3的循环至少要有3个元素（2个元素的情况会在后面处理））

- 二分：

   - 退出循环的条件是 left + 1 == right 。 这是为了确保始终区间内有至少3个元素

   - mid与左右邻居比较，如果是峰值则返回

   - 否则哪边大则进入哪边的区间 , 如(2, 3, 4) 则进入右区间。 

      原因：根据上坡则一定有峰值，如果一直走到边界都没有峰值，则边界值是峰值(我修改测试例发现官方这样规定的)

   - 跳出循环后还剩2个值，直接判断是否符合条件。 同时len(nums) = 2的情况也会在这一步得到解决

```go
func findPeakElement(nums []int) int {
    // 模板三：需要与mid的左右比较
    // 进入左右区间的条件:上坡必有顶
    endindex := len(nums) - 1
    if endindex == 0 {
        return 0
    }
    left, right := 0, endindex
    for left + 1 < right { //保证mid左右有可以比较的内容
        mid := left + (right - left) / 2
        fmt.Printf("left:%d, mid:%d, right:%d\n", left, mid, right)
        if (nums[mid-1] < nums[mid]) && (nums[mid+1] < nums[mid]) {
            return mid
        }else if (nums[mid - 1] > nums[mid]) { //上坡必有顶，万一一直上坡直到边界呢？边界就是峰值
            right = mid
        } else {
            left = mid
        }
    }
    // 剩下2个元素：直接判断是否符合条件
    if right == endindex && nums[left] < nums[right] { //峰值在右边界
        return endindex
    }else if left == 0 && nums[left] > nums[right] {
        return 0
    }
    return -1
}
```



























