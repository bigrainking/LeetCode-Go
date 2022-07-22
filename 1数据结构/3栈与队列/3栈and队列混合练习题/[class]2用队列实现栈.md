

# [队列实现栈](https://leetcode-cn.com/leetbook/read/queue-stack/gw7fg/)











# 题解

入栈：利用两个队列

​			先入inQ， 再将outQ中所有元素出队入队到inQ

​			交换inQ， outQ

出栈：直接从outQ中获取值

<img src="pic/%5Bclass%5D%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.assets/225_fig1.gif" alt="225_fig1" style="zoom: 50%;" />





```go
type MyStack struct {
    inQ, outQ []int
}

func Constructor() MyStack {
    return MyStack{}
}

func (this *MyStack) Push(x int)  { 
    this.inQ = append(this.inQ, x) //入队
    for len(this.outQ) != 0 { // outQ全部转移到
        this.inQ = append(this.inQ, this.outQ[0])
        this.outQ = this.outQ[1:]
    }
    this.inQ, this.outQ = this.outQ, this.inQ// 交换
}

func (this *MyStack) Pop() int {
    if len(this.outQ) > 0 {
        x := this.outQ[0]
        this.outQ = this.outQ[1:]
        return x
    }else {
        return -9999
    }
}

func (this *MyStack) Top() int {
    if len(this.outQ) > 0 {
        return this.outQ[0]
    }else {
        return -9999
    }
}

func (this *MyStack) Empty() bool { //每次入栈inQ都只是暂时中转站，都会即刻清理掉数据，因此不会留下元素
    return len(this.outQ) == 0
}
```





