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
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) {
        return 0;
    }
    int n = intervals.size();
    sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b)
    {
        return a[1] < b[1];
    });
    int removed = 0, prev = intervals[0][1];
    for (int i = 1; i < n; ++i) {
        if (intervals[i][0] < prev) {
            ++removed;
        } else {
            prev = intervals[i][1];
        }
    }
    return removed;
}

int main() {
    vector<vector<int>> intervals = {{1,2},{2,3},{3,4},{1,3}};
    cout << eraseOverlapIntervals(intervals) << endl;
    return 0;
}
```

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
解题就是在选区间时，选择的区间结尾越小，余留给其它区间的空间就越大，就越能保留更多的区间。因此，我们采取的贪心策略为，优先保留结尾小且不相交的区间。先以后面最大的排序。每次选择结尾最早的，且和前一个区间不重叠的区间。选取结尾最早的，就可以给后面留出更大的空间，供后面的区间选择。这样可以保留更多的区间。这种做法是贪心算法的思想。