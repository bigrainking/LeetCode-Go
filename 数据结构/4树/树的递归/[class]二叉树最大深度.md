# [二叉树最大深度](https://leetcode-cn.com/leetbook/read/data-structure-binary-tree/xoh1zg/)

<img src="pic/%5Bclass%5D%E4%BA%8C%E5%8F%89%E6%A0%91%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.assets/image-20220507163636470.png" alt="image-20220507163636470" style="zoom:33%;" />



# 题解

## 法1: 递归

根据本节的Readme.md, 有**自顶向下**(先序遍历的递归法) 和 **自底向上**(后序遍历的递归法)两种方法

- 两种方法的写法与二叉树的递归遍历类似， 主要难在匿名函数的写法上



**复杂度分析**：

时间复杂度：O(n)

空间复杂度：O(height) 递归深度取决于树的高度，平衡二叉树为O(logn), 单边树为O(n)



**自顶向下**

- 注意判断当前节点是否为叶子节点时，需要先判断当前节点是否为nil

```go
// 自顶向下
func maxDepth(root *TreeNode) int {
    res:= 0
    // 匿名函数内部函数循环
    var maxdepth func(root *TreeNode, depth int)
    maxdepth = func(root *TreeNode, depth int) {
        if root == nil { //防止后面出现nil指针
            return
        }
        if root.Left == nil && root.Right == nil { //当为叶子节点则更新result
            res = max(res, depth)
            return
        }
        maxdepth(root.Left, depth+1)// 左子树
        maxdepth(root.Right, depth+1)// 右子树   
    }
    // 根节点第一次调用
    maxdepth(root, 1)//初始root节点深度为1
    return res
}

func max(x, y int) int {
    if x > y {
        return x
    }else {
        return y
    }
}
```



**自底向上**

```go
// 自底向上：后序遍历的递归法
func maxDepth(root *TreeNode) int {
    // 先计算左右子树的深度，树的深度 = max(left, right) + 1
    var maxdepth func(root *TreeNode) int
    maxdepth = func(root *TreeNode) int {
        if root == nil {
            return 0
        }
        left_depth := maxdepth(root.Left)
        right_depth := maxdepth(root.Right)
        return max(left_depth, right_depth) + 1
    }
    return maxdepth(root)
}
```



## 法2: 层次遍历

非递归的层次遍历计算树的层数



**伪代码**：

```go
if root 为空 return 0
depth := 0
根节点入队;
count = 1 : 当前层的元素个数为1
for 【队不空】 {
   depth++ //每循环一层，depth+1
   nowCount = count
   count = 0 
   for nowCount次 {
      node = Dequeue()
      Enqueue(node左孩子); count++
      Enqueue(node右孩子); count++
   }

}
```



**Go代码**

```go
// 层次遍历计算层数
func maxDepth(root *TreeNode) int {
	if root == nil {
        return 0
    }
    // 根节点入队
    queue := [](*TreeNode){root}
    depth, count := 0, 1
    for len(queue) > 0 {
        depth++
        nowCount := count
        count = 0 
        for i := 0; i < nowCount; i++ {
            root = queue[0]
            queue = queue[1:]
            if root.Left != nil {
                queue = append(queue, root.Left)
                count++
            }
            if root.Right != nil {
                queue = append(queue, root.Right)
                count++
            }
        }
    }
    return depth
}
```

