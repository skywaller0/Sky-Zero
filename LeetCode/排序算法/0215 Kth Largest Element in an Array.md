# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

给定一个整数数组 nums 和一个整数 k，找出数组中第 k 大的元素。例如，如果 nums = [3,2,1,5,6,4]，k = 2，那么第二大的元素是 5。

这个代码使用了一个叫做**快速选择**的算法，它是快速排序的变种。快速选择的思路是，随机选择一个数组中的元素作为**基准**，然后将数组分成两部分，一部分是小于等于基准的元素，另一部分是大于基准的元素。这样，我们就可以知道基准在排序后的位置，以及它是第几小的元素。如果它恰好是第 k 小的元素，那么我们就找到了答案。如果它比第 k 小的元素还要小，那么我们就在它右边的部分继续查找。如果它比第 k 小的元素还要大，那么我们就在它左边的部分继续查找。这个过程不断重复，直到找到第 k 小的元素为止。

这个算法的时间复杂度期望是 O(n)，最坏情况是 O(n^2)。空间复杂度是 O(log n)，因为需要递归调用函数。

代码中的函数 findKthLargest 就是实现了这个算法，它调用了 selectSmallest 函数来查找第 m 小的元素，其中 m = nums.size() - k + 1。因为第 k 大的元素就是第 m 小的元素。selectSmallest 函数接收四个参数，nums 是要查找的数组，l 和 r 是要查找的区间范围，i 是要查找的第 i 小的元素。selectSmallest 函数首先判断区间是否只有一个元素，如果是，就直接返回该元素。否则，就调用 partition 函数来对区间进行划分，并返回基准的位置 q。然后根据 q 和 i 的关系来决定在哪个部分继续查找。

partition 函数也接收三个参数，nums 是要划分的数组，l 和 r 是要划分的区间范围。partition 函数首先随机选择一个区间内的元素作为基准，并将其与区间末尾的元素交换位置。然后定义一个变量 i 来表示小于等于基准的元素的右边界。从区间左端开始遍历每个元素 j，如果 j 处的元素小于等于基准，就将其与 i+1 处的元素交换位置，并将 i 加一。这样可以保证 nums[l…i] 都是小于等于基准的元素，nums[i+1…j-1] 都是大于基准的元素。遍历完后，将基准与 i+1 处的元素交换位置，并返回 i+1 作为基准在排序后的位置。

```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量容器库

using namespace std; // 使用标准命名空间

int partition(vector<int> &nums, int l, int r) { // 定义一个函数，用于对数组进行划分，并返回基准的位置
    int k = l + rand() % (r - l + 1); // 随机选择一个区间内的元素作为基准
    swap(nums[k], nums[r]); // 将基准与区间末尾的元素交换位置
    int i = l - 1; // 定义一个变量 i 来表示小于等于基准的元素的右边界
    for (int j = l; j < r; j++) { // 从区间左端开始遍历每个元素 j
        if (nums[j] <= nums[r]) { // 如果 j 处的元素小于等于基准
            i++; // 将 i 加一
            swap(nums[i], nums[j]); // 将 i 和 j 处的元素交换位置
        }
    }
    swap(nums[i + 1], nums[r]); // 将基准与 i+1 处的元素交换位置
    return i + 1; // 返回基准在排序后的位置
}

int selectSmallest(vector<int> &nums, int l, int r, int i) { // 定义一个函数，用于查找第 i 小的元素，并返回其值
    if (l >= r) { // 如果区间只有一个元素
        return nums[l]; // 直接返回该元素
    }
    int q = partition(nums, l, r); // 调用 partition 函数来对区间进行划分，并返回基准的位置 q
    int k = q - l + 1; // 计算 q 是第几小的元素
    if (k == i) { // 如果 q 恰好是第 i 小的元素
        return nums[q]; // 返回 q 处的元素
    }
    if (i < k) { // 如果 i 小于 k，说明第 i 小的元素在 q 的左边
        return selectSmallest(nums, l, q - 1, i); // 在左边的部分继续查找
    } else { // 如果 i 大于 k，说明第 i 小的元素在 q 的右边
        return selectSmallest(nums, q + 1, r, i - k); // 在右边的部分继续查找，注意要减去 k，因为左边已经有 k 个元素了
    }
}

int findKthLargest(vector<int> &nums, int k) { // 定义一个函数，用于查找第 k 大的元素，并返回其值
    int m = nums.size() - k + 1; // 计算第 k 大的元素是第 m 小的元素，其中 m = nums.size() - k + 1
    return selectSmallest(nums, 0, nums.size() - 1, m); // 调用 selectSmallest 函数来查找第 m 小的元素，并返回其值
}

int main() { // 定义主函数，程序入口点
    vector<int> nums = {3, 2, 1, 5, 6, 4}; // 定义一个向量容器，存放要查找的数组
    int k = 2; // 定义一个整数变量，存放要查找的第 k 大的元素
    int res = findKthLargest(nums, k); // 调用 findKthLargest 函数来查找第 k 大的元素，并将其值赋给 res 变量
    printf("%d\n", res); // 输出 res 的值
    return 0; // 返回 0，表示程序正常结束
}
```
执行用时：88 ms, 在所有 C++ 提交中击败了66.37%的用户

