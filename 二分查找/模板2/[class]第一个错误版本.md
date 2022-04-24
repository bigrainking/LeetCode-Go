## [题目](https://leetcode-cn.com/problems/first-bad-version/)

```
你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。
假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。
你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

示例 1：
输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false 
调用 isBadVersion(5) -> true 
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。

示例 2：
输入：n = 1, bad = 1
输出：1

提示：
1 <= bad <= n <= 231 - 1
```



## 题解

#### 方法一：

不断二分，不断判断mid是否满足target的条件，

target是否为错误版本的条件是：

- is(mid)以及mid后面都是True， is(mid-1)前面的都是False。（由于数组特性，我们只需要比较mid前后相邻数组）

#### 方法二：

（见代码一）

找到target数字。

那么如何确定某个数字就是target呢？除了上面说的，还可以通过二分法不断逼近直到left == right跳出循环，此时的left就是target.

<img src="pic/%5Bclass%5D%E7%8C%9C%E9%94%99%E8%AF%AF%E7%89%88%E6%9C%AC.assets/image-20220314195049816.png" alt="image-20220314195049816" style="zoom: 25%;" />

#### 方法三：

（见代码二）

`sort.Search()` 的作用是找到数组[0,n)中i满足`f(i) == true`,且`f(i+1)==true`的 正好我们的target数字满足这样的条件：一个最小的满足下面条件的数字`isB(target)==true`且`isB(target+1)==true`

代码一：直接写

```go
func firstBadVersion(n int) int {
    left, right := 1, n
    //isBadVersion()则是判断是否满足条件
    for left < right {
        mid := left + (right - left)/2
        // 因为mid一定是T的
        if isBadVersion(mid) { // 向左，target在[1,mid]
            right = mid //答案在[left, mid]中
        }else { // mid false向右
            left = mid + 1 //答案在[mid + 1, right]中
        }
    }
    return left // 退出时left == right
}
```

代码二：使用 `sort.Search()`

```go
func firstBadVersion(n int) int {
    return sort.Search(n, func(i int) bool {return isBadVersion(i)})
}
```

