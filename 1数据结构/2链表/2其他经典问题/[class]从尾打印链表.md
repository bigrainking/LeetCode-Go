





# 题解

### 法一：原地旋转数组

**方法：**

- 遍历链表，将元素加入数组
- 原地旋转数组

**复杂度：**

遍历链表、旋转数组共O(n)

申请数组额外空间O(n)

**关键点**：

- 无法确定申请多大的数组：申请一个空切片 `arr := []int{}`， 后面用append函数追加元素
- 原地旋转数组时 写法 `i++, j++`不能通过编译，正确写法是`i,  j = i+1, j-1`

```go
func reversePrint(head *ListNode) []int {
    arr := make([]int,0,10000)
    // 顺序遍历链表 
    index := 0
    for head != nil {
        arr = append(arr, head.Val)
        head = head.Next
        index++
    }
	 //原地旋转数组
    for i, j := 0, len(arr)-1; i < j; i, j = i+1, j-1 {
        arr[i], arr[j] = arr[j], arr[i]
    }
    return arr
}
```

