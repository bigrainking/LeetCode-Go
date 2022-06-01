## 栈



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



1. **前提**

   - 理解前中序遍历

   - 知道后序遍历思想

2. **非递归后序遍历思想**：
   1. 当前节点入栈， 
   2. 如果有左子树则将左子树入栈，否则将右子树入栈
   3. 如果当前节点无左右子树，则访问它
   4. 访问了当前节点之后，进入父亲节点右子树并访问
   5. 当左右子树均访问完，再访问父亲节点

3. **非递归后序遍历难点**：

- 对于每个节点我们有**三次**会路过它：
   1. 节点入栈
   2. 左子树访问完毕后，访问父亲节点，以便进入右子树
   3. 右子树访问完毕，弹出父亲节点，并将父亲节点加入result行列

- 那么如何确定此时左右子树访问完毕可以加入result行列呢？

**解决办法**：

- 使用双栈记录节点，每个栈内的节点同时带一个flag记录他们被访问的状态。
- 节点初始进入flag = false， 而当我们从Node的左子树访问完毕准备进入右子树时，root.flag = true

> 为什么要在此时设置？
> 我们一共有三次会经过根节点
>
>     1. root压入栈，并进入左子树。 
>     2. 左子树访问完毕，Top(root)进入右子树 
>     3. 右子树访问完毕，Pop(root), 并visit(root)
>        因此，当从右子树回来时，右子树已经被访问完， 通过判断root.flag = true 我们直接visit(root)

伪代码：

```go
func postorderTraversal(root *TreeNode) (res []int) {
    for 【栈不空】or 【node != nil】 {
        // 一路将左子树全部压入栈
        while 【node != null】 {
            Push(node, F)
            node = node.left
        }
        node = stack.top()

       //节点的左右子树均已经被访问完
        if node.flag == true { 
            Visit(node)
            Pop(node)
            node = null

       //当节点是从左子树访问完， 进入右子树，则将flag设置为true
        }else { 
            node.flag = true //进入右子树之前设置为true
            node = node.right
        }
        
    }
}
```

复杂度分析

- 时间复杂度：
   遍历一遍树： `O(N)`
- 空间复杂度：
   借用双栈 `2*N` -> `O(N)`

Go：

```go
func postorderTraversal(root *TreeNode) (res []int) {
    // 双栈准备
    stackNode := [](*TreeNode){}
    stackFlag := []bool{}
    node := root
    for len(stackNode) > 0 || node != nil {
        // 一路将左子树全部压入栈
        for node != nil {
            // fmt.Print(node.Val)
            stackNode = append(stackNode, node)
            stackFlag = append(stackFlag, false)
            node = node.Left
        }
        node = stackNode[len(stackNode) - 1]
        //如果根节点的左右子树均已经被访问完
        if stackFlag[len(stackFlag) - 1] == true {
            res = append(res, node.Val)
            stackNode = stackNode[:len(stackNode) - 1] //出栈
            stackFlag = stackFlag[:len(stackFlag) - 1]
            node = nil
        }else { //当节点是从左子树访问完， 进入右子树，则将flag设置为true
            stackFlag[len(stackFlag) - 1] = true
            node = node.Right
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





## 二、应用

### 2.1 求根节点到某个子节点的路径

**方法：**

用非递归后序遍历，当遇到目标节点时，此时栈内的节点：从栈底到栈顶的节点为 根节点到目标节点的路径

如下图中的q为目标节点

<img src="pic/%5Bclass%5D1%E9%80%92%E5%BD%92%E6%B3%95(%E5%89%8D%E4%B8%AD%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86).assets/image-20220518100058039.png" alt="image-20220518100058039" style="zoom: 25%;" />

**详解**：

当访问到不是目标节点，将会弹出，然后继续按照后序遍历的方法访问其他节点。上图为了寻找到q节点的路径，其过程为

栈内元素：35

​		356  pop(6):6不是目标节点

​		352

​		3527 pop(7):7不是目标节点

​		3524 4是目标节点，停止寻找

寻找根到1的路径：

​		35

​		356  pop(6):6不是目标节点

​		352

​		3527 pop(7):7不是目标节点

​		3524 pop(4):4不是目标节点

​		352 pop(2) 2的左右子树已经被访问完

​		35

​		3

​		31 



**伪代码**

```go
// Input: 目标节点； 装路径的栈
var search func(aim *TreeNode, stack [](*TreeNode)) {
   for 【栈不空】|| 【node != nil】 {
      // 1. 一路左节点压入栈
      while node != nil {
         Push(node)
         node.flag = false
         if node == p {return}// 找到目标节点
         node = node.left
      }
      node = top()
      // 3. 左右子树均已被访问，访问并弹出根节点
      if node左右子树均已被访问:node.flag == true {
         pop(node)
         node = nil
         // 2. 继续访问右子树
      }else {
         node.flag = true //进入右子树之前设置为true
         node = node.Right //等后面for循环的时候入栈
      }
   }
}
```



