# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

给定一个只包含 0，1，2 的数组 nums，对它进行排序，使得相同的元素相邻，且按照 0，1，2 的顺序排列。例如，如果 nums = [2, 0, 2, 1, 1, 0]，那么排序后的数组是 [0, 0, 1, 1, 2, 2]。

这个代码使用了一个叫做**三指针**的算法，它可以在一次遍历中完成排序。它使用了两个变量 zero 和 one 来表示 0 和 1 的右边界，即 zero 左边的元素都是 0，one 左边的元素都是 0 或 1。然后从左到右遍历数组中的每个元素 i，根据它的值 n 来更新 zero 和 one 的位置，并将 i 处的元素设为正确的值。具体来说：

- 如果 n 等于 2，那么不需要做任何操作，因为 i 处的元素已经是 2。
- 如果 n 等于 1，那么需要将 one 处的元素设为 1，并将 one 加一，表示 1 的右边界向右移动一位。
- 如果 n 等于 0，那么需要将 one 处的元素设为 1，并将 one 加一，表示 1 的右边界向右移动一位。同时还需要将 zero 处的元素设为 0，并将 zero 加一，表示 0 的右边界向右移动一位。

这样，在遍历结束后，数组就被分成了三个部分：[0, zero) 是所有的 0，[zero, one) 是所有的 1，[one, nums.size()) 是所有的 2。因此，数组就被正确地排序了。

```c++
#include <iostream>
#include <vector>

using namespace std;

void sortColors(vector<int> &nums) { // 定义一个函数，用于对一个只包含 0，1，2 的数组进行排序
    int zero = 0, one = 0; // 定义两个变量，分别表示 0 和 1 的右边界
    for (int i = 0; i < nums.size(); i++) { // 遍历数组中的每个元素 i
        int n = nums[i]; // 获取 i 处的元素值 n
        nums[i] = 2; // 将 i 处的元素设为 2
        if (n <= 1) { // 如果 n 小于等于 1
            nums[one] = 1; // 将 one 处的元素设为 1
            one++; // 将 one 加一
        }
        if (n == 0) { // 如果 n 等于 0
            nums[zero] = 0; // 将 zero 处的元素设为 0
            zero++; // 将 zero 加一
        }
    }
}

int main() {
    vector<int> nums = {2, 0, 2, 1, 1, 0}; // 定义一个只包含 0，1，2 的数组
    sortColors(nums); // 对数组进行排序
    for (int i = 0; i < nums.size(); i++) { // 遍历数组中的每个元素 i
        cout << nums[i] << " "; // 输出 i 处的元素值
    }
    cout << endl; // 换行
    return 0; // 返回 0，表示程序正常退出
}
```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：8 MB, 在所有 C++ 提交中击败了80.32%的用户

通过测试用例：87 / 87


```go
package main // 定义包名为 main

func sortColors(nums []int) { // 定义一个函数，用于对一个只包含 0，1，2 的数组进行排序
	zero, one := 0, 0 // 定义两个变量，分别表示 0 和 1 的右边界
	for i, n := range nums { // 遍历数组中的每个元素 i 和它的值 n
		nums[i] = 2 // 将 i 处的元素设为 2
		if n <= 1 { // 如果 n 小于等于 1
			nums[one] = 1 // 将 one 处的元素设为 1
			one++ // 将 one 加一，表示 1 的右边界向右移动一位
		}
		if n == 0 { // 如果 n 等于 0
			nums[zero] = 0 // 将 zero 处的元素设为 0
			zero++ // 将 zero 加一，表示 0 的右边界向右移动一位
		}
	}
}

func main() { // 定义主函数，程序入口点
	var nums = []int{2, 0, 2, 1, 1, 0} // 定义一个整数切片，存放要排序的数组
	sortColors(nums) // 调用 sortColors 函数对数组进行排序
	for _, n := range nums { // 遍历排序后的数组中的每个元素 n
		print(n) // 输出 n 的值
	}
}
```

执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：1.9 MB, 在所有 Go 提交中击败了58.70%的用户

通过测试用例：87 / 87

三指针，先判断 2，再判断 1，最后判断 0。zero和one就是0和1的右边界，i是当前遍历的元素，n是当前遍历的元素的值。

