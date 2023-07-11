# [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

给你一个区间数组，表示一些时间段。你要从中选择一些区间，使得它们不重叠，也就是没有两个区间的交集。返回你最多能选择多少个区间。

代码的思路是这样的：

- 首先判断区间数组是否为空，如果为空，直接返回 0，因为没有可选的区间。
- 然后获取区间数组的大小，用一个变量 n 表示。
- 接着对区间数组进行排序，按照每个区间的右端点从小到大排序，这样可以保证优先选择结束早的区间，留出更多的空间给后面的区间。
- 然后用一个变量 removed 表示需要移除的区间数量，初始为 0，用一个变量 prev 表示上一个选择的区间的右端点，初始为第一个区间的右端点。
- 接着从第二个区间开始遍历区间数组，对于每个区间，如果它的左端点小于 prev，说明它和上一个选择的区间有重叠，那么就需要移除它，让 removed 加一；否则说明它和上一个选择的区间没有重叠，那么就可以选择它，让 prev 更新为它的右端点。
- 最后返回 removed 的值，就是最少需要移除的区间数量，用 n 减去 removed 就是最多能选择的区间数量。

```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件
using namespace std; // 使用标准命名空间

int eraseOverlapIntervals(vector<vector<int>>& intervals) { // 定义一个函数，参数是一个二维整数向量的引用 intervals，返回值是一个整数
    if (intervals.empty()) { // 如果 intervals 为空，表示没有区间
        return 0; // 直接返回 0
    }
    int n = intervals.size(); // 声明一个变量 n，赋值为 intervals 的大小，表示区间的个数
    sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b) // 对 intervals 按照右端点升序排序，使用 lambda 表达式作为比较函数
    {
        return a[1] < b[1]; // 如果 a 的右端点小于 b 的右端点，返回 true，否则返回 false
    });
    int removed = 0, prev = intervals[0][1]; // 声明两个变量 removed 和 prev，分别表示移除的区间个数和上一个保留的区间的右端点，初始分别为 0 和 intervals 的第一个区间的右端点
    for (int i = 1; i < n; ++i) { // 遍历 intervals 中的每个区间，从第二个开始，用变量 i 表示索引
        if (intervals[i][0] < prev) { // 如果当前区间的左端点小于上一个保留的区间的右端点，说明有重叠
            ++removed; // 将 removed 加一，表示移除当前区间
        } else { // 否则说明没有重叠
            prev = intervals[i][1]; // 将 prev 更新为当前区间的右端点，表示保留当前区间
        }
    }
    return removed; // 返回 removed，表示最少需要移除多少个区间
}

int main() { // 定义主函数
    vector<vector<int>> intervals = {{1,2},{2,3},{3,4},{1,3}}; // 定义一个二维向量 intervals，并初始化为 {{1,2},{2,3},{3,4},{1,3}}
    cout << eraseOverlapIntervals(intervals) << endl; // 调用 eraseOverlapIntervals 函数，并输出结果，换行
    return 0; // 返回 0，表示程序正常结束
}

```

执行用时：328 ms, 在所有 C++ 提交中击败了96.23%的用户

内存消耗：87.6 MB, 在所有 C++ 提交中击败了92.74%的用户

通过测试用例：58 / 58

```go
package main

import (
	"fmt"
	"sort"
)

type Intervals [][]int

func (a Intervals) Len() int {
	return len(a)
}
func (a Intervals) Swap(i, j int) {
	a[i], a[j] = a[j], a[i]
}
func (a Intervals) Less(i, j int) bool {
	for k := 0; k < len(a[i]); k++ {
		if a[i][k] < a[j][k] {
			return true
		} else if a[i][k] == a[j][k] {
			continue
		} else {
			return false
		}
	}
	return true
}

func eraseOverlapIntervals(intervals [][]int) int {
	if len(intervals) == 0 {
		return 0
	}
	sort.Sort(Intervals(intervals))
	pre, res := 0, 1
	for i := 1; i < len(intervals); i++ {
		if intervals[i][0] >= intervals[pre][1] {
			res++
			pre = i
		} else if intervals[i][1] < intervals[pre][1] {
			pre = i
		}
	}
	return len(intervals) - res
}

func main() {
	intervals := [][]int{{1, 2}, {2, 3}, {3, 4}, {1, 3}}
	fmt.Println(eraseOverlapIntervals(intervals))
}
```
执行用时：212 ms, 在所有 Go 提交中击败了46.94%的用户

内存消耗：13.6 MB, 在所有 Go 提交中击败了99.18%的用户

通过测试用例：58 / 58

解题就是在选区间时，选择的区间结尾越小，余留给其它区间的空间就越大，就越能保留更多的区间。因此，我们采取的贪心策略为，优先保留结尾小且不相交的区间。先以后面最大的排序。每次选择结尾最早的，且和前一个区间不重叠的区间。选取结尾最早的，就可以给后面留出更大的空间，供后面的区间选择。这样可以保留更多的区间。这种做法是贪心算法的思想。