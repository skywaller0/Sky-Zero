# [655. Non Decreasing Array](https://leetcode.cn/problems/non-decreasing-array/)
判断一个整数数组是否可以通过修改最多一个元素变成非递减的。它的参数是一个整数数组 nums，它的返回值是一个布尔值，表示是否可以修改。它的逻辑是这样的：

- 定义一个变量 count，用来记录修改的次数，初始为 0
- 遍历数组 nums，从第 0 个元素到倒数第二个元素，用 i 表示下标
- 如果当前元素 nums[i] 大于下一个元素 nums[i+1]，说明不满足非递减的条件，需要修改
- 将 count 加一，表示修改了一次
- 如果 count 大于 1，说明修改了多次，直接返回 false，表示不可能修改成非递减的
- 如果 i 大于 0，说明不是第一个元素，需要考虑前一个元素 nums[i-1] 的影响
- 如果下一个元素 nums[i+1] 小于前一个元素 nums[i-1]，说明修改当前元素 nums[i] 不够，还需要修改下一个元素 nums[i+1]
- 将下一个元素 nums[i+1] 赋值为当前元素 nums[i]，相当于删除了一个递减的部分
- 遍历结束后，返回 true，表示可以修改成非递减的


```c++
// 引入输入输出流，向量，算法等头文件
#include <iostream>
#include <vector>
#include <algorithm>

// 使用标准命名空间
using namespace std;

// 定义一个函数，判断一个整数向量是否可以通过修改最多一个元素变成非递减的
bool checkPossibility(vector<int> &nums) {
    // 定义一个变量，记录修改的次数，初始为 0
    int count = 0;
    // 遍历向量，从第 0 个元素到倒数第二个元素，用 i 表示下标
    for (int i = 0; i < nums.size() - 1; i++) {
        // 如果当前元素大于下一个元素，说明不满足非递减的条件，需要修改
        if (nums[i] > nums[i + 1]) {
            // 将 count 加一，表示修改了一次
            count++;
            // 如果 count 大于 1，说明修改了多次，直接返回 false，表示不可能修改成非递减的
            if (count > 1) {
                return false;
            }
            // 如果 i 大于 0，说明不是第一个元素，需要考虑前一个元素的影响
            if (i > 0 && nums[i + 1] < nums[i - 1]) {
                // 将下一个元素赋值为当前元素，相当于删除了一个递减的部分
                nums[i + 1] = nums[i];
            }
        }
    }
    // 遍历结束后，返回 true，表示可以修改成非递减的
    return true;
}

// 定义主函数
int main() {
    // 定义一个向量，初始化为 {4, 2, 3}
    vector<int> nums = {4, 2, 3};
    // 调用 checkPossibility 函数，并输出结果到控制台
    cout << checkPossibility(nums) << endl;
    // 返回 0，表示程序正常结束
    return 0;
}
```
执行用时：24 ms, 在所有 C++ 提交中击败了56.19%的用户

内存消耗：26.5 MB, 在所有 C++ 提交中击败了8.97%的用户

通过测试用例：335 / 335


```go
// 定义一个包名为 main 的包
package main

// 引入 fmt 包，用于格式化输出
import "fmt"

// 定义一个函数，判断一个整数切片是否可以通过修改最多一个元素变成非递减的
func checkPossibility(nums []int) bool {
	// 定义一个变量，记录修改的次数，初始为 0
	count := 0
	// 遍历切片，从第 0 个元素到倒数第二个元素，用 i 表示下标
	for i := 0; i < len(nums)-1; i++ {
		// 如果当前元素大于下一个元素，说明不满足非递减的条件，需要修改
		if nums[i] > nums[i+1] {
			// 将 count 加一，表示修改了一次
			count++
			// 如果 count 大于 1，说明修改了多次，直接返回 false，表示不可能修改成非递减的
			if count > 1 {
				return false
			}
			// 如果 i 大于 0，说明不是第一个元素，需要考虑前一个元素的影响
			if i > 0 && nums[i+1] < nums[i-1] {
				// 将下一个元素赋值为当前元素，相当于删除了一个递减的部分
				nums[i+1] = nums[i]
			}
		}
	}
	// 遍历结束后，返回 true，表示可以修改成非递减的
	return true
}

// 定义主函数
func main() {
	// 调用 checkPossibility 函数，并输出结果到控制台
	fmt.Println(checkPossibility([]int{2, 3, 4, 1, 3, 4}))
}
```
执行用时：16 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：6.3 MB, 在所有 Go 提交中击败了74.00%的用户

通过测试用例：335 / 335

循环扫描数组，找到 `nums[i] > nums[i+1]` 这种递减组合。一旦这种组合超过 2 组，直接返回 false。找到第一组递减组合，需要手动调节一次。如果 `nums[i + 1] < nums[i - 1]`，就算交换 `nums[i+1]` 和 `nums[i]`，交换结束，`nums[i - 1]` 仍然可能大于 `nums[i + 1]`，不满足题意。正确的做法应该是让较小的那个数变大，即 `nums[i + 1] = nums[i]`。两个元素相等满足非递减的要求。


