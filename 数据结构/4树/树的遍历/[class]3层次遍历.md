# 一、层次遍历

- [层次遍历的简介](https://leetcode-cn.com/leetbook/read/data-structure-binary-tree/xej9yc/)

   <img src="pic/%5Bclass%5D%E5%B1%82%E6%AC%A1%E9%81%8D%E5%8E%86.assets/image-20220506101909177.png" alt="image-20220506101909177" style="zoom:33%;" />





## 实现

[题目连接](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

<img src="pic/%5Bclass%5D%E5%B1%82%E6%AC%A1%E9%81%8D%E5%8E%86.assets/image-20220506102153445.png" alt="image-20220506102153445" style="zoom: 50%;" />



### 题解：

题目要求在结果中分离出每一层的节点。即，二维数组中每一个子数组包含的是每一层的节点。

**解决办法：**

- Count记住每一层节点个数
- 【该层循环nowCount次】nowCount = 该层的Count
- 【count记录下一层的节点个数】每次Dequeue()队头节点， 入栈其孩子节点并且每次入栈Count++





### 代码

**伪代码**

```go
res := [][]int // 遍历最终结果
ceng := []int //每层遍历结果

// 根节点入队
enqueue(node)
count++

//层次遍历整棵树
for  [队不空] {
   nowcount = count //记录该层需要循环的次数
   count = 0 // 清零，count用于记录下一层的节点数
   ceng = []int //清空该层的节点
   for nowcount {
      node = Dequeue() 
      ceng.append(node)
      //孩子节点全部入队
      if chiledleft {enqueue(nodechild) count++ }
      if chiledright {enqueue(nodechild) count++ }
   }
   res.append(ceng)
}
```



```go
func levelOrder(root *TreeNode) [][]int {
    result := [][]int{} // 最终输出层次遍历的顺序
    if root == nil { 
       return result
    }
    queue := [](*TreeNode){root} //层次遍历的队列，根节点入队
    count := 1 //记录当前层的节点数
    // 遍历后续节点
    for len(queue) != 0 {
        nowCount := count
        count = 0
        res := []int{} //承载每一层的层次遍历
        for i := 0; i < nowCount; i++ {
            node := queue[0] 
            queue = queue[1:] //出队
            res = append(res, node.Val)
            if node.Left != nil {
                queue = append(queue, node.Left) //入栈
                count++
            }
            if node.Right != nil {
                queue = append(queue, node.Right) //入栈
                count++
            }
        }
        result = append(result, res) //当前层次的节点入result
    }
    return result
}
```





# 二、不分层的层次遍历

不计算每一行有多少个节点

```go
queue = [](*TreeNode){root}
for len(queue) != 0 {
   node := queue[0]
   queue = queue[1:] // 这两层不能合并写，否则queue在重新定义
   if node.Left != nil {
      queue= append(queue, node.Left)
   }
   if node.Right != nil {
      queue = append(queue, node.Right) 
   }
}
```

