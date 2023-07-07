# [452. Minimum Number Of Arrows To Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

给定一些气球的水平位置，用最少的箭射爆所有的气球。每个气球用一个二维数组表示，例如 [2, 3] 表示气球的左端点是 2，右端点是 3。如果一支箭射在某个气球的右端点或者左端点，也可以射爆这个气球。函数的参数是一个二维向量，表示所有气球的位置。函数的返回值是一个整数，表示最少需要多少支箭。

函数的思路是先按照气球的左端点从小到大排序，然后从左到右遍历所有的气球，用一个变量 result 记录需要的箭数，初始为 1。对于每个气球，判断它和前一个气球是否有重叠部分，如果没有重叠，说明需要一支新的箭，result 加一；如果有重叠，说明可以用同一支箭射爆它们，但是要更新重叠部分的最小右端点，以便后面的气球能够尽可能多地重叠。最后返回 result 即可。
```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件

using namespace std; // 使用标准命名空间

static bool cmp(const vector<int> &a, const vector<int> &b) { // 定义一个静态函数，参数是两个整数向量的常引用 a 和 b，返回值是一个布尔值
    return a[0] < b[0]; // 比较 a 和 b 的第一个元素，如果 a 的第一个元素小于 b 的第一个元素，返回 true，否则返回 false
}

int findMinArrowShots(vector<vector<int>> &points) { // 定义一个函数，参数是一个二维整数向量的引用 points，返回值是一个整数
    if (points.size() == 0) return 0; // 如果 points 的大小为 0，表示没有气球，直接返回 0
    int result = 1; // 声明一个变量 result，表示需要的箭数，初始为 1
    sort(points.begin(), points.end(), cmp); // 对 points 按照第一个元素升序排序，使用自定义的比较函数 cmp
    for (int i = 1; i < points.size(); i++) { // 遍历 points 中的每个向量，从第二个开始，用变量 i 表示索引
        /* 判断气球 0 和气球 1 的边界是否重叠 */
        if (points[i - 1][1] < points[i][0]) result++; // 如果前一个气球的右端点小于当前气球的左端点，说明没有重叠，需要一支新的箭，将 result 加一
            /* 更新重叠气球最小右边界 */
        else points[i][1] = min(points[i][1], points[i - 1][1]); // 否则说明有重叠，可以用同一支箭射爆它们，但是要更新当前气球的右端点为它和前一个气球右端点的最小值，以便后面的气球能够尽可能多地重叠
    }
    return result; // 返回 result
}

int main() { // 定义主函数
    vector<vector<int>> points = {{10, 16},
                                  {2,  8},
                                  {1,  6},
                                  {7,  12}}; // 定义一个二维向量 points，并初始化为 {{10,16},{2,8},{1,6},{7,12}}
    cout << findMinArrowShots(points) << endl; // 调用 findMinArrowShots 函数，并输出结果，换行
    return 0; // 返回 0，表示程序正常结束
}

```



```go
package main

import (
	"fmt"
	"sort"
)

func findMinArrowShots(points [][]int) int {
	if len(points) == 0 {
		return 0
	}
	result := 1
	sort.Slice(points, func(i, j int) bool {
		return points[i][0] < points[j][0]
	})
	for i := 1; i < len(points); i++ {
		// 判断气球0和气球1的边界是否重叠
		if points[i-1][1] < points[i][0] {
			result++
		} else {
			// 更新重叠气球最小右边界
			points[i][1] = min(points[i][1], points[i-1][1])
		}
	}
	return result
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	fmt.Println(findMinArrowShots([][]int{{10, 16}, {2, 8}, {1, 6}, {7, 12}}))
}
```


这个题目大致的思路就是这样，但是这个题目还有一个坑，就是当气球的起始位置和结束位置都是整数的最小值时，会出现溢出的情况，导致结果错误。因此，我们需要对这种情况进行特殊处理，
