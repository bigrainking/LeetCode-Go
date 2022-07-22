



# 一、题目

[midum](https://leetcode.cn/problems/valid-sudoku/)

【NO】看的答案，自己写了代码

# 二、解析



## (一)9+9+9个哈希表法



##### 1）思路

1. 对应每一行、每一列、每个三宫格 都创建一个map

  key=number由于number=>1-9,用数组来代替map

2. 其中

     `rows[i][number] = times` 表示i行某个number出现了times次

     `columns[j][number] = times` 表示j列某个number出现了times次

     `subboxs[i/3][j/3][number] = times` 一共9个小宫格,第i/3行第j/3列个宫格 中 的hashMpa中 number出现了times次

3. 遍历整个数组，遇到一个数字，分别将他对应的行HashMap,列hashMap，宫格hashMap添加一个数字



##### 2）实现

- 由于原数组是byte类型的数组和空格，因此

   遇到空格跳过不操作

   遇到数字将byte类型 => int类型（byte存储的字符集中的编号）

```go
func isValidSudoku(board [][]byte) bool {
    // 1. 数字1-9， 第二个位置表示number：10 => 0-9
    rows, columns := [9][10]int{}, [9][10]int{}
    subboxs := [3][3][10]int{}
    // 2. 遍历数组
    for i, row := range board {
        for j, num := range row {
            // 空格不用管
            if num == '.' {
                continue
            }
            number := num - '1' //将byte换算成数值
            
            // 判断某元素是否出现过>1次
            if (rows[i][number] > 0) || (columns[j][number] > 0) || (subboxs[i/3][j/3][number] > 0) {
                return false
            }
            // 没有出现过，则找打对应的行列宫格map中对应的number次数++
            rows[i][number]++
            columns[j][number]++
            subboxs[i/3][j/3][number]++
        }
    }
    return true
}
```

