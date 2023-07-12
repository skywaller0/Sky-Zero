# [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

给定一个有序的整数数组nums和一个目标值target，返回target在nums中出现的起始和结束位置的下标。如果target在nums中不存在，就返回[-1,-1]。

代码的思路是，使用两次二分查找的方法来分别找到target的第一个和最后一个位置。二分查找是一种在有序的数组中查找目标值的算法，它每次将搜索区间缩小一半，直到找到目标值或者搜索区间为空为止。具体来说，如果要在一个有序的数组中查找一个值v，就可以从数组的中间位置开始，比较v和中间位置的值，如果相等，就找到了；如果v小于中间位置的值，就说明v在左半区间；如果v大于中间位置的值，就说明v在右半区间。然后在相应的区间继续重复这个过程，直到找到v或者区间为空为止。

在这个问题中，要求解target的第一个和最后一个位置，也就是满足nums[i]==target的最小和最大的i。所以，可以分别对左边界和右边界进行二分查找。代码中定义了两个函数searchFirstEqualElement和searchLastEqualElement来实现这两种查找。它们都使用了三个变量low, high和mid来表示搜索区间的左右边界和中点，初始时为0, len(nums)-1和(low+high)/2。然后不断进行以下操作：

- 如果nums[mid]大于target，说明target在左半区间，就将high更新为mid-1。
- 如果nums[mid]小于target，说明target在右半区间，就将low更新为mid+1。
- 如果nums[mid]等于target，说明找到了一个可能的位置，但还需要判断是否是第一个或最后一个。
  - 对于第一个位置，需要判断mid是否为0或者nums[mid-1]是否不等于target，如果是，就说明mid是第一个位置，返回mid；否则，就说明第一个位置在左半区间，将high更新为mid-1。
  - 对于最后一个位置，需要判断mid是否为len(nums)-1或者nums[mid+1]是否不等于target，如果是，就说明mid是最后一个位置，返回mid；否则，就说明最后一个位置在右半区间，将low更新为mid+1。

当low大于high时，说明搜索区间为空，没有找到目标值，返回-1。

最后，在主函数searchRange中，调用这两个函数，并将结果组合成一个数组返回。

