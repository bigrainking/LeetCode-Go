[toc]



# 路径总和()







# 题解

题目要求查看从根到叶子的路径和是否满足给出的targetNum



首先要知道每个叶子节点到根只有唯一的路径，因此可以

- 计算每个叶节点到根的路径和，如果存在targetNum则返回
- 路径 = 每个节点value之和
- 二叉树深度计算与本题一样，只是深度计算每个节点相当于value = 1

因此，使用二叉树深度计算的：自顶向下递归

**复杂度**

- 时间：遍历整个树 O(n)
- 空间：树的高度 O(height)







### **自顶向下递归**

#### 如何写出递归

**递归的方法**：   

1. 根到叶的长度 = 叶.Val + 叶.Parent到根的长度

2. 叶.Parent到根的长度 = 叶.Parent.Val + 叶.Parent.Parent到根的长度

3. 于是需要实现一个函数F(node, Num) ：

   **功能**：计算从root到node长度

​    	**方法**：长度 = Num + node.Val



```go
func hasPathSum(root *TreeNode, targetSum int) bool {
    flag := false
    var length func(root *TreeNode, road int)
    length = func(node *TreeNode, road int) {
        if node == nil {
            return
        }
        // 计算当前节点路径长
        road = road + node.Val
        if node.Left == nil && node.Right == nil {
            if road == targetSum {
                flag = true
            }
            return
        }
        // 先序遍历
        length(node.Left, road)
        length(node.Right, road)
    }
    length(root, 0)
    return flag
}
```





### 层次遍历

层次遍历

计算每个节点到根节点的长度，

当遇到叶子节点判断当前是否符合targetNum

```go
func hasPathSum(root *TreeNode, targetSum int) bool {
    // 层次遍历
    if root == nil {
        return false
    }
    numqueue, queue := []int{root.Val}, [](*TreeNode){root} //根节点入队
    for len(queue) != 0 {
        // 出队
        node, nodeLen := queue[0], numqueue[0]
        queue, numqueue = queue[1:], numqueue[1:]
        // 判断当前节点是否为叶子节点
        if node.Left == nil && node.Right == nil {
            if nodeLen == targetSum {
                return true
            }
            continue
        }
        // 孩子节点入队并计算对应的Length
        if node.Left != nil {
            len := nodeLen + node.Left.Val
            queue = append(queue, node.Left)
            numqueue = append(numqueue, len)
        }
        if node.Right != nil {
            len := nodeLen + node.Right.Val
            queue = append(queue, node.Right)
            numqueue = append(numqueue, len)
        }
    }
    return false
}
```

