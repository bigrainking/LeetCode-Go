[toc]



# 一、题目

[205. 同构字符串easy](https://leetcode.cn/problems/isomorphic-strings/)

战绩:dagger:已OC 需要修改代码的简洁渡

<img src="pic/%5Bclass%5D%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%90%8C%E6%9E%84.assets/image-20220608101951953.png" alt="image-20220608101951953" style="zoom: 33%;" /> <img src="pic/%5Bclass%5D%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%90%8C%E6%9E%84.assets/image-20220608102003642.png" alt="image-20220608102003642" style="zoom: 33%;" />



# 二、解析

## 哈希表法: 双映射



### **1. 解析**

【双射关系】 题目要求的是 串S 中的字符是否与 串T 中的字符串一一对应。 

egg对应add g对应d, 

而babe对应pdpd就不是一一对应。 虽然a-d, e-d是一一对应，但反过来pdpd中的d-a,d-e就是一个字母有两个不同的映射。不合条件。

因此，需要建立两个哈希表，来保存两个映射



### 2. **实现**

#### 代码易理解版

```go
func isIsomorphic(s string, t string) bool {
    hashTable1 := map[byte]byte{}
    hashTable2 := map[byte]byte{}
    for i := 0; i < len(s); i++ {
        // 当前要存入的键值对已经存在
        if tvalue, ok := hashTable1[s[i]]; ok { //Table1是否一一映射
            if tvalue == t[i] {
                continue
            }else {
                return false
            }
        }
        if tvalue, ok := hashTable2[t[i]]; ok {//Table2是否一一映射
            if tvalue == s[i] {
                continue
            }else {
                return false
            }
        }
        hashTable1[s[i]] = t[i]
        hashTable2[t[i]] = s[i]
    }
    return true
}
```



#### 代码简洁版

`hashTable1[s[i]] > 0` 表示元素存在（value是byte类型元素，value要么没有要么用于>0）

如果value是int类型，则还是最好用上面的 `value,ok := hash[key]`来判断key-value 是否存在

```go
func isIsomorphic(s string, t string) bool {
    // 代码简单
    hashTable1, hashTable2 := map[byte]byte{}, map[byte]byte{}
    for i := 0; i < len(s); i++ {
        // 保证双向的映射都是唯一的
        if hashTable1[s[i]] > 0 && hashTable1[s[i]] != t[i] || hashTable2[t[i]] > 0 && hashTable2[t[i]] != s[i] {
            return false
        }
        hashTable1[s[i]] = t[i]
        hashTable2[t[i]] = s[i]
    }
    return true
}
```











