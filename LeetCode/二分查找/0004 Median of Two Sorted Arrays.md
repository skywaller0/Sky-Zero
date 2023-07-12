# [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

代码的思路是利用二分查找的方法，根据数组的有序性和中位数的定义，找到两个数组的合适的划分，使得划分左边的元素都小于或等于划分右边的元素，并且左右两边的元素个数相等或相差一。具体步骤如下：

- 首先，假设nums1的长度小于或等于nums2的长度，如果不是，交换两个数组。
- 然后，初始化五个变量，low和high表示nums1的左右边界，k表示两个数组长度之和的一半加一，nums1Mid和nums2Mid表示两个数组的分界线位置。
- 接着，当low小于或等于high时，循环执行以下操作：
  - 计算nums1的分界线位置为low和high的中点。
  - 计算nums2的分界线位置为k减去nums1的分界线位置。
  - 比较nums1和nums2中分界线左右两边的元素，判断是否满足中位数的条件。如果不满足，根据比较结果调整low或high的值，缩小查找范围。
- 最后，当找到合适的划分后，根据两个数组长度之和是奇数还是偶数，返回中位数。如果是奇数，中位数就是划分左边的最大元素；如果是偶数，中位数就是划分左右两边最大和最小元素的平均值。

```c++
#include <iostream> // 导入iostream头文件，用于输入输出
#include <vector> // 导入vector头文件，用于存储数组

using namespace std; // 使用std命名空间

double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2) { // 定义一个函数，接受两个整数向量的引用作为参数，返回一个浮点数
    // 假设 nums1 的长度小
    if (nums1.size() > nums2.size()) { // 如果nums1的长度大于nums2的长度，交换两个数组，使得nums1的长度小于或等于nums2的长度
        return findMedianSortedArrays(nums2, nums1);
    }
    int low = 0, high = nums1.size(), k = (nums1.size() + nums2.size() + 1) /
                                          2, nums1Mid = 0, nums2Mid = 0; // 初始化五个变量，low和high表示nums1的左右边界，k表示两个数组长度之和的一半加一，nums1Mid和nums2Mid表示两个数组的分界线位置
    while (low <= high) { // 当low小于或等于high时，循环执行以下操作
        // nums1:  ……………… nums1[nums1Mid-1] | nums1[nums1Mid] ……………………
        // nums2:  ……………… nums2[nums2Mid-1] | nums2[nums2Mid] ……………………
        nums1Mid = low + (high - low) / 2; // 计算nums1的分界线位置
        nums2Mid = k - nums1Mid; // 计算nums2的分界线位置，使得两个分界线左边的元素个数之和等于k
        if (nums1Mid > 0 &&
            nums1[nums1Mid - 1] > nums2[nums2Mid]) { // 如果nums1中的分界线左边的元素大于nums2中的分界线右边的元素，说明nums1中的分界线划多了，要向左边移动
            high = nums1Mid - 1;
        } else if (nums1Mid != nums1.size() &&
                   nums1[nums1Mid] < nums2[nums2Mid - 1]) { // 如果nums1中的分界线右边的元素小于nums2中的分界线左边的元素，说明nums1中的分界线划少了，要向右边移动
            low = nums1Mid + 1;
        } else {
            // 找到合适的划分了，需要输出最终结果了
            // 分为奇数偶数 2 种情况
            break;
        }
    }
    int midLeft = 0, midRight = 0; // 初始化两个变量，midLeft表示中位数左边的元素，midRight表示中位数右边的元素
    if (nums1Mid == 0) { // 如果nums1中没有元素在分界线左边，说明midLeft是nums2中的分界线左边的元素
        midLeft = nums2[nums2Mid - 1];
    } else if (nums2Mid == 0) { // 如果nums2中没有元素在分界线左边，说明midLeft是nums1中的分界线左边的元素
        midLeft = nums1[nums1Mid - 1];
    } else { // 否则，midLeft是nums1和nums2中分界线左边的元素中较大的那个
        midLeft = max(nums1[nums1Mid - 1], nums2[nums2Mid - 1]);
    }
    if ((nums1.size() + nums2.size()) % 2 == 1) { // 如果两个数组长度之和是奇数，说明中位数就是midLeft
        return midLeft;
    }
    if (nums1Mid == nums1.size()) { // 如果nums1中没有元素在分界线右边，说明midRight是nums2中的分界线右边的元素
        midRight = nums2[nums2Mid];
    } else if (nums2Mid == nums2.size()) { // 如果nums2中没有元素在分界线右边，说明midRight是nums1中的分界线右边的元素
        midRight = nums1[nums1Mid];
    } else { // 否则，midRight是nums1和nums2中分界线右边的元素中较小的那个
        midRight = min(nums1[nums1Mid], nums2[nums2Mid]);
    }
    return (midLeft + midRight) / 2.0; // 如果两个数组长度之和是偶数，说明中位数是midLeft和midRight的平均值
}


int main() {
    vector<int> nums1 = {1, 3}, nums2 = {2};
    cout << findMedianSortedArrays(nums1, nums2) << endl;
    return 0;
}
```
执行用时：16 ms, 在所有 C++ 提交中击败了97.31%的用户

内存消耗：87.1 MB, 在所有 C++ 提交中击败了76.93%的用户

通过测试用例：2094 / 2094


