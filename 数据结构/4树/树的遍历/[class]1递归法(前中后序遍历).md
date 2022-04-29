

# 树的遍历



# 1 递归调用(三者通用)

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



# 栈



### 前序遍历

<img src="pic/%5Bclass%5D1%E9%80%92%E5%BD%92%E6%B3%95(%E5%89%8D%E4%B8%AD%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86).assets/images.gif" alt="images" style="zoom: 50%;" />

[图片来源](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode-solution/)

模拟递归中的潜在栈

- 遇到根节点先遍历，再入栈(保证之后右子树的遍历)

- node = Left ，继续遍历左子树



**复杂度**：

1. 时间： 遍历每个元素一次O(n)
2. 空间：借用栈，当链状树最多装n个元素 O(n)， 平均需要O(logn)

伪代码：

```go
for 栈不空 || node != nil { //转向右子树时，栈空，但是node = root.right = 5
   if node != nil {
      visit(node)
      push(node)
      node = left
   }else {
      node = top().right
      pop()
   }
}
```



代码：

```go
func preorderTraversal(root *TreeNode) []int {
    stack, res := [](*TreeNode){}, []int{}
    node := root
    for len(stack) != 0 || node != nil {
        if node != nil {
            res = append(res, node.Val)
            stack = append(stack, node)
            node = node.Left
        }else {
            node = stack[len(stack) - 1].Right
            stack = stack[:len(stack) - 1]
        }
    }
    return res
}
```



### 中序遍历

与前序遍历类似：

1. 根节点先压入栈
2. 一路将left节点压入栈
3. if某节点无 左节点 ： 
   1. 遍历栈顶节点（就是当前子树的根节点）
   2. pop()
   3. 遍历栈顶节点的右子树（node = top().Right) ：进入下一轮循环

**伪代码**：

```go
for 栈不空 || node != nil {
      if node != nil {
          push()
          n = left
      }else {
          n = top().Right
          if n pop()
		}
}
```







**GO 代码：**

```go
func inorderTraversal(root *TreeNode) []int {
    stack, res := [](*TreeNode){}, []int{}
    node := root
    for len(stack) != 0 || node != nil { //遍历到最后一个元素
        if node != nil {
            stack = append(stack, node)
            node = node.Left
        }else {
            res = append(res, stack[len(stack) - 1].Val)
            node = stack[len(stack) - 1].Right
            stack = stack[:len(stack) - 1]
        }
        
    }
    return res
}
```



### 后序遍历





- 将中、左、右节点按次序入栈（和前面的前序、中序遍历入栈一样）

- 用一个prev临时变量记录：上一个访问节点

   - node.Prev是刚刚访问节点的右节点，可以访问当前这个根节点。

      > 遍历顺序按照 ： 左 中 右。刚刚将右节点访问完了，则如果当前节点是 刚刚访问节点的父亲则访问这个根节点

      (比如当前是左叶子节点，prev就是父节点。当前节点不是prev的父节点；

      当前节点若是右叶子节点，prev)

后序遍历太难了！！！！！！！！！！！！！！

还没搞懂ing 

```go
func postorderTraversal(root *TreeNode) (res []int) {
    stk := []*TreeNode{}
    var prev *TreeNode
    for root != nil || len(stk) > 0 {
        for root != nil { //一路将左子树压入栈
            stk = append(stk, root)
            root = root.Left
        }
        root = stk[len(stk)-1]
        stk = stk[:len(stk)-1]
        if root.Right == nil || root.Right == prev { // node.Prev是刚刚访问节点的右节点
            res = append(res, root.Val)
            prev = root
            root = nil
        } else { //转向右子树
            stk = append(stk, root)
            root = root.Right
        }
    }
    return
}
```









#### 方法2：前序遍历反转

```go
 // 先序遍历： 中 左 右
    // 后序遍历： 左 右 中
    // 1. 调整先序遍历代码：变为 中 右 左 （这一步简单：一路将右子树压入栈）
    // 2. 将返回的res反转一下
    // for 栈不空 || node != nil { //转向右子树时，栈空，但是node = root.right = 5
    //     if node != nil {
    //         visit(node)
    //         push(node)
    //         node = right 
    //     }else {
    //         node = top().left
    //         pop()
    //     }
    // }
    // sort.Reverse(sort.IntSlice(s))
    stack, res := [](*TreeNode){}, []int{}
    node := root
    for len(stack) != 0 || node != nil {
        if node != nil {
            res = append(res, node.Val)
            stack = append(stack, node)
            node = node.Right
        }else {
            node = stack[len(stack) - 1].Left
            stack = stack[:len(stack) - 1]
        }
    }
    resPost := reverse(res)
    return resPost
}

func reverse(s []int) []int {
    for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
        s[i], s[j] = s[j], s[i]
    }
    return s
```











```go
func postorderTraversal(root *TreeNode) []int {
    for 当栈不空 || 节点不是空 ：
    1. 一路将当前节点压入栈， 如果有左右子树，压入左子树，如果只有右子树压入右子树
    2. if 当前节点是叶子节点 cur 
        Pop(cur)
        if top().Left == cur { Pop后的节点的左节点是cur
            visit(cur)
            node = node.Right
        }else { // cur是top的右子树
            visit(top())
            Pop(top())
        }
}
```

















