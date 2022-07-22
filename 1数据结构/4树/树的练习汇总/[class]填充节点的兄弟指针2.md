# [题目](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

<img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516210402124.png" alt="image-20220516210402124" style="zoom: 50%;" /> <img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516210413496.png" alt="image-20220516210413496" style="zoom: 50%;" />

# 题解



### **层次遍历**

```go
func connect(root *Node) *Node {
	// 层次遍历
    if root == nil {
        return nil
    }
    queue, count := [](*Node){root}, 1
    for len(queue) != 0 {
        nowCount := count
        count = 0
        for i := 1; i <= nowCount; i++ {
            node :=  queue[0]
            queue = queue[1:]
            if i != nowCount {
                node.Next = queue[0]
            }
            if node.Left != nil {
                queue = append(queue, node.Left)
                count++
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
                count++
            }
        }
    }
    return root
}
```



### 层次遍历：不使用额外空间

上面的方法需要使用额外的队列。我们可以不使用队列，

将每一行看做一个链表，通过当前行的链表，将下一行串联起来。

一、**下面是链表**

<img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516112658846.png" alt="image-20220516112658846" style="zoom:33%;" />

二、**如何用当前链表，将下面链表串联起来？**

- 设置一个dummy节点，作为下一层的起始节点；pre为上一次已经连接的节点

- 分为3种情况：

   1. cur有左子树：`pre.next = cur.left`
   2. cur有右子树：`pre.next = cur.right`

   3. cur均无左右子树：`cur = cur.next` , 此时cur会移动到下一个节点，并将下一个节点的左右子树赋给`pre.next`

- `cur.next`为空：则`pre.next = nil`并继续连接下一层

<img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516113604163.png" alt="image-20220516113604163" style="zoom: 25%;" /><img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516113614729.png" alt="image-20220516113614729" style="zoom: 25%;" />

<img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516113642353.png" alt="image-20220516113642353" style="zoom:25%;" /> <img src="pic/%5Bclass%5D%E6%B2%A1%E5%81%9A%E5%AE%8C%E5%A1%AB%E5%85%85%E8%8A%82%E7%82%B9%E7%9A%84%E5%85%84%E5%BC%9F%E6%8C%87%E9%92%882.assets/image-20220516113656754.png" alt="image-20220516113656754" style="zoom:25%;" />

三、**跳到下一层时，如何找到当前层的第一个节点？**

比如上图中我们继续连接下一层，cur层就是第三层，那么如何找到cur层的第一个节点呢？

通过哑巴节点.next找到`cur = dummy.next`

**四、如何知道已经遍历完整棵树**

第三步骤中，如果找到的当前层第一个节点为空，则该层一个节点也没有。则整棵树已经遍历完毕，结束运行

**伪代码**：

```go
cur = root
for {
   if cur == nil {
      该层一个元素也没有，已经连接完毕所有数据
   }
   for cur != nil {
      pre = dummy
      if cur.left != nil {
         pre.next = cur.left
         pre = pre.next
      }
      if cur.right != nil {
         pre.next = cur.right
         pre = pre.next
      }
      cur = cur.next
   }
   cur = dummy.next //为下一轮准备
}
```

**Go代码**：

```go
func connect(root *Node) *Node {
    cur := root
    for cur != nil {
        dummy := new(Node)
        pre := dummy
        for cur != nil {
            if cur.Left != nil {
                pre.Next = cur.Left
                pre = pre.Next
            }
            if cur.Right != nil {
                pre.Next = cur.Right
                pre = pre.Next
            }
            cur = cur.Next
        }
        cur = dummy.Next
    }
    return root
}
```

