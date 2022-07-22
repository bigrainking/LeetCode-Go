

[toc]



# 树的遍历



## 

## 一、递归调用(三者通用)

> 递归法遍历， 前中后序遍历方法差不多



### [前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode-solution/)：

```go
func preorderTraversal(root *TreeNode) []int {
    res := []int{}
    // 匿名函数循环调用
    var preorder func(node *TreeNode)
    preorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        res = append(res, node.Val) //中
        preorder(node.Left)
        preorder(node.Right)
    }
    preorder(root) //第一次调用
    return res
}
```

### [中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/)：

```go
func inorderTraversal(root *TreeNode) []int {
    res := []int{}
    var Inorder func(node *TreeNode)
    Inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        Inorder(node.Left)
        res = append(res, node.Val)
        Inorder(node.Right)
    }
    Inorder(root)
    return res
}
```

### [后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/er-cha-shu-de-hou-xu-bian-li-by-leetcode-solution/)：

```go
func postorderTraversal(root *TreeNode) []int {
    res := []int{}
    var postorder func(node *TreeNode)
    postorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        postorder(node.Left)
        postorder(node.Right)
        res = append(res, node.Val)
    }
    postorder(root)
    return res
}
```



