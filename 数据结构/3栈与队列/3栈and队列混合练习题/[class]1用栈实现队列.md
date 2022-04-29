

# [用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

- Go中没有栈，因此使用slice来作栈，并实现Pop、Push等

<img src="pic/%5Bclass%5D%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.assets/image-20220424211517216.png" alt="image-20220424211517216" style="zoom: 50%;" />



# 题解

用一个instack、 outStack栈合成一个队列

- 入队： 元素入instack

- 出队：如果outStack栈不空 ->直接出栈

   ​			如outStack栈空 -> 将instack栈所有元素依次出栈再依次入栈到outStack，Pop栈顶元素

- Peek：同出队，但不Pop栈顶元素
- empty：inStack、outStack都为空则empty



```go
type MyQueue struct {
    inStack, outStack []int
}

func Constructor() MyQueue {
    return MyQueue{} //会默认初始化为0
}

// 入栈
func (this *MyQueue) Push(x int)  {
    this.inStack = append(this.inStack, x)
}
// 将inStack元素全部出栈，依次入栈到outStack
func (this *MyQueue) in2out() {
    for len(this.inStack) > 0 {
        this.outStack = append(this.outStack, this.inStack[len(this.inStack) -1])
        this.inStack = this.inStack[:len(this.inStack) -1]
    }
}
// 出栈
func (this *MyQueue) Pop() int {
    if len(this.outStack) == 0 { //outStack无元素
        if this.Empty() {
            return -9999
        }
        this.in2out()
    }
    x := this.outStack[len(this.outStack) -1]
    this.outStack = this.outStack[:len(this.outStack) -1]
    return x
}

// 查看栈顶元素
func (this *MyQueue) Peek() int {
    if len(this.outStack) == 0 { //outStack无元素
        if this.Empty() {
            return -9999
        }
        this.in2out()
    }
    return this.outStack[len(this.outStack) -1]
}

func (this *MyQueue) Empty() bool {
    return len(this.inStack) == 0 && len(this.outStack) == 0 
}
```









