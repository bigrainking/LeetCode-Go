







# [填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

<img src="pic/%5Bclass%5D%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%88.assets/image-20220511142300057.png" alt="image-20220511142300057" style="zoom: 33%;" />



<img src="pic/%5Bclass%5D%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%88.assets/image-20220511142328798.png" alt="image-20220511142328798" style="zoom: 33%;" />



### 1. **层次遍历解法**

- 额外空间:借用了一个队列 : 空间复杂度 O(n)



1.1 **思路**：

- 按照层次遍历，将每层的节点加入队列

- 每次Pop队头节点时，都将Pop节点指向下一个top节点。

- 如果已经是每层结尾，则不需要指向下一个节点



1.2 **算法伪代码**：

```go
//每一层从第一个节点依次指向下一个节点
queue{root}
count = 1
for 【队不空】 {
   nowcount = count 
   count = 0
   for count次 {//遍历本层
      node = dequeue()
      if i == count {
         node.Next = null
      }else {
         node.Next = top()
      }
      if node有孩子 {
         node左右孩子入栈
      }
   }
}
return root
```



1.3 **Go代码**

```go
func connect(root *Node) *Node {
    if root == nil {
        return nil
    }
    queue, count := [](*Node){root}, 1
    for len(queue) != 0 {
        nowCount := count //本层的节点个数
        count = 0
        for i := 1; i <= nowCount; i++ {
            node := queue[0]
            queue = queue[1:]
            if i != nowCount { //非最后一个节点
                node.Next = queue[0]
            // 孩子节点入栈
            if node.Left != nil { //完美二叉树，如果有左孩子则一定不是叶节点
                queue = append(queue, node.Left)
                queue = append(queue, node.Right)
                count += 2
            }
        }
    }
    return root
}
```



### 2. 不用额外空间法



- 直接在原来的树上操作，空间复杂度为O(1)

[题解来源于官方题解:没做出来](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/solution/tian-chong-mei-ge-jie-dian-de-xia-yi-ge-you-ce-2-4/)

#### 2.1 思路

1. root 初始为N-1层

2. 利用N-1层已经建立好的next指针，

3. 为N层建立好指针,

   ```go
   head = leftmost
   while (head != nil) {
      1)同一棵树连接节点
      2)非同一棵树链接节点
      head = head.Next
   }
   ```

3. 如果N+1还有节点则重复1,2步； 如果没有则退出

   ```go
   leftmost = 上一层leftmost.Next
   if leftmost == nil break
   ```

   

其中第二步骤分为两种情况：

- 第一种情况直接利用father节点

   <img src="pic/%5Bclass%5D%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%88.assets/image-20220511115243170.png" alt="image-20220511115243170" style="zoom: 33%;" />

- 第二种需要利用上一层的next指针

   <img src="pic/%5Bclass%5D%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%88.assets/image-20220511115258376.png" alt="image-20220511115258376" style="zoom:33%;" />

```go
1)同一棵树： head.left.next = head.right
2)非同一个树：if head.next != nil { 
   head.right.next = head.next.left }
head = head.Next
```



伪代码：

```go
leftmost = root
for leftmost.left != nil { //探究即将填充的N层是否存在
   head = leftmost //N-1层
   while (head != nil) { //N 层赋值
      1）同一棵树： head.left.next = head.right
      2)非同一个树：if head.next != nil {head.right.next = head.next.left}
      head = head.next
   }
   leftmost = leftmost.Left // N层又变成下一波的N-1层
}
```



####  2.2 Go代码

```go
func connect(root *Node) *Node {
 if root == nil {
        return nil
    }
    leftmost := root
    for leftmost.Left != nil { //等待填充的N层存在
        head := leftmost 
        for head != nil {
            head.Left.Next = head.Right
            if head.Next != nil { //如果head已经是本层最后一个节点则next为nil
                head.Right.Next = head.Next.Left
            }
            head = head.Next
        }
        leftmost = leftmost.Left //leftmost为下一层做准备
    }
    return root
}
```







































