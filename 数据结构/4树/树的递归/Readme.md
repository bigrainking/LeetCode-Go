

# 树的递归

`递归`是解决树相关问题的最有效和最常用的方法之一。

本节中，我们将会介绍两种典型的递归方法： **自顶向下**







## 一、自顶向下

方法原理类似于 **先序遍历**：先根节点，后左右子树

- 在每个层级递归， 先计算当前节点的值， 再递归调用孩子节点，当前值传递给孩子节点并计算孩子节点的对应值



下面计算树的最大深度就是**自顶向下**法：首先要知道树的深度为depth最大的叶子节点的depth。

1. 首先计算root的depth为1 
2. 如果当前节点是叶子节点则 更新整棵树的result值 = MAX(当前depth, result) 
3. 如果不是叶子节点，递归计算孩子节点的depth，Max_depth(root.Left, depth+1)

- depth在每次向下不断递归的时候都在计算

### 树的最大深度



伪代码：求树的最大深度办法：

```go
return if root is null
if root is a leaf node:
 		answer = max(answer, depth) // update the answer if needed
maximum_depth(root.left, depth + 1)// call the function recursively for left child
maximum_depth(root.right, depth + 1)// call the function recursively for right child
```

Go代码

```java
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



<img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507153356658.png" alt="image-20220507153356658" style="zoom: 25%;" /> <img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507153414342.png" alt="image-20220507153414342" style="zoom: 25%;" /> 

<img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507153436187.png" alt="image-20220507153436187" style="zoom: 25%;" /> <img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507153515909.png" alt="image-20220507153515909" style="zoom: 25%;" />



## 二、自底向上

**自底向上**相当于后序遍历， 先计算左右子树，再计算根节点。

下面的伪代码与后序遍历相似：

```go
return specific value for null node //当递归到叶子节点的孩子节点时 return null
left_ans = bottom_up(root.left)// call function recursively for left child
right_ans = bottom_up(root.right)// call function recursively for right child
return answers          // answer <-- left_ans, right_ans, root.val
```



### 树的深度

用自底向上的方法计算树的深度：

- 首先树的深度为 Max(左子树深度， 右子树深度) + 1
- 递归的，我们计算树中， 每一颗以该节点为根节点的树的深度，这样最终就可以得到整棵树根节点的左右子树的深度
- 比如：递归到某叶子节点时候，他的左右子树深度均为0，则叶子节点这棵树的深度为1

下面是伪代码：

```go
1. return 0 if root is null                 // return 0 for null node
2. left_depth = maximum_depth(root.left)
3. right_depth = maximum_depth(root.right)
4. return max(left_depth, right_depth) + 1	// return depth of the subtree rooted at root
```

Go代码模板：

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



#### 自底向上树的深度图例

下面是 **自底向上**计算整棵树的深度的步骤：

1. **A**为根节点的子树深度为1
2. B左子树深度为1 ， B右子树深度为2  =》B为根节点的树深度为 Max(Left， Right) + 1 = 3
3. 同理计算：**E**根节点树高度 = Max(E.Left, E.Right) + 1 = 3+1 = 4

<img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507162225804.png" alt="image-20220507162225804" style="zoom: 25%;" /> <img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507162248844.png" alt="image-20220507162248844" style="zoom:25%;" /> 

<img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507162804778.png" alt="image-20220507162804778" style="zoom: 25%;" />

<img src="pic/%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92.assets/image-20220507162924494.png" alt="image-20220507162924494" style="zoom:25%;" />

 