内存消耗：44.4 MB, 在所有 C++ 提交中击败了67.52%的用户

通过测试用例：40 / 40


```go
package main // 定义包名为 main

import "math/rand" // 引入 math/rand 包，用于生成随机数

func findKthLargest(nums []int, k int) int { // 定义一个函数，用于查找第 k 大的元素，并返回其值
	m := len(nums) - k + 1 // 计算第 k 大的元素是第 m 小的元素，其中 m = len(nums) - k + 1
	return selectSmallest(nums, 0, len(nums)-1, m) // 调用 selectSmallest 函数来查找第 m 小的元素，并返回其值
}

func selectSmallest(nums []int, l, r, i int) int { // 定义一个函数，用于查找第 i 小的元素，并返回其值
	if l >= r { // 如果区间只有一个元素
		return nums[l] // 直接返回该元素
	}
	q := partition(nums, l, r) // 调用 partition 函数来对区间进行划分，并返回基准的位置 q
	k := q - l + 1 // 计算 q 是第几小的元素
	if k == i { // 如果 q 恰好是第 i 小的元素
		return nums[q] // 返回 q 处的元素
	}
	if i < k { // 如果 i 小于 k，说明第 i 小的元素在 q 的左边
		return selectSmallest(nums, l, q-1, i) // 在左边的部分继续查找
	} else { // 如果 i 大于 k，说明第 i 小的元素在 q 的右边
		return selectSmallest(nums, q+1, r, i-k) // 在右边的部分继续查找，注意要减去 k，因为左边已经有 k 个元素了
	}
}

func partition(nums []int, l, r int) int { // 定义一个函数，用于对数组进行划分，并返回基准的位置
	k := l + rand.Intn(r-l+1) // 随机选择一个区间内的元素作为基准
	nums[k], nums[r] = nums[r], nums[k] // 将基准与区间末尾的元素交换位置
	i := l - 1 // 定义一个变量 i 来表示小于等于基准的元素的右边界
	// nums[l..i] <= nums[r]
	// nums[i+1..j-1] > nums[r]
	for j := l; j < r; j++ { // 从区间左端开始遍历每个元素 j
		if nums[j] <= nums[r] { // 如果 j 处的元素小于等于基准
			i++ // 将 i 加一
			nums[i], nums[j] = nums[j], nums[i] // 将 i 和 j 处的元素交换位置
		}
	}
	nums[i+1], nums[r] = nums[r], nums[i+1] // 将基准与 i+1 处的元素交换位置
	return i + 1 // 返回基准在排序后的位置
}

func main() { // 定义主函数，程序入口点
	nums := []int{3, 2, 1, 5, 6, 4} // 定义一个切片，存放要查找的数组
	k := 2 // 定义一个整数变量，存放要查找的第 k 大的元素
	println(findKthLargest(nums, k)) // 输出 findKthLargest 函数的返回值
}
```

执行用时：64 ms, 在所有 Go 提交中击败了91.56%的用户

内存消耗：7.5 MB, 在所有 Go 提交中击败了97.06%的用户

通过测试用例：40 / 40