

# [题目](https://leetcode-cn.com/problems/daily-temperatures/)

<img src="pic/%5Bclass%5D%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.assets/image-20220422201216261.png" alt="image-20220422201216261" style="zoom:50%;" />



# 题解

借用单调栈（栈内数字单调增or单调减）

如果新元素＜=栈顶元素：入栈

新元素＞栈顶元素：栈顶元素Pop，新元素继续与下一个栈顶元素比较

[动画图解:点击下方图片]()

[<img src="pic/%5Bclass%5D%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.assets/image-20220422201541459.png" alt="image-20220422201541459" style="zoom:25%;" />](https://leetcode-cn.com/problems/daily-temperatures/solution/leetcode-tu-jie-739mei-ri-wen-du-by-misterbooo/)

**伪代码**：

```go
for 整个字符串遍历{
   if { //栈内无元素 or 新元素 < 栈顶元素
      新元素入栈
   }
   for 栈内有元素 && 新元素 > 栈顶元素{
      计算栈顶元素对应answer
      Pop(栈顶元素)
   }
}
```







**一维数组辅助栈**：

栈内可以仅存储每个温度对应的序号 （而不用存储温度本身）

```go
func dailyTemperatures(temperatures []int) []int {
    lens := len(temperatures)
    answer, stack := make([]int, lens), []int{}
    stack = append(stack, 0)

    for i := 1; i < lens; i++ {
        temperature := temperatures[i]
        for len(stack) > 0 && temperature > temperatures[stack[len(stack) - 1]] { //栈内有元素 && 新元素 > 栈顶元素
            answer[stack[len(stack) - 1]] = i - stack[len(stack) - 1] // 栈顶元素对应answer:新元素序号 - 栈顶元素序号
            stack = stack[:len(stack) - 1] //Pop
        }
        if len(stack) == 0 || temperature <= temperatures[stack[len(stack) - 1]] { //栈内无元素 or 新元素 <= 栈顶元素
            stack = append(stack, i)
        }
    }
    return answer
}
```



**使用二维数组的栈**：

```go
func dailyTemperatures(temperatures []int) []int {
    lens := len(temperatures)
    answer, stack := make([]int, lens), [][]int{}
    stack = append(stack, []int{0, temperatures[0]})

    for i := 1; i < lens; i++ {
        temperature := temperatures[i]
        for len(stack) > 0 && temperature > stack[len(stack) - 1][1] { //栈内有元素 && 新元素 > 栈顶元素
            answer[stack[len(stack) - 1][0]] = i - stack[len(stack) - 1][0] // 栈顶元素对应answer:新元素序号 - 栈顶元素序号
            stack = stack[:len(stack) - 1] //Pop
        }
        if len(stack) == 0 || temperature <= stack[len(stack) - 1][1] { //栈内无元素 or 新元素 <= 栈顶元素
            stack = append(stack, []int{i, temperatures[i]})
        }
    }
    return answer
}
// 时间：遍历一次字符串O(n)
// 空间：使用一个空栈 O(n)

// 实现时，栈内可以仅存储每个温度对应的序号 （而不用温度本身）
```

