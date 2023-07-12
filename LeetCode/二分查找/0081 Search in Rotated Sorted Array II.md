# [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

这个代码是用来在一个旋转排序数组中查找一个目标值是否存在的。它使用了二分查找的思想，但是需要处理一些特殊情况，比如数组中有重复元素，或者无法确定哪个区间是有序的。代码的基本思路是：

- 定义两个指针 l 和 r，分别指向数组的左右边界。
- 计算中间位置 mid = l + (r - l) / 2，并判断 nums[mid] 是否等于 target，如果是，返回 true。
- 如果 nums[l] 等于 nums[mid]，说明左边界和中间位置的元素相同，无法判断左区间是否有序，所以将 l 右移一位，继续查找。
- 如果 nums[r] 等于 nums[mid]，说明右边界和中间位置的元素相同，无法判断右区间是否有序，所以将 r 左移一位，继续查找。
- 如果 nums[mid] 小于 nums[r]，说明右区间是有序的，那么如果 target 在 nums[mid] 和 nums[r] 之间，就将 l 更新为 mid + 1，否则将 r 更新为 mid - 1。
- 如果 nums[mid] 大于 nums[r]，说明左区间是有序的，那么如果 target 在 nums[l] 和 nums[mid] 之间，就将 r 更新为 mid - 1，否则将 l 更新为 mid + 1。
- 重复上述步骤，直到 l > r 或者找到 target。
- 如果没有找到 target，返回 false。

```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量容器头文件

using namespace std; // 使用标准命名空间

bool search(vector<int> &nums, int target) { // 定义一个函数，接受一个整数向量和一个目标值作为参数，返回一个布尔值
    int l = 0, r = nums.size() - 1; // 定义两个指针 l 和 r，分别指向数组的左右边界
    while (l <= r) { // 当 l <= r 时，循环执行
        int mid = l + (r - l) / 2; // 计算中间位置 mid
        if (nums[mid] == target) { // 如果 nums[mid] 等于 target，说明找到了目标值
            return true; // 返回 true
        }
        if (nums[l] == nums[mid]) {// 如果 nums[l] 等于 nums[mid]，说明左边界和中间位置的元素相同，无法判断左区间是否有序
            ++l; // 将 l 右移一位，继续查找
        } else if (nums[r] == nums[mid]) {// 如果 nums[r] 等于 nums[mid]，说明右边界和中间位置的元素相同，无法判断右区间是否有序
            --r; // 将 r 左移一位，继续查找
        } else if (nums[mid] < nums[r]) {// 如果 nums[mid] 小于 nums[r]，说明右区间是有序的
            if (target > nums[mid] && target <= nums[r]) { // 如果 target 在 nums[mid] 和 nums[r] 之间，说明 target 在右区间内
                l = mid + 1; // 将 l 更新为 mid + 1，缩小查找范围
            } else { // 否则，说明 target 在左区间内
                r = mid - 1; // 将 r 更新为 mid - 1，缩小查找范围
            }
        } else {// 如果 nums[mid] 大于 nums[r]，说明左区间是有序的
            if (target >= nums[l] && target < nums[mid]) { // 如果 target 在 nums[l] 和 nums[mid] 之间，说明 target 在左区间内
                r = mid - 1; // 将 r 更新为 mid - 1，缩小查找范围
            } else { // 否则，说明 target 在右区间内
                l = mid + 1; // 将 l 更新为 mid + 1，缩小查找范围
            }
        }
    }
    return false; // 如果循环结束后还没有找到 target，返回 false
}

int main() { // 定义主函数
    vector<int> nums = {2, 5, 6, 0, 0, 1, 2}; // 定义一个旋转排序数组 nums
    int target = 0; // 定义一个目标值 target
    cout << search(nums, target) << endl; // 调用 search 函数，并将结果输出到控制台
    return 0; // 返回 0，表示程序正常结束
}
```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：13.5 MB, 在所有 C++ 提交中击败了94.28%的用户

通过测试用例：280 / 280

```go
package main // 定义一个包名为 main 的包

import "fmt" // 引入 fmt 包，用于格式化输出

func search(nums []int, target int) bool { // 定义一个函数，接受一个整数切片和一个目标值作为参数，返回一个布尔值
	if len(nums) == 0 { // 如果切片的长度为 0，说明没有元素
		return false // 返回 false
	}
	low, high := 0, len(nums)-1 // 定义两个指针 low 和 high，分别指向切片的左右边界
	for low <= high { // 当 low <= high 时，循环执行
		mid := low + (high-low)>>1 // 计算中间位置 mid，使用位运算代替除法
		if nums[mid] == target { // 如果 nums[mid] 等于 target，说明找到了目标值
			return true // 返回 true
		} else if nums[mid] > nums[low] { // 如果 nums[mid] 大于 nums[low]，说明左区间是有序的
			if nums[low] <= target && target < nums[mid] { // 如果 target 在 nums[low] 和 nums[mid] 之间，说明 target 在左区间内
				high = mid - 1 // 将 high 更新为 mid - 1，缩小查找范围
			} else { // 否则，说明 target 在右区间内
				low = mid + 1 // 将 low 更新为 mid + 1，缩小查找范围
			}
		} else if nums[mid] < nums[high] { // 如果 nums[mid] 小于 nums[high]，说明右区间是有序的
			if nums[mid] < target && target <= nums[high] { // 如果 target 在 nums[mid] 和 nums[high] 之间，说明 target 在右区间内
				low = mid + 1 // 将 low 更新为 mid + 1，缩小查找范围
			} else { // 否则，说明 target 在左区间内
				high = mid - 1 // 将 high 更新为 mid - 1，缩小查找范围
			}
		} else { // 如果 nums[mid] 等于 nums[low] 或者等于 nums[high]，说明无法判断哪个区间是有序的
			if nums[low] == nums[mid] { // 如果 nums[low] 等于 nums[mid]
				low++ // 将 low 右移一位，继续查找
			}
			if nums[high] == nums[mid] { // 如果 nums[high] 等于 nums[mid]
				high-- // 将 high 左移一位，继续查找
			}
		}
	}
	return false // 如果循环结束后还没有找到 target，返回 false
}

func main() { // 定义主函数
	nums := []int{1, 1, 3, 1} // 定义一个旋转排序数组 nums
	target := 3 // 定义一个目标值 target
	fmt.Println(search(nums, target)) // 调用 search 函数，并将结果输出到控制台
}
```

执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：3 MB, 在所有 Go 提交中击败了100.00%的用户

通过测试用例：280 / 280



给出一个数组，数组中本来是从小到大排列的，并且数组中有重复数字。但是现在把后面随机一段有序的放到数组前面，这样形成了前后两端有序的子序列。在这样的一个数组里面查找一个数，设计一个 O(log n) 的算法。如果找到就输出 `true`，如果没有找到，就输出 `false` 。

