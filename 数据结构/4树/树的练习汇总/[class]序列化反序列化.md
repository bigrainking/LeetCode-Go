

# 一、题目

[题目](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)

【难】

输入一个树的根节点

serialize：输出他的序列化结果

deserialize: 根据序列化结果输出一棵树



<img src="pic/%5Bclass%5D%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96.assets/image-20220521122626496.png" alt="image-20220521122626496" style="zoom:50%;" />

<img src="pic/%5Bclass%5D%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96.assets/image-20220521122638475.png" alt="image-20220521122638475" style="zoom:33%;" />

# 二、题解

## 1. 递归解法

(参考解析)[https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/]

递归遍历一棵树，重点关注当前节点，它的子树的遍历交给递归完成：
**“serialize函数，请帮我分别序列化我的左右子树，我等你返回的结果，再拼接一下。”**



#### **先序遍历构建序列化**

```go
func (this *Codec) serialize(root *TreeNode) string {
	if root == nil {
		return "X"
	}
	return strconv.Itoa(root.Val) + "," + this.serialize(root.Left) + "," + this.serialize(root.Right)
}
```



<img src="pic/%5Bclass%5D%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96.assets/image-20220521120104596.png" alt="image-20220521120104596" style="zoom:33%;" />

**对上图的理解**：

比如当递归到2XX这棵子树时，返回的内容为什么是2XX呢？

```go
serialize(2) {
	return 2 + "," + X + "," + "X"  // == 2XX
}
```



#### 反序列化



- 定义函数 buildTree 用于还原二叉树，传入由序列化字符串转成的 list 数组。

- 逐个 pop 出 list 的首项，构建当前子树的根节点，顺着 list，构建顺序是根节点，左子树，右子树。
   如果弹出的字符为 "X"，则返回 null 节点。
   如果弹出的字符是数值，则创建root节点，并递归构建root的左右子树，最后返回root

<img src="pic/%5Bclass%5D%E5%BA%8F%E5%88%97%E5%8C%96%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96.assets/image-20220521121755657.png" alt="image-20220521121755657" style="zoom: 33%;" />

上图中：

由于采用的先序遍历，每次递归pop()之后，剩下的第一个节点为根节点

第一层递归时， 1是root

第二层递归时，2是root

```go
func buildTree(list *[]string) *TreeNode {
	rootVal := (*list)[0]
	*list = (*list)[1:] //每次出队
	if rootVal == "X" {
		return nil
	}
	Val, _ := strconv.Atoi(rootVal) 
	root := &TreeNode{Val: Val} //构建好根节点
	root.Left = buildTree(list) //继续向左
	root.Right = buildTree(list) //继续向右
	return root //返回创建好的树
}

func (this *Codec) deserialize(data string) *TreeNode {
	list := strings.Split(data, ",")
	return buildTree(&list)
}
```



## 我的解法

> 解法有问题， 先学习别人的解法

#### 2.1 树转换成字符串



1. 层次遍历所有节点
2. 遇到非空节点加入node.Val, 遇到空的子节点加入 ".null"
3. 最后去掉字符串尾部的所有的",null"

#### Trim函数

作用：去掉字符串首尾的所有相同字符串

```go
newStr = strings.Trim(str, cutset) //str原来的字符串, cutset需要去掉的内容

str = "!!hello, world !!!!"
newStr := strings.Trim(str, "!")
fmt.Pritln(newStr)
==============
"hello, world "
```



#### Go实现

```go
func (this *Codec) serialize(root *TreeNode) string {
    // 0. 根节点为空
    if root == nil {
        return "[]"
    }
    // 1、 根节点入队， 加入结果
    res := "["
    queue := [](*TreeNode){root}
    res += fmt.Sprint(root.Val)

    // 2.层次遍历填充字符串
    for len(queue) != 0 {
        node := queue[0]
        queue = queue[1:]
        if node.Left != nil {
            queue= append(queue, node.Left)
            res += fmt.Sprintf("%s%d", ",", node.Left.Val)
        }else {
            res += ",null"
        }
        if node.Right != nil {
            queue = append(queue, node.Right) 
            res += fmt.Sprintf("%s%d", ",", node.Right.Val)
        }else {
            res += ",null"
        }
    }
    // 3. 转换
    res = strings.Trim(res, ",null") //去除叶子节点添加的null
    res += "]"
    return res
}
```



#### 2.2 字符串转树

中序遍历创建树



#### 字符串分割成字符数组

```go
    res := strings.TrimSpace(info)
    //把字符串以空格分割成字符串数组
    str_arr := strings.Split(res, " ")
    //计算字符串数组的长度
    count := len(str_arr)
```



