

[toc]



## [题目](https://leetcode-cn.com/leetbook/read/binary-search/xeqevt/)

```
给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序排列  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。
以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。
你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。
你所设计的解决方案必须只使用常量级的额外空间。 
示例 1：
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

示例 2：
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。

示例 3：
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```



## 题解

#### 二分查找法

> - 本题将在数组中二分查找target，改为查找两个数字。
> - 第一个数字固定，在剩下的有序数组中使用二分查找剩下的一个数字。 二分O(logn) * 固定数字O(n) = O(nlogn)



- 因此在给出两数之和的情况下， 固定一个数字`nums[i]`，**在`nums[i: lens-1]`数组中用二分查找我们的target2**。

- target2 = target - nums[1]

```go
func twoSum(nums []int, target int) []int {
    // 固定一个元素i， 在[i:lens-1]中二分查找剩下的一个数字
    // 剩下的一个数字为 target - nums[i]
    left, right :=0, len(nums) - 1
    for i := 0; i < len(nums); i++ { //i是固定的数字
        left = i + 1 //从固定数字后面开始寻找
        tar2 := target - nums[i] // 另一个数字
        // 在剩下数组中二分查找
        for left <= right {
            mid := left + (right - left) / 2
            if tar2 == nums[mid] {
                return []int{i+1, mid+1} //数组下标从1开始
            }else if tar2 < nums[mid] {
                right = mid - 1
            }else {
                left = mid + 1
            }
        }
        // 如果没有找到则继续进行下一次循环
    }
    return []int{0, 0}
}
```



#### 双指针法

- **缩减搜索空间法**

本题要求找到两数之和为target。

1） 我们设置两个指针`i, j` , 假设nums的长度为8

​	则 `i,j` 的搜索空间是下图中的白色部分。

- A[i] + A[j] = target  i ≠ j

<img src="pic/%5Bmore%5D%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C%20II%20-%20%E8%BE%93%E5%85%A5%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220323105459918.png" alt="image-20220323105459918" style="zoom:50%;" />

2.1）双指针法就是在缩减搜索空间，缩减办法如下举例：

首先 如果A[0] + A[7] < target ，要找和＞target的，那么A[0] + A[6] (or A[1,2,3,4,5])这部分之和都比target小(因为数组有序)，则舍去。就是把上图中的第一行白格子去掉了.

此时搜索空间变为如下：此时对应的`i++`

<img src="pic/%5Bmore%5D%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C%20II%20-%20%E8%BE%93%E5%85%A5%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220323110332171.png" alt="image-20220323110332171" style="zoom: 50%;" />

2.2）继续 A[1] + A[7] > target ，要找之和＜target的，因此 A[2] (以及A[3]...A[6])+ A[7] > target， 则这些都应该舍去。对应的搜索空间将会舍去A[7]那一列

此时搜索空间如下：同时`j--`

<img src="pic/%5Bmore%5D%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C%20II%20-%20%E8%BE%93%E5%85%A5%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220323111057018.png" alt="image-20220323111057018" style="zoom:50%;" />

3)最后逐渐缩小，直到找到 A[i] + A[j] == target

<img src="pic/%5Bmore%5D%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C%20II%20-%20%E8%BE%93%E5%85%A5%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220323111236715.png" alt="image-20220323111236715" style="zoom: 25%;" /><img src="pic/%5Bmore%5D%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C%20II%20-%20%E8%BE%93%E5%85%A5%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84.assets/image-20220323111345474.png" alt="image-20220323111345474" style="zoom:25%;" />



4) 复杂度分析：

最坏的情况是搜索空间缩减为最后一个才找到，缩减过程中加入每次都是删减横着一行， 则最多需要与target比较n-1次

复杂度O(n)



[参考链接](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/solution/yi-zhang-tu-gao-su-ni-on-de-shuang-zhi-zhen-jie-fa/)

代码：

```go
func twoSum(numbers []int, target int) []int {
    // 初始化
    i, j := 0, len(numbers) - 1
    for !(numbers[i] + numbers[j] == target) {
        if numbers[i] + numbers[j] > target {
            // 找更小的，删除A[j]对应的一整列
            j--
        }else {
            i++ // 找更大的，删除A[i]对应的一整行
        }
    }
    return []int{i+1,j+1} // 数组下标从1开始
}
```

