```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量头文件

using namespace std; // 使用标准命名空间

// 定义一个函数，接受一个有序的整数向量nums和一个目标值target，返回target在nums中出现的第一个位置的下标。如果target在nums中不存在，就返回-1。
int searchFirstEqualElement(vector<int> &nums, int target) {
    int low = 0, high = nums.size() - 1; // 定义两个变量low和high，分别表示搜索区间的左右边界，初始时为0和nums的长度减一
    while (low <= high) { // 当low小于等于high时，继续循环
        int mid = low + ((high - low) >> 1); // 计算搜索区间的中点
        if (nums[mid] > target) { // 如果中点的值大于target，说明target在左半区间
            high = mid - 1; // 将右边界更新为中点减一
        } else if (nums[mid] < target) { // 如果中点的值小于target，说明target在右半区间
            low = mid + 1; // 将左边界更新为中点加一
        } else { // 如果中点的值等于target，说明找到了一个可能的位置，但还需要判断是否是第一个
            if (mid == 0 || nums[mid - 1] != target) { // 如果mid为0或者前一个元素不等于target，说明mid是第一个位置
                return mid; // 返回mid
            }
            high = mid - 1; // 否则，说明第一个位置在左半区间，将右边界更新为mid减一
        }
    }
    return -1; // 如果循环结束还没有找到目标值，返回-1
}

// 定义一个函数，接受一个有序的整数向量nums和一个目标值target，返回target在nums中出现的最后一个位置的下标。如果target在nums中不存在，就返回-1。
int searchLastEqualElement(vector<int> &nums, int target) {
    int low = 0, high = nums.size() - 1; // 定义两个变量low和high，分别表示搜索区间的左右边界，初始时为0和nums的长度减一
    while (low <= high) { // 当low小于等于high时，继续循环
        int mid = low + ((high - low) >> 1); // 计算搜索区间的中点
        if (nums[mid] > target) { // 如果中点的值大于target，说明target在左半区间
            high = mid - 1; // 将右边界更新为中点减一
        } else if (nums[mid] < target) { // 如果中点的值小于target，说明target在右半区间
            low = mid + 1; // 将左边界更新为中点加一
        } else { // 如果中点的值等于target，说明找到了一个可能的位置，但还需要判断是否是最后一个
            if (mid == nums.size() - 1 || nums[mid + 1] != target) { // 如果mid为最后一个元素或者后一个元素不等于target，说明mid是最后一个位置
                return mid; // 返回mid
            }
            low = mid + 1; // 否则，说明最后一个位置在右半区间，将左边界更新为mid加一
        }
    }
    return -1; // 如果循环结束还没有找到目标值，返回-1
}

// 定义一个函数，接受一个有序的整数向量nums和一个目标值target，返回target在nums中出现的起始和结束位置的下标。如果target在nums中不存在，就返回[-1,-1]。
vector<int> searchRange(vector<int> &nums, int target) {
    return {searchFirstEqualElement(nums, target), searchLastEqualElement(nums, target)}; // 返回一个由两个函数结果组成的向量
}

// 主函数
int main() {
    vector<int> nums = {5, 7, 7, 8, 8, 10}; // 定义一个有序的整数向量nums，并赋值为{5,7,7,8,8,10}
    int target = 8; // 定义一个目标值target，并赋值为8
    vector<int> result = searchRange(nums, target); // 调用searchRange函数，并将结果赋值给result
    cout << "[" << result[0] << "," << result[1] << "]" << endl; // 将result输出到标准输出流，换行
    return 0; // 返回0，表示程序正常结束
}
```
执行用时：8 ms, 在所有 C++ 提交中击败了64.84%的用户

内存消耗：13.2 MB, 在所有 C++ 提交中击败了70.98%的用户

通过测试用例：88 / 88


```go
package main // 声明包名为main

import "fmt" // 引入fmt包，用于格式化输入输出

// 定义一个函数，接受一个有序的整数切片nums和一个目标值target，返回target在nums中出现的起始和结束位置的下标。如果target在nums中不存在，就返回[-1,-1]。
func searchRange(nums []int, target int) []int {
	return []int{searchFirstEqualElement(nums, target), searchLastEqualElement(nums, target)} // 返回一个由两个函数结果组成的切片
}

// 定义一个函数，接受一个有序的整数切片nums和一个目标值target，返回target在nums中出现的第一个位置的下标。如果target在nums中不存在，就返回-1。
func searchFirstEqualElement(nums []int, target int) int {
	low, high := 0, len(nums)-1 // 定义两个变量low和high，分别表示搜索区间的左右边界，初始时为0和nums的长度减一
	for low <= high { // 当low小于等于high时，继续循环
		mid := low + ((high - low) >> 1) // 计算搜索区间的中点
		if nums[mid] > target { // 如果中点的值大于target，说明target在左半区间
			high = mid - 1 // 将右边界更新为中点减一
		} else if nums[mid] < target { // 如果中点的值小于target，说明target在右半区间
			low = mid + 1 // 将左边界更新为中点加一
		} else { // 如果中点的值等于target，说明找到了一个可能的位置，但还需要判断是否是第一个
			if (mid == 0) || (nums[mid-1] != target) { // 如果mid为0或者前一个元素不等于target，说明mid是第一个位置
				return mid // 返回mid
			}
			high = mid - 1 // 否则，说明第一个位置在左半区间，将右边界更新为mid减一
		}
	}
	return -1 // 如果循环结束还没有找到目标值，返回-1
}

// 定义一个函数，接受一个有序的整数切片nums和一个目标值target，返回target在nums中出现的最后一个位置的下标。如果target在nums中不存在，就返回-1。
func searchLastEqualElement(nums []int, target int) int {
	low, high := 0, len(nums)-1 // 定义两个变量low和high，分别表示搜索区间的左右边界，初始时为0和nums的长度减一
	for low <= high { // 当low小于等于high时，继续循环
		mid := low + ((high - low) >> 1) // 计算搜索区间的中点
		if nums[mid] > target { // 如果中点的值大于target，说明target在左半区间
			high = mid - 1 // 将右边界更新为中点减一
		} else if nums[mid] < target { // 如果中点的值小于target，说明target在右半区间
			low = mid + 1 // 将左边界更新为中点加一
		} else { // 如果中点的值等于target，说明找到了一个可能的位置，但还需要判断是否是最后一个
			if (mid == len(nums)-1) || (nums[mid+1] != target) { // 如果mid为最后一个元素或者后一个元素不等于target，说明mid是最后一个位置
				return mid // 返回mid
			}
			low = mid + 1 // 否则，说明最后一个位置在右半区间，将左边界更新为mid加一
		}
	}
	return -1 // 如果循环结束还没有找到目标值，返回-1
}

// 主函数
func main() {
	nums := []int{5, 7, 7, 8, 8, 10} // 定义一个有序的整数切片nums，并赋值为{5, 7, 7, 8, 8, 10}
	target := 8 // 定义一个目标值target，并赋值为8
	fmt.Println(searchRange(nums, target)) // 调用searchRange函数，并将结果输出到标准输出流，换行
}
```