```go
package main

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 { // 定义一个函数，接受两个整数切片作为参数，返回一个浮点数
	// 假设 nums1 的长度小
	if len(nums1) > len(nums2) { // 如果nums1的长度大于nums2的长度，交换两个数组，使得nums1的长度小于或等于nums2的长度
		return findMedianSortedArrays(nums2, nums1)
	}
	low, high, k, nums1Mid, nums2Mid := 0, len(nums1), (len(nums1)+len(nums2)+1)>>1, 0, 0 // 初始化五个变量，low和high表示nums1的左右边界，k表示两个数组长度之和的一半加一，nums1Mid和nums2Mid表示两个数组的分界线位置
	for low <= high {                                                                     // 当low小于或等于high时，循环执行以下操作
		// nums1:  ……………… nums1[nums1Mid-1] | nums1[nums1Mid] ……………………
		// nums2:  ……………… nums2[nums2Mid-1] | nums2[nums2Mid] ……………………
		nums1Mid = low + (high-low)>>1                           // 计算nums1的分界线位置，使用位运算代替除法提高效率
		nums2Mid = k - nums1Mid                                  // 计算nums2的分界线位置，使得两个分界线左边的元素个数之和等于k
		if nums1Mid > 0 && nums1[nums1Mid-1] > nums2[nums2Mid] { // 如果nums1中的分界线左边的元素大于nums2中的分界线右边的元素，说明nums1中的分界线划多了，要向左边移动
			high = nums1Mid - 1
		} else if nums1Mid != len(nums1) && nums1[nums1Mid] < nums2[nums2Mid-1] { // 如果nums1中的分界线右边的元素小于nums2中的分界线左边的元素，说明nums1中的分界线划少了，要向右边移动
			low = nums1Mid + 1
		} else {
			// 找到合适的划分了，需要输出最终结果了
			// 分为奇数偶数 2 种情况
			break
		}
	}
	midLeft, midRight := 0, 0 // 初始化两个变量，midLeft表示中位数左边的元素，midRight表示中位数右边的元素
	if nums1Mid == 0 {        // 如果nums1中没有元素在分界线左边，说明midLeft是nums2中的分界线左边的元素
		midLeft = nums2[nums2Mid-1]
	} else if nums2Mid == 0 { // 如果nums2中没有元素在分界线左边，说明midLeft是nums1中的分界线左边的元素
		midLeft = nums1[nums1Mid-1]
	} else { // 否则，midLeft是nums1和nums2中分界线左边的元素中较大的那个
		midLeft = max(nums1[nums1Mid-1], nums2[nums2Mid-1])
	}
	if (len(nums1)+len(nums2))&1 == 1 { // 如果两个数组长度之和是奇数，说明中位数就是midLeft
		return float64(midLeft)
	}
	if nums1Mid == len(nums1) { // 如果nums1中没有元素在分界线右边，说明midRight是nums2中的分界线右边的元素
		midRight = nums2[nums2Mid]
	} else if nums2Mid == len(nums2) { // 如果nums2中没有元素在分界线右边，说明midRight是nums1中的分界线右边的元素
		midRight = nums1[nums1Mid]
	} else { // 否则，midRight是nums1和nums2中分界线右边的元素中较小的那个
		midRight = min(nums1[nums1Mid], nums2[nums2Mid])
	}
	return float64(midLeft+midRight) / 2 // 如果两个数组长度之和是偶数，说明中位数是midLeft和midRight的平均值
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
func max(a, b int) (c int) {
	if a > b {
		return a
	}
	return b
}

func main() {
	nums1 := []int{1, 2}
	nums2 := []int{3, 4}
	println(findMedianSortedArrays(nums1, nums2))
}
```

执行用时：12 ms, 在所有 Go 提交中击败了69.97%的用户

内存消耗：4.8 MB, 在所有 Go 提交中击败了51.73%的用户

通过测试用例：2094 / 2094

解题：这道题目想通了就没那么难
- 给出两个有序数组，要求找出这两个数组合并以后的有序数组中的中位数。要求时间复杂度为 O(log (m+n))。
- 这一题最容易想到的办法是把两个数组合并，然后取出中位数。但是合并有序数组的操作是 `O(m+n)` 的，不符合题意。看到题目给的 `log` 的时间复杂度，很容易联想到二分搜索。
- 由于要找到最终合并以后数组的中位数，两个数组的总大小也知道，所以中间这个位置也是知道的。只需要二分搜索一个数组中切分的位置，另一个数组中切分的位置也能得到。为了使得时间复杂度最小，所以二分搜索两个数组中长度较小的那个数组。
- 关键的问题是如何切分数组 1 和数组 2 。其实就是如何切分数组 1 。先随便二分产生一个 `midA`，切分的线何时算满足了中位数的条件呢？即，线左边的数都小于右边的数，即，`nums1[midA-1] ≤ nums2[midB] && nums2[midB-1] ≤ nums1[midA]` 。如果这些条件都不满足，切分线就需要调整。如果 `nums1[midA] < nums2[midB-1]`，说明 `midA` 这条线划分出来左边的数小了，切分线应该右移；如果 `nums1[midA-1] > nums2[midB]`，说明 midA 这条线划分出来左边的数大了，切分线应该左移。经过多次调整以后，切分线总能找到满足条件的解。
- 假设现在找到了切分的两条线了，`数组 1` 在切分线两边的下标分别是 `midA - 1` 和 `midA`。`数组 2` 在切分线两边的下标分别是 `midB - 1` 和 `midB`。最终合并成最终数组，如果数组长度是奇数，那么中位数就是 `max(nums1[midA-1], nums2[midB-1])`。如果数组长度是偶数，那么中间位置的两个数依次是：`max(nums1[midA-1], nums2[midB-1])` 和 `min(nums1[midA], nums2[midB])`，那么中位数就是 `(max(nums1[midA-1], nums2[midB-1]) + min(nums1[midA], nums2[midB])) / 2`。


