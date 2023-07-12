# [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/)

思路是利用二分查找的方法，根据数组的有序性和元素的重复性，缩小查找范围，直到找到唯一的元素。具体步骤如下：

- 首先，初始化两个变量，left表示数组的左边界，right表示数组的右边界。
- 然后，当左边界小于右边界时，循环执行以下操作：
  - 计算数组的中间位置mid。
  - 如果中间位置是偶数，说明左半部分有偶数个元素。如果中间位置的元素等于它右边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边。否则，说明唯一的元素在左半部分，将右边界移动到中间位置。
  - 如果中间位置是奇数，说明左半部分有奇数个元素。如果中间位置的元素等于它左边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边。否则，说明唯一的元素在左半部分，将右边界移动到中间位置。
- 最后，循环结束后，返回左边界的元素，即唯一的元素。



```c++
#include <iostream> // 导入iostream头文件，用于输入输出
#include <vector> // 导入vector头文件，用于存储数组

using namespace std; // 使用std命名空间

int singleNonDuplicate(vector<int> &nums) { // 定义一个函数，接受一个整数向量的引用作为参数，返回一个整数
    int left = 0, right = nums.size() - 1; // 初始化两个变量，left表示数组的左边界，right表示数组的右边界
    while (left < right) { // 当左边界小于右边界时，循环执行以下操作
        int mid = (left + right) / 2; // 计算数组的中间位置
        if (mid % 2 == 0) { // 如果中间位置是偶数，说明左半部分有偶数个元素
            if (nums[mid] == nums[mid + 1]) { // 如果中间位置的元素等于它右边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边
                left = mid + 1;
            } else { // 否则，说明唯一的元素在左半部分，将右边界移动到中间位置
                right = mid;
            }
        } else { // 如果中间位置是奇数，说明左半部分有奇数个元素
            if (nums[mid] == nums[mid - 1]) { // 如果中间位置的元素等于它左边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边
                left = mid + 1;
            } else { // 否则，说明唯一的元素在左半部分，将右边界移动到中间位置
                right = mid;
            }
        }
    }
    return nums[left]; // 循环结束后，返回左边界的元素，即唯一的元素
}

int main() { // 定义主函数
    vector<int> nums = {1, 1, 2, 3, 3, 4, 4, 8, 8}; // 初始化一个只有一个元素出现一次，其余元素都出现两次的有序数组
    cout << singleNonDuplicate(nums) << endl; // 调用singleNonDuplicate函数，并输出结果
    return 0; // 返回0表示程序正常结束
}

```
执行用时：20 ms, 在所有 C++ 提交中击败了78.18%的用户

内存消耗：21.8 MB, 在所有 C++ 提交中击败了43.57%的用户

通过测试用例：15 / 15


```go
package main

func singleNonDuplicate(nums []int) int { // 定义一个函数，接受一个整数切片作为参数，返回一个整数
	left, right := 0, len(nums)-1 // 初始化两个变量，left表示数组的左边界，right表示数组的右边界
	for left < right {            // 当左边界小于右边界时，循环执行以下操作
		mid := (left + right) / 2 // 计算数组的中间位置
		if mid%2 == 0 {           // 如果中间位置是偶数，说明左半部分有偶数个元素
			if nums[mid] == nums[mid+1] { // 如果中间位置的元素等于它右边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边
				left = mid + 1
			} else { // 否则，说明唯一的元素在左半部分，将右边界移动到中间位置
				right = mid
			}
		} else { // 如果中间位置是奇数，说明左半部分有奇数个元素
			if nums[mid] == nums[mid-1] { // 如果中间位置的元素等于它左边的元素，说明唯一的元素在右半部分，将左边界移动到中间位置的右边
				left = mid + 1
			} else { // 否则，说明唯一的元素在左半部分，将右边界移动到中间位置
				right = mid
			}
		}
	}
	return nums[left] // 循环结束后，返回左边界的元素，即唯一的元素
}

func main() {
	nums := []int{1, 1, 2, 3, 3, 4, 4, 8, 8}
	println(singleNonDuplicate(nums))
}
```
执行用时：24 ms, 在所有 Go 提交中击败了85.82%的用户

内存消耗：7.7 MB, 在所有 Go 提交中击败了67.91%的用户

通过测试用例：15 / 15

就是一直查找，直到找到不同的元素，时间复杂度为O(n)，空间复杂度为O(1)。
但是需要注意mid为奇偶数时。

```
假设下标idx是单独的数字,idx左边的偶数下标x有nums[x] == nums[x + 1],
idx右边的奇数下标y有nums[y] == nums[y + 1],可以根据此特性用二分查找idx对应的值 
```

