# [题目](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

<img src="pic/%5Bclass%5D%E6%9C%80%E5%B0%8F%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.assets/image-20220520125423326.png" alt="image-20220520125423326" style="zoom: 50%;" /> 

<img src="pic/%5Bclass%5D%E6%9C%80%E5%B0%8F%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.assets/image-20220520125438923.png" alt="image-20220520125438923" style="zoom:50%;" />



# 题解

## 方法一：比较路径

#### **1. 题解**：

找根节点到两个目标节点的路径， 取两个路径上最后一个相同的节点

<img src="pic/%5Bclass%5D%E6%9C%80%E5%B0%8F%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.assets/image-20220516214501437.png" alt="image-20220516214501437" style="zoom:33%;" />



#### **2. 算法实现**

- **求根节点到目标节点路径**：

   用非递归后序遍历，当遍历到该节点时，栈内所有节点是从根节点到该节点的路径

- **比较路径找公共节点**：

   取出栈内节点，从根节点开始比较，最后一个相同的节点就是最近公共祖先



#### **3. 伪代码**

```go
    if p,q == root return root
    // 找到p,q的路径的栈
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
    // 比较路径找到最近公共祖先
    while {
       	栈内元素全部出栈，并进入数组；
       	从数组尾开始比较；（出栈后叶节点在数组首）
    }
```



#### **Go代码**

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    if p == root || q == root {
        return root
    }
    // 2. 找路径
    // pSta, qSta := [](*TreeNode){}, [](*TreeNode){}
    var search func(aim *TreeNode) [](*TreeNode)
    search = func(aim *TreeNode) [](*TreeNode){
        stack, fstack := [](*TreeNode){}, []bool{}
        node := root
        for len(stack) > 0 || node != nil {
            for node != nil {
                stack, fstack = append(stack, node), append(fstack, false)
                if node == aim {
                    return stack
                }
                node = node.Left
            }
            node = stack[len(stack) - 1]
            if fstack[len(fstack) - 1] == true {
                stack, fstack = stack[:len(stack) - 1], fstack[:len(fstack) - 1]
                node = nil
            }else {
                fstack[len(fstack) - 1] = true
                node = node.Right
            }
        }
        return nil
    }
    pSta, qSta := search(p), search(q)
    // 3.找公共节点
    return Judge(pSta, qSta)

}
// 判公共祖先
func Judge(pSta, qSta[](*TreeNode)) *TreeNode{
      // 将栈内元素转移到数组
     pArr, qArr := [](*TreeNode){}, [](*TreeNode){} 
    pLen, qLen := len(pSta), len(qSta)
    for i := 0; i < pLen; i++ {
        pArr = append(pArr, pSta[len(pSta) - 1])
        pSta = pSta[:len(pSta) - 1]
    }
    for i := 0; i < qLen; i++ {
        qArr = append(qArr, qSta[len(qSta) - 1])
        qSta = qSta[:len(qSta) - 1]
    }
    // 数组是从叶节点到根节点的顺序存储的
    i, j := len(pArr) - 1 , len(qArr) - 1
    for ; i>=0 && j >= 0; i, j = i-1, j-1{
        if pArr[i] != qArr[j] {
            return pArr[i+1]
        }
    }
    if i == -1 {
        return pArr[0]
    }
    if j == -1 {
        return qArr[0]
    }
    return nil
}
```



#### 参考

[参考资料](https://www.bilibili.com/video/BV12Z4y15721?spm_id_from=333.337.search-card.all.click)

## 方法二:递归解法



因为是递归，使用函数后可认为左右子树已经算出结果

#### **2.1 思路**：

**分三种情况**：

1. root为空，return nil
2. p, q在node节点的左右两侧
3. node = p 或者 q, 另一个节点在node某一边子树中

#### 2.2 递归实现：

> 因为递归：
>
> 最重要：**假设已经知道node左右子树是否含有p, q节点**

1）root节点为nil

2) root = q或者p， root是最近公共祖先

3）递归遍历root的左右子树，因为是递归，假设已经知道left,right的情况，左右子树的结果用left，right表示

​	4）left，right均不为空，则root是最近公共祖先

​	5）left为空，right不空，则进入right子树；相反的，进入left子树

​	6）left,right均为空，返回nil

#### 2.4 如何写递归代码？

> 下面的思考，建立在假设p,q都在root子树内

**我们这样思考**

    1. Q： 如何知道root是否为最近公共祖先？
        
    Answer： 
    
    - p,q分别在root的左右子树则root是最近公共祖先
    - 如果p, q均在同一颗子树，则去那颗子树里面寻找
    
2. Q: 如何知道p是否在子树？

      先序遍历root左子树，如果左子树中某个node == p 则在左子树里面

3. 因此：
- 需要一个func实现下面的功能：判断某节点是否在子树内，如果在则返回true

    - 另一个func实现：判断root是否为p,q的公共祖先

4. 对于func1直接用先序遍历的递归代码

   对于func2 ： 

   ```go
   func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
      if root == nil {return nil}
      if root == p || root == q {return root}
      1. p,q在左子树:则进入左子树继续寻找
      if search(root.left, p) && search(root.left,q){lowestCommonAncestor(root.left, p, q *TreeNode)}
      2. p,q在右子树:则进入右子树继续寻找
      if search(root.right, p) && search(root.right,q){lowestCommonAncestor(root.right, p, q *TreeNode)}
      3. p,q在分别左右子树
      return root
      
      func search(root, aim) bool {
         if root == nil {return false}
         if root == aim {return true}
         return seach(root.left, p) || seach(root.left, p)
      }
   }
   ```

   

#### 2.3 Go实现

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    var search func(root , aim *TreeNode) bool 
    search = func (root , aim *TreeNode) bool {
        if root == nil {return false}
        if root == aim {return true}
        return search(root.Left, aim) || search(root.Right, aim)
    }
   if root == nil {
       return nil
    }
   if root == p || root == q {
       return root
    }
//    1. p,q在左子树:则进入左子树继续寻找
   if search(root.Left, p) && search(root.Left,q){
       return lowestCommonAncestor(root.Left, p, q)
    }
//    2. p,q在右子树:则进入右子树继续寻找
   if search(root.Right, p) && search(root.Right,q){
       return lowestCommonAncestor(root.Right, p, q)
    }
//    3. p,q在分别左右子树
   return root
}
```





#### 2.4 参考资料

[二叉树的最近公共祖先（DFS ，清晰图解](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solution/c-jing-dian-di-gui-si-lu-fei-chang-hao-li-jie-shi-/)

[【C++ 经典递归】思路非常好理解](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solution/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/)



