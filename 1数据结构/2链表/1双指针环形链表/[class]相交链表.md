







# 题解



## 方法一：双指针分别移动

> 我想出来的

- 为了让两个指针同时到达交点，去掉长链表的多出来的部分（相比短链表）
- 两个指针每次同时前进一格，如果没有相遇则null，否则一定会同时到达交点

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    // 长链表去掉比短链链表长的那部分
    pA, pB := headA, headB
    lenA, lenB := Len(pA, 0), Len(pB, 0)

    pA, pB = headA, headB
    if lenB > lenA {
        pB = Move(pB, lenB - lenA)
    }else {
        pA = Move(pA, lenA - lenB)// A移动
    }

    // 开始找相交节点
    for pA != pB {
        if pA == nil || pB == nil {
            return nil
        }
        pA, pB = pA.Next, pB.Next
    }
    return pA
}

func Move(p *ListNode, len int) *ListNode {
    for i := 0; i < len; i++ {
        p = p.Next
    }
    return p
}

func Len(p *ListNode, len int) int {
    for p != nil {
        len++
        p = p.Next
    }
    return len
}

```



## 方法二：双指针同时移动

[参考链接](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/solution/intersection-of-two-linked-lists-shuang-zhi-zhen-l/)

- 类似 环形链表2

<img src="pic/%5Bclass%5D%E7%9B%B8%E4%BA%A4%E9%93%BE%E8%A1%A8.assets/image-20220402161055659.png" alt="image-20220402161055659" style="zoom:50%;" />

- 设置 `pA = headA, pB = headB`  

- 首先：有`pA`遍历完2个链表，与`pB`遍历完2个链表，走过的步数一样。(重合部分均只遍历一次)。 

- 那么`pA`与`pB`将会同时到达`某个点`，要想同时到达的是首个公共节点，走的方法如下：

   pA：先走 `a-c` , 再走 `c`, 再走 `b-c`

   pB：先走`b-c`，再走 `c`, 再走`a-c`

- 如果没有公共节点，则同时到达对方链表的末尾 

**代码：**

- `for pa != pb {` 如果没有公共节点，则同时到达对方链表的末尾 pa == pb 退出循环

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    pa, pb := headA, headB
    for pa != pb { //pa, pb同时行动
        if pa != nil { //遍历A链表
            pa = pa.Next
        }else { //遍历对方链表的交点前面部分
            pa = headB
        }
        if pb != nil {
            pb = pb.Next
        }else {
            pb = headA
        }
    }
    return pa
}
```







