`key：` 用 `sort.Search` 解二分查找



## `sort.Search`

`sort.Search` 介绍：

从数组[0, n)中寻找一个index，index满足 f(i) == true 的最小索引， 并且同时 f(i+1) == true. 

找到返回index， 否则返回n

```go
index := sort.Search(n int,f func(i int) bool) int
```

输入：

- n : 排序树组的长度
- f() ：判断函数

输出：要查找的目标数值





## [题目](https://leetcode-cn.com/problems/guess-number-higher-or-lower/solution/cai-shu-zi-da-xiao-by-leetcode-solution-qdzu/)

在[0, n]查找`pick == num`的`num`, `pick`未知， 但 `guess(num int) int`会告诉你是否猜对.

`guess(num int) int` 返回

```
-1 : num > pick
1  : num < pick
0  : num == pick
```



## 模板

```go
func guessNumber (int n) {
   return sort.Search(n, func (i int) bool { return guess(i) <= 0})
}
```





## 题解：

```go
func guessNumber(n int) int {
    return sort.Search(n, func(x int) bool { return guess(x) <= 0 })
    
}
```



