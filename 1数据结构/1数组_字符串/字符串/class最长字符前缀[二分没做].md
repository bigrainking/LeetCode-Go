### [最长公共前缀](https://leetcode-cn.com/leetbook/read/array-and-string/ceda1/)

> 战绩：没做出来没想到

编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 `""`。

示例 1：

输入：`strs = ["flower","flow","flight"]`
输出："fl"

示例 2：

输入：`strs = ["dog","racecar","car"]`
输出：""
解释：输入不存在公共前缀。

### 思路：

#### 方法一:横向比较



- 转化成两两字符串比较的问题，得出最大前缀字符串。

- 两个字符串比较，在Go中通过下标访问依次比较对应的元素

<img src="pic/%5B%E8%AF%BE%5D%E6%9C%80%E9%95%BF%E5%AD%97%E7%AC%A6%E5%89%8D%E7%BC%80.assets/image-20220307182504203.png" alt="image-20220307182504203" style="zoom:50%;" />



**没有解出的关键tip：**

Go字符串可以通过 == 来比较， 因此两个字符串的前缀直接用切片 == 来比较即可

（而不用每个元素一个个的比较！！）



**代码：**

**伪代码**

```go
//返回两个串的最长前缀
function LCP (str1, str2) prefix{
   依次比较每个元素；
   相同则继续往后比较，否则退出返回str[:index]
}

1. merge = 数组第一个串
2. 比较merge和第二个串找出最大相同前缀(用LCP function)，同时更新merge

```

**全部代码**

```go
func longestCommonPrefix(strs []string) string {
    prefix := strs[0]// 最大前缀
    if len(strs) == 1 {
        return prefix
    }
    // 比较两个字符串得到最大前缀
    index := 1
    for index<len(strs) && prefix != "" {
        prefix = LCP(prefix, strs[index]) //更新前缀
        index++
    }
    return prefix
}

// 找出两个字符串的最大前缀子串
func LCP(s1, s2 string) string {
    length := Min(len(s1), len(s2))
    index := 0
    for index < length && s1[index] == s2[index]{
        index++
    }
    return s1[:index]
}

func Min(x int, y int) int {
    if x < y {
        return x
    }else{
        return y
    }
}
```







#### 方法2：纵向比较

<img src="pic/class%E6%9C%80%E9%95%BF%E5%AD%97%E7%AC%A6%E5%89%8D%E7%BC%80.assets/image-20220308145229687.png" alt="image-20220308145229687" style="zoom: 50%;" />



按列遍历字符数组，每个字母与第一行对应字母想比较。如果i列出现不同的字符，则返回 第一行数组的前i个字母

```go
for i := 0; i < len(strs[0]) ; i++
```

```go
func longestCommonPrefix(strs []string) string {
    lens := len(strs)
    for i := 0; i < len(strs[0]) ; i++{
        for j := 0; j < lens; j++ {
            if len(strs[j]) == i || strs[j][i] != strs[0][i] { //发现第i个不同or发现某串长度不够（本串的长度与即将访问的索引号i相同，则已经到达了最后一个元素的后面，需要返回）
                return strs[0][:i]// 返回已经检查过的字符串 
            }
        }
    }
    return strs[0] // 全都相同
}
```





#### 方法三：二分法

- 这个方法太难了，代码写一半拉了，看完二分查找再看

<img src="pic/class%E6%9C%80%E9%95%BF%E5%AD%97%E7%AC%A6%E5%89%8D%E7%BC%80.assets/image-20220308160026673.png" alt="image-20220308160026673" style="zoom:50%;" />

相当于把问题转换成：

- 在一个字符串里面寻找符合条件的那个字符的索引。这个字符可以通过二分查找找到。

- 第一个字符串[:索引]的部分就是prefix的部分。（prefix：最长前缀）

- 二分查找通常判断条件是mid位置是否为target数字，这里判断条件是[0:mid]部分是否全部相同



1. 找到最短字符串长度(通过遍历整个字符串所有串的长度)

2. 将最短字符串二分，用 [0, mid]长度的最短字符串与剩下所有字符串的[0,mid]长度对比。

   相同：则`len(prefix)≥mid`，继续二分比较[mid,min]部分

   不同：则`len(prefix)≤mid`， 继续二分比较[0,mid]部分

   （不断二分是为了减少比较次数，Go中可以直接用 == 比较一整块字符串相比单个字符比较更加方便）

3. 直到找范围长度



**TIPS:**

```go

```

