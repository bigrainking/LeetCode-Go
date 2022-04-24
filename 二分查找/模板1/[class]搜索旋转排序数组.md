> n))析}019}8}716{514{3d2{1)029{8分7​6}514间3x2间1化0间9​8}7+6限504间3)2{1￼如下找 区间都大1]界点如下)：1504：3​21130：9​84706：5​4。3。2。1￼题目

## [题目](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

- 类似题目：[寻找旋转排序的最小值](https://leetcode-cn.com/leetbook/read/binary-search/xeawbd/)

```
整数数组 nums 按升序排列，数组中的值 互不相同 。
在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。
给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

示例 1：
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4

示例 2：
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1

示例 3：
输入：nums = [1], target = 0
输出：-1
```



## 题解：

### 方法一：(我的)

> 复杂度O(n)

部分有序，因此我们需要确定target所在区间，再在该区间使用二分。该方法思考很简单，复杂度高O(n)

**解题步骤如下**

1. **查找临界点划分区间**：遍历满足`nums[index] > nums[index+1]`(逆序)， 则是临界点

   左区间：`[0,index]` 右区间 `[index+1, lens-1]`

2. **确定target所在区间**：`nums`分为左右两个区间他们都内部有序，因此`nums[0]`比后面那个区间内所有数字都大

   - `target < nums[0]`则在右边的区间。`target >= nums[0]` 则在左边区间

3. **在区间内二分查找** 

### 代码如下

```go
func search(nums []int, target int) int {
    lens := len(nums)
    // 1. 找临界点确定区间
    index := 0
    for index <= (lens-2) && nums[index] < nums[index+1]{ // 先判断范围，以免索引超出界限
        index++
    }

    // 2. 确定区间
    left, right := 0, lens - 1 //初始化
    if target >= nums[0] { // 在左边区间
        right = index
    }else { // 在右区间
        left = index + 1
    }

    // 二分
    for left <= right {
        mid := (left+right)/2
        fmt.Println(mid)
        if nums[mid] == target {
            return mid
        }else if target < nums[mid] {
            right = mid - 1
        }else {
            left = mid + 1
        }
    }
    return -1
}
```



### 复杂度分析

- 时间：寻找临界点复杂度 `O(n)`, 后面二分的复杂度 `O(logn)`， 因此复杂度为 O(n)
- 空间：`O(n)`



### 方法二：（官方）

> 复杂度`O(logn)`

对数组直接使用二分法

- 如果[l, mid]是有序数组，且target在区间内，则进入左边寻找，否则进入右边
- 如果[mid+1, right]是有序数组，且target在区间内，则进入右边寻找，否则进入左边

<img src="pic/%5Bclass%5D%E6%90%9C%E7%B4%A2%E6%97%8B%E8%BD%AC%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220315101705915.png" alt="image-20220315101705915" style="zoom: 50%;" />

```go
func search(nums []int, target int) int {
    lens := len(nums)
    left, right := 0, lens-1

    for left <= right {
        mid := (left+right)/2
        if nums[mid] == target {
            return mid
        }else if nums[left] <= nums[mid] {// 左区间有序
            if nums[left]<=target && target<= nums[mid] {// target在区间内
                right = mid - 1// 进入区间内寻找
            }else { // 不在有序区间则进入另一个无序区间
                left = mid + 1
            }
        }else { //右边区间有序
            if nums[mid]<=target && target<=nums[right] { //在右区间
                left = mid + 1
            }else {
                right = mid - 1
            }
        }
    }
    return -1
}
```


