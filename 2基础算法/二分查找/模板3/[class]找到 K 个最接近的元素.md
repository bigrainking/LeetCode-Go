

## [题目]()



## 题解

#### 方法1：二分法

[参考官方题解](https://leetcode-cn.com/problems/find-k-closest-elements/solution/zhao-dao-kge-zui-jie-jin-de-yuan-su-by-leetcode/)

《二分查找+双指针》

1. 首先考虑目标数字x在 `x<arr[0] or x > arr[len - 1]` 的情况，此时直接取数组前K个or数组后K个数字即可。
2. 考虑 arr[0] < x < arr[lens-1]的情况：
   1. 找到比x大一点点的数字的索引index
      - 为什么：如果x不在数组内那么需要找x可以插入的位置并index-1，如果x在数组内则index为x的索引。此时就分为了两种情况，为了简单考虑，我们可以直接找比x大一点点的元素的位置记为index。那么无论x是否在数组中，都将找到 该元素的位置。x的位置为index - 1
      - 怎么找：二分法(在有序数组中插入一个数)
   2. k个数字一定在范围`[index-1 -k, index-1 +k]`内
   3. 找出k个数字：双指针不断向中间移动
      - left =max( index - 1 -k, 0 ) , right = min (index -1 +k , lens-1)
      - left 比 right 更接近x, right --
      - right 比 left 更接近 x, left ++
      - 直到[left, right]之间为K个数字

```go
func findClosestElements(arr []int, k int, x int) []int {
    lens := len(arr)
    // 0.先判断x是否在arr范围外面
    if x <= arr[0] {
        return arr[:k]
    }
    if x >= arr[lens-1] {
        return arr[lens-k:]
    }
    
    // 1. 找到比x大一点点的数字对应的index:大于x的最小值 模板一：mid只需要与target比较
    index := 0
    left, right := 0, lens-1
    for left <= right {
        mid := left + (right - left) / 2
        if x < arr[mid] { //找比x大的,看看还有无大但是大的少一点的， 向左
            right = mid - 1
            index = mid
        }else {
            left = mid + 1
        }
    }
    fmt.Println(index)
    // 2. 在范围内找k个数字
    low, high := Max(0, index-1-k), Min(index-1+k, lens-1)
    for (high-low)+1 > k { //找到k个数字则停止
        fmt.Printf("low:%d, high:%d\n", low, high)
        if math.Abs(float64(arr[low] - x)) <= math.Abs(float64(arr[high]  - x)) {
            high--
        }else if math.Abs(float64(arr[low]  - x)) > math.Abs(float64(arr[high] - x)) {
            low++
        }
    }
    return arr[low:high+1]
}

func Max(x, y int) int {
    if x > y {
        return x
    }else{
        return y
    }
}

func Min(x, y int) int {
    if x < y {
        return x
    }else{
        return y
    }
} 
```



#### 方法2：[双指针删除法](https://leetcode-cn.com/problems/find-k-closest-elements/solution/pai-chu-fa-shuang-zhi-zhen-er-fen-fa-python-dai-ma/)

复杂度O(1)

- 审题：题目所求的数的范围可以通过不断删除数组前后边界来实现

   通过判断 x与前后边界的绝对值之差来确定删除左边界or右边界

- 如： [0,1, 2, 3, 4, 5,6] k=3, x=4 。 需要删除4次 = （lens - k）

   - `(4-0)>(6-4)` low++ = 1
   - `(4-1)>(6-4)` low++ = 2
   - `(4-2)==(6-4)` 且 6>2 high-- =5
   - `(4-2)>(5-4))` low++=3

   四次结束：return [`3:5+1]` = [3,4,5]

   ```go
   func findClosestElements(arr []int, k int, x int) []int {
       // 双指针法
       lens := len(arr)
       low, high := 0, lens-1
       times := lens - k
       for times > 0 {
           if math.Abs(float64(arr[low] - x)) <= math.Abs(float64(arr[high] - x)) {
               high--
           }else {
               low++
           }
           times--
       }
       return arr[low:high+1]
   }
   ```

   