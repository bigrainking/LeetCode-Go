

[题目]()

给一个字符串，判断其中所有括号是否对称。 

字符串只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s`

<img src="pic/%5Bclass%5D%E6%8B%AC%E5%8F%B7%E5%8C%B9%E9%85%8D.assets/image-20220422161932717.png" alt="image-20220422161932717" style="zoom: 50%;" />



## 题解

- 先把没有匹配的元素压入栈，

- 如果遇到栈顶元素和新的元素匹配则将栈顶元素出栈。 
   - 这样一旦有元素与栈顶元素不匹配则出现不匹配括号，
   - or 出现右边括号栈内没有元素，也不匹配。

- 排除只有单数个元素的字符串
- 排除字符串只有左括号





左括号：遇到 `([{`   压入栈

遇到右括号：

- 栈如果是空在false
- 查看是否与栈顶元素对称， 对称则出栈。否则返回false



```go
func isValid(s string) bool {
    // 排除只有单数个元素
    lens := len(s)
    if lens % 2 != 0 {
        return false
    }

    pairs := map[byte]byte{
        ')':'(',
        ']':'[',
        '}':'{',
    }
    stack := []byte{}
    // 遍历字符串
    for i := 0; i < lens; i++ {
        if pairs[s[i]] == 0 { //左括号:压入栈 (pairs中不存在左括号)
            stack = append(stack, s[i])
        }else { //右括号:找栈顶是否有符合条件的对象
            if len(stack) == 0 || pairs[s[i]] != stack[len(stack) - 1] { //栈顶元素与当前元素的对应左括号不一致
                return false
            }else { //匹配则出栈
                stack = stack[:len(stack) - 1]
            }
        }
    }
    return len(stack) == 0
}
```



