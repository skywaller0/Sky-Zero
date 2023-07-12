# [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

用来在一个旋转排序数组中查找最小值的。它使用了二分查找的思想，但是需要处理一些特殊情况，比如数组中有重复元素，或者无法确定哪个区间是有序的。代码的基本思路是：

- 定义两个指针 low 和 high，分别指向切片的左右边界。
- 当 low < high 时，循环执行。
- 如果 nums[low] 小于 nums[high]，说明当前区间是有序的，那么最小值就是 nums[low]，直接返回。
- 计算中间位置 mid，使用位运算代替除法。
- 如果 nums[mid] 大于 nums[low]，说明左区间是有序的，那么最小值一定在右区间内，所以将 low 更新为 mid + 1。
- 如果 nums[mid] 等于 nums[low]，说明左边界和中间位置的元素相同，无法判断左区间是否有序，所以将 low 右移一位，继续查找。
- 如果 nums[mid] 小于 nums[low]，说明右区间是有序的，那么最小值一定在左区间内，所以将 high 更新为 mid。
- 重复上述步骤，直到 low >= high。
- 返回 nums[low] 作为最小值。

```c++
#include <iostream> // 导入iostream头文件，用于输入输出
#include <vector> // 导入vector头文件，用于存储数组
using namespace std; // 使用std命名空间

int findMin(vector<int>& nums) { // 定义一个函数，接受一个整数向量的引用作为参数，返回一个整数
	int low = 0, high = nums.size() - 1; // 初始化两个变量，low表示数组的左边界，high表示数组的右边界
	while (low < high) { // 当左边界小于右边界时，循环执行以下操作
		if (nums[low] < nums[high]) { // 如果左边界的元素小于右边界的元素，说明数组没有旋转，直接返回左边界的元素
			return nums[low];
		}
		int mid = low + (high - low) / 2; // 计算数组的中间位置
		if (nums[mid] > nums[low]) { // 如果中间位置的元素大于左边界的元素，说明最小元素在右半部分，将左边界移动到中间位置的右边
			low = mid + 1;
		} else if (nums[mid] == nums[low]) { // 如果中间位置的元素等于左边界的元素，说明有重复元素，将左边界向右移动一位
			low++;
		} else { // 否则，说明最小元素在左半部分，将右边界移动到中间位置
			high = mid;
		}
	}
	return nums[low]; // 循环结束后，返回左边界的元素，即最小元素
}

int main() { // 定义主函数
	vector<int> nums = {2, 2, 2, 0, 1}; // 初始化一个旋转过的有序数组
	cout << findMin(nums) << endl; // 调用findMin函数，并输出结果
	return 0; // 返回0表示程序正常结束
}
```



```go
package main // 声明包名为main

import "fmt" // 导入fmt包，用于输出

func findMin(nums []int) int { // 定义一个函数，接受一个整数切片作为参数，返回一个整数
	low, high := 0, len(nums)-1 // 初始化两个变量，low表示数组的左边界，high表示数组的右边界
	for low < high { // 当左边界小于右边界时，循环执行以下操作
		if nums[low] < nums[high] { // 如果左边界的元素小于右边界的元素，说明数组没有旋转，直接返回左边界的元素
			return nums[low]
		}
		mid := low + (high-low)>>1 // 计算数组的中间位置，使用位运算代替除法提高效率
		if nums[mid] > nums[low] { // 如果中间位置的元素大于左边界的元素，说明最小元素在右半部分，将左边界移动到中间位置的右边
			low = mid + 1
		} else if nums[mid] == nums[low] { // 如果中间位置的元素等于左边界的元素，说明有重复元素，将左边界向右移动一位
			low++
		} else { // 否则，说明最小元素在左半部分，将右边界移动到中间位置
			high = mid
		}
	}
	return nums[low] // 循环结束后，返回左边界的元素，即最小元素
}

func main() { // 定义主函数
	nums := []int{2, 2, 2, 0, 1} // 初始化一个旋转过的有序数组
	fmt.Println(findMin(nums)) // 调用findMin函数，并输出结果
}

```

执行用时：4 ms, 在所有 Go 提交中击败了77.70%的用户

内存消耗：2.9 MB, 在所有 Go 提交中击败了100.00%的用户

通过测试用例：193 / 193



这题目的数组按照升序排列，然后在某个点上旋转了一下，比如 [0, 1, 2, 4, 5, 6, 7] 可能会变成 [4, 5, 6, 7, 0, 1, 2]。这样的数组中，最小值是 0。只要一直找到最小值就可以了。旋转其实跟片段的移动是一样的，只不过旋转是把一个片段移到了数组的开头，而片段的移动是把一个片段移到了数组的结尾。
