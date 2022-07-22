



# 情况

[左程云视频二分法对应时间1:31](https://www.bilibili.com/video/BV1Ef4y1T7Qi?p=2&vd_source=47272764e1eb400edc65776bfe6a48af)



> 1. 在有序数组中，找符合条件的某个数字
> 2. 在有序数组中，找满足>=target的最左侧的位置
> 3. 在有序数组中，找满足<=target的最右侧的位置
> 4. 局部最小值





# 对应代码

## 1: 直接找符合条件的数字

针对Leetbook的模板一，`arr[index] == target`可以直接获得符合条件的元素

```go
func search(nums []int, target int) int {
   l, r := 0, len(nums)-1
   for l <= r { //为甚要等于:查找范围不为空
      //mid := left + (right - left) / 2 //防止left+right会越界
      mid := left + （(right left) >> 1） // 移位 复杂度低
      if target == nums[mid] {
         return mid //一定要有找到后退出条件
      }else if target < nums[mid] {
         r = mid - 1
      }else {
         l = mid + 1
      }
   }
   return -1
}
```



## 2: 找满足>=target的最左侧的位置

**步骤**

1. 二分
2. 当前位置是达标的(arr[mid]>=target)，index记录当前位置，继续向**左**二分
3. 当前位置不达标，向**右**侧二分
4. 直到left right里面完全没有数字： 因此范围是 Left <= right

<img src="pic/%5Bz%5D%E4%BA%8C%E5%88%86%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%83%85%E5%86%B5.assets/image-20220620130556592.png" alt="image-20220620130556592" style="zoom:25%;" />

```go
func search(nums []int, target int) int {
   l, r := 0, len(nums)-1
   index := -1 //记录最左侧的位置
   for l <= r { 
      mid := left + （(right left) >> 1） // 移位 复杂度低//mid := left + (right - left) / 2 
      // 满足≥， 但不一定是最左侧， 继续向左寻找
      if nums[mid] >= target {
         index = mid
         right= mid - 1
      }else { //直接num < target 不满足，向右寻找
         left = mid + 1
      }
   }
   return -1
}
```



## 3: 最小峰值

