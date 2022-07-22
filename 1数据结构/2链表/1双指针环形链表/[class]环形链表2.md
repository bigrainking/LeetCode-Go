

# [题目](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

返回环形链表的入口节点。如果没有环则返回null

<img src="pic/%5Bclass%5D%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A82.assets/image-20220401182459563.png" alt="image-20220401182459563" style="zoom:50%;" />

<img src="pic/%5Bclass%5D%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A82.assets/image-20220401182509997.png" alt="image-20220401182509997" style="zoom:50%;" />

# 解析

> 本题没做起

## 方法一：双指针

[参考题解](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/linked-list-cycle-ii-kuai-man-zhi-zhen-shuang-zhi-/)

> 通过计算得到入口起点的位置

<img src="pic/%5Bclass%5D%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A82.assets/image-20220401180846523.png" alt="image-20220401180846523" style="zoom: 33%;" />





**第一次相遇时**：

> 同时这一步需要判断是否没有环

- 有fast slow指针, fast每次移动2格，slow移动一格

- slow指针移动s步， fast指针移动2s步【相遇节点是s步】 （注意是向前移动多少步5-->3是一步）
- 由于fast是追上的slow，因此fast比slow多走了n圈 共n*b步
- 有  2s = s + nb  ==>  s = nb【相遇节点是nb步】



**第二次相遇**：

- 首先需要知道走到入口节点 `1`的条件是：移动 a + nb （移动a步到入口节点，再移动n圈会回到入口）

- 构造第二次相遇：

   - slow节点从上次相遇节点（nb）出发，fast从新从起点出发， 两者均每次移动一步
   - fast 移动a步到入口(a)， slow移动a步到入口(slow : a+nb)

   - 此时相遇的节点就是入口节点， 返回该节点

```go
func detectCycle(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return nil
    }

    // 第一次相遇，确定是否有环
    slow, fast := head, head
    for {
        if fast == nil || fast.Next == nil { // 无环
            return nil
        }
        fast, slow = fast.Next.Next, slow.Next
        if slow == fast {  // 以此来避免head处的相遇
            break
        }
    }

    // 构造第二次相遇
    fast = head // fast从起点再次出发，low还在原地出发
    for slow != fast {
        fast,slow = fast.Next, slow.Next
    }
    return fast
}
```

