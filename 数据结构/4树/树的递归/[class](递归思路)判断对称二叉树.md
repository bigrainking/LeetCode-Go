





# [题目](https://leetcode.cn/problems/symmetric-tree/solution/)

<img src="pic/%5Bclass%5D%E5%88%A4%E6%96%AD%E4%BA%8C%E5%8F%89%E6%A0%91.assets/image-20220510110336731.png" alt="image-20220510110336731" style="zoom:33%;" />

# 题解

> 下面是思考如何写出递归的方法

**如何写出递归**

如何知道一棵树是否为对称二叉树：

1. 如果root的左树 与 root右树 对称则对称

2. 如何判断左树与右树是否对称？

3. p.Val == q.Val

   p的左树 与 q的右树 对称且 

   p的右树 与 q的左树  对称，则左树与右树对称

4. 这时我们就需要一个函数F，他的功能是判断两个节点的子树是否对称：

   F(p, q) { 

   p ?= q &&

   check(p.左树, q.右树) &&

   check(p.左树, q.右树) 

   }

```go
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    var check func(p, q *TreeNode) bool
    check = func(p, q *TreeNode) bool { //判断p q两颗树是否对称
        if p == nil && q == nil { //如果两个节点都是空的， 则不用比较值
            return true
        }
        if p == nil || q == nil { //如果只有一方为nil也不用比较val，直接不对称（其中双方是nil的情况已经被排除）
            return false 
        }
        // 同时满足，两个子树的根节点相同； p左与q右对称； p右与q左对称
        return p.Val == q.Val && check(p.Left, q.Right) && check(p.Right, q.Left)
    }
    return check(root.Left, root.Right)
}
```



