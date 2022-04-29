

> 第一次：没做起

# [题目地址](https://leetcode-cn.com/leetbook/read/array-and-string/cd71t/)

<img src="pic/%5Bclass%5D%E6%9C%80%E9%95%BF%E8%BF%9E%E7%BB%AD1%E7%9A%84%E4%B8%AA%E6%95%B0.assets/image-20220324095046263.png" alt="image-20220324095046263" style="zoom: 50%;" />



# 题解



分析：

- 【直接遍历数组】： 用maxcount 记录最大连续个数， count记录当前连续个数。
- **【记录方法】**：遇到1就count++, 遇到0就count清零，并且更新maxcount

- 【尾部处理】：最后一个元素如果是1，则在遍历最后一段连续数时，没有更新maxcount因此需要手动更新

   [来自官方解析](https://leetcode-cn.com/problems/max-consecutive-ones/solution/zui-da-lian-xu-1de-ge-shu-by-leetcode-so-252a/)

```go
func findMaxConsecutiveOnes(nums []int) int {
    // 记录当前连续1的个数 最大连续1的个数
    maxcount, count := 0, 0
    // 当数字为1时候count++
    // 当数字为0时候更新maxcount
    for _, num := range nums {
        if num == 1 {
            count++
        }else {
            maxcount = Max(count, maxcount)
            count = 0 // 当到达0时，count要重新计数
        }
    }
    // 数组到达末端，尾部不为0时，maxcount此时还没有继续更新，因此需要我们手动更新最后一次
    maxcount = Max(maxcount, count)
    return maxcount
}

func Max(x, y int) int {
    if x > y {
        return x
    }else {
        return y
    }
}
```

