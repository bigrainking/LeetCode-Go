

<img src="pic/%5Bclass%5D%E5%88%A0%E9%99%A4%E5%80%92%E6%95%B0%E7%AC%ACn%E4%B8%AA%E8%8A%82%E7%82%B9.assets/image-20220402171946917.png" alt="image-20220402171946917" style="zoom: 50%;" />

# 题解：

## 方法一：双指针

> 我想出来的

想要仅遍历一次链表就删除。

- 需要找到倒数第n节点前面的节点
- 设置`slow, fast` 指针， **`slow`永远保持距离`fast` n 步**（如保持2步：`slow.Next.Next == fast`）。
- 当`fast`到达末尾节点（如下图的 5 号节点）， `slow`就是倒数第`n+1`个（如下图的 3 号节点）。 删除第n个节点

<img src="pic/%5Bclass%5D%E5%88%A0%E9%99%A4%E5%80%92%E6%95%B0%E7%AC%ACn%E4%B8%AA%E8%8A%82%E7%82%B9.assets/image-20220402172537412.png" alt="image-20220402172537412" style="zoom: 33%;" />

- Tips : 为了让删除`head`节点与普通的其他节点操作一致，设置一个`dummy`节点。

   返回的时候返回新的头节点： `dummy.Next`

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    // slow一直保持距离fast指针n步, 当fast到达尾结点，slow在倒数第n+1节点(以便去掉第n个节点)

    // 方便删除头节点
    var dummy *ListNode = &ListNode{Next:head}

    slow, fast := dummy,dummy
    for i := 0; i < n; i++ {
        fast = fast.Next
    }

    for fast.Next != nil {
        slow, fast = slow.Next, fast.Next
    }

    slow.Next = slow.Next.Next //删除
    return dummy.Next
}
```

