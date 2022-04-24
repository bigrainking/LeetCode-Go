











暴力法：

```go
func minSubArrayLen(s int, nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    ans := math.MaxInt32
    for i := 0; i < n; i++ {
        sum := 0
        for j := i; j < n; j++ {
            sum += nums[j]
            if sum >= s {
                ans = min(ans, j - i + 1)
                break
            }
        }
    }
    //如果全部数字相加都没有target大，则ans不会更新
    if ans == math.MaxInt32 {
        return 0
    }
    return ans
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```



```go
lenNums := len(nums) //数组长度
    lenMin := len(nums) //最小子数组长度
    for low, _ := range nums {
        cursum := 0 //每次计算下一个数组时，sum要清零
        for fast := low; fast < lenNums; fast++ {
            cursum += nums[fast]
            if target <= cursum {
                if (fast - low + 1) < lenMin {//更新最小子数组相关信息
                    lenMin = fast - low + 1
                }
                break //当当前数组已经大于target的时候跳出循环
            }else if (fast == lenNums - 1) && (low == 0) { // 已经走到末尾
                return 0 // 如果整个数组之和都＜target则return 0
            }
        }   
    }
    return lenMin
}
```

