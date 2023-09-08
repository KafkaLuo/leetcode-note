# Day1 数组 part01
数组理论基础，704.二分查找，27.移除元素

## 704.二分查找
### 题目描述：
给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例 1:

>输入: nums = [-1,0,3,5,9,12], target = 9
>
>输出: 4
>
>解释: 9 出现在 nums 中并且下标为 4

示例 2:

>输入: nums = [-1,0,3,5,9,12], target = 2
>
>输出: -1
>
>解释: 2 不存在 nums 中因此返回 -1  



### Idea
* 循环不变量：区间是while循环中的不变量。
* 左闭右闭 [left, right]
* 左闭右开 [left, right)

### 总结
区间的选择决定了 -> while的条件是<还是<=, right是取middle还是middle-1 （选择边界）  
注意溢出问题，越界

### Solution
1. 二分查找-方法1：左闭右闭
```python
# 二分查找-方法1：左闭右闭
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1 # 初始化左右区间

        while left <= right:
            middle = left + (right - left) // 2 #防止 left+right 越界

            if nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle - 1
            else:
                return middle
        return -1

```

2. 二分查找-方法2：左闭右开
```python
# 二分查找-方法2：左闭右开
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1 # 初始化左右区间

        while left < right:
            middle = left + (right - left) // 2 #防止 left+right 越界

            if nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle
            else:
                return middle
        return -1
```

- - -
## 27.移除元素
### 题目描述:
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。  

示例 1：

>输入：nums = [3,2,2,3], val = 3
>
>输出：2, nums = [2,2]
>
>解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。

示例 2：

>输入：nums = [0,1,2,2,3,0,4,2], val = 2
>
>输出：5, nums = [0,1,4,0,3]
>
>解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。

### Idea
* 暴力解法：两层for循环，复杂度O(n^2)
* 双指针：复杂度O(n)，只用1层for循环，快和慢两个指针。快指针指向新数组需要的元素的值，需要更新的下标是慢指针。  

### 总结
* 熟悉快慢-双指针的思想，可以减少复杂度。
* 快指针是循环条件，遍历数组
* 快指针搜索新数组需要的值，传递给慢指针对应下标的位置。
* 数组的erase，不存在删除元素，都是元素的覆盖。
* 循环结束的时候，慢指针会比快指针快走一步，代表新数组的长度。

### Solution
1. 暴力解法（双循环）
```
# 暴力解法
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        n = len(nums)

        while i < n:
            if nums[i] == val:
                for j in range(i+1, n):
                    nums[j-1] = nums[j]
                n -= 1
            else:
                i += 1

        return n
```

2. 双指针
```
# 双指针解法
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow, fast, n = 0, 0, len(nums)

        while fast < n:
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1

        return slow
```