- 这一题是经典的二分搜索变种题。二分搜索有 4 大基础变种题：

  1. 查找第一个值等于给定值的元素
  2. 查找最后一个值等于给定值的元素
  3. 查找第一个大于等于给定值的元素
  4. 查找最后一个小于等于给定值的元素

  这一题的解题思路可以分别利用变种 1 和变种 2 的解法就可以做出此题。或者用一次变种 1 的方法，然后循环往后找到最后一个与给定值相等的元素。不过后者这种方法可能会使时间复杂度下降到 O(n)，因为有可能数组中 n 个元素都和给定元素相同。(4 大基础变种的实现见代码)

```go
  // 二分查找第一个与 target 相等的元素，时间复杂度 O(logn)
func searchFirstEqualElement(nums []int, target int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + ((high - low) >> 1)
		if nums[mid] > target {
			high = mid - 1
		} else if nums[mid] < target {
			low = mid + 1
		} else {
			if (mid == 0) || (nums[mid-1] != target) { // 找到第一个与 target 相等的元素
				return mid
			}
			high = mid - 1
		}
	}
	return -1
}

// 二分查找最后一个与 target 相等的元素，时间复杂度 O(logn)
func searchLastEqualElement(nums []int, target int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + ((high - low) >> 1)
		if nums[mid] > target {
			high = mid - 1
		} else if nums[mid] < target {
			low = mid + 1
		} else {
			if (mid == len(nums)-1) || (nums[mid+1] != target) { // 找到最后一个与 target 相等的元素
				return mid
			}
			low = mid + 1
		}
	}
	return -1
}

// 二分查找第一个大于等于 target 的元素，时间复杂度 O(logn)
func searchFirstGreaterElement(nums []int, target int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + ((high - low) >> 1)
		if nums[mid] >= target {
			if (mid == 0) || (nums[mid-1] < target) { // 找到第一个大于等于 target 的元素
				return mid
			}
			high = mid - 1
		} else {
			low = mid + 1
		}
	}
	return -1
}

// 二分查找最后一个小于等于 target 的元素，时间复杂度 O(logn)
func searchLastLessElement(nums []int, target int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + ((high - low) >> 1)
		if nums[mid] <= target {
			if (mid == len(nums)-1) || (nums[mid+1] > target) { // 找到最后一个小于等于 target 的元素
				return mid
			}
			low = mid + 1
		} else {
			high = mid - 1
		}
	}
	return -1
}
```


