# [695. Max Area of Island ](https://leetcode.com/problems/max-area-of-island/)

求一个二维矩阵中最大的岛屿面积。岛屿是由1组成的连通区域，0表示水域。代码中有两个函数，一个是主函数，一个是辅函数。主函数遍历矩阵中的每个元素，如果是1，就调用辅函数进行深度优先搜索（dfs），并更新最大面积。辅函数用递归的方式，把当前位置的1变成0，并向四个方向扩展，返回当前岛屿的面积。

```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量库

using namespace std; // 使用标准命名空间
int dfs(vector<vector<int>>& grid, int r, int c); // 声明辅函数
vector<int> direction{-1, 0, 1, 0, -1}; // 定义一个方向数组，用于辅函数中的扩展，(x,y)的四个方向左，上，右，下分别是(x-1,y),(x,y+1),(x+1,y),(x,y-1)
// 主函数
int maxAreaOfIsland(vector<vector<int>>& grid) { // 定义一个函数，输入是一个二维矩阵，输出是一个整数
    if (grid.empty() || grid[0].empty()) return 0; // 如果矩阵为空，直接返回0
    int max_area = 0; // 定义一个变量，用于存储最大面积，初始为0
    for (int i = 0; i < grid.size(); ++i) { // 遍历矩阵的每一行
        for (int j = 0; j < grid[0].size(); ++j) { // 遍历矩阵的每一列
            if (grid[i][j] == 1) { // 如果当前位置是1，表示是岛屿的一部分
                max_area = max(max_area, dfs(grid, i, j)); // 调用辅函数进行深度优先搜索，并更新最大面积
            }
        }
    }
    return max_area; // 返回最大面积
}
// 辅函数
int dfs(vector<vector<int>>& grid, int r, int c) { // 定义一个函数，输入是一个二维矩阵和一个坐标，输出是一个整数
    if (grid[r][c] == 0) return 0; // 如果当前位置是0，表示是水域或已经访问过，直接返回0
    grid[r][c] = 0; // 把当前位置的1变成0，表示已经访问过
    int x, y, area = 1; // 定义三个变量，分别表示新的坐标和当前岛屿的面积，初始为1
    for (int i = 0; i < 4; ++i) { // 遍历四个方向
        x = r + direction[i], y = c + direction[i+1]; // 计算新的坐标
        if (x >= 0 && x < grid.size() && y >= 0 && y < grid[0].size()) { // 如果新的坐标在矩阵范围内
            area += dfs(grid, x, y); // 调用辅函数进行深度优先搜索，并累加当前岛屿的面积
        }
    }
    return area; // 返回当前岛屿的面积
}
int main() {
    vector<vector<int>> grid{{0,0,1,0,0,0,0,1,0,0,0,0,0},
                             {0,0,0,0,0,0,0,1,1,1,0,0,0},
                             {0,1,1,0,1,0,0,0,0,0,0,0,0},
                             {0,1,0,0,1,1,0,0,1,0,1,0,0},
                             {0,1,0,0,1,1,0,0,1,1,1,0,0},
                             {0,0,0,0,0,0,0,0,0,0,1,0,0},
                             {0,0,0,0,0,0,0,1,1,1,0,0,0},
                             {0,0,0,0,0,0,0,1,1,0,0,0,0}};
    cout << maxAreaOfIsland(grid) << endl;
    return 0;
}
```



```go
package main // 定义包名为main

var dir = [][]int{ // 定义一个二维数组，用于表示四个方向
	{-1, 0}, // 上
	{0, 1}, // 右
	{1, 0}, // 下
	{0, -1}, // 左
}

func maxAreaOfIsland(grid [][]int) int { // 定义一个函数，输入是一个二维矩阵，输出是一个整数
	res := 0 // 定义一个变量，用于存储最大面积，初始为0
	for i, row := range grid { // 遍历矩阵的每一行，i是行索引，row是行元素
		for j, col := range row { // 遍历矩阵的每一列，j是列索引，col是列元素
			if col == 0 { // 如果当前位置是0，表示是水域或已经访问过，跳过本次循环
				continue
			}
			area := areaOfIsland(grid, i, j) // 调用辅函数计算当前岛屿的面积
			if area > res { // 如果当前岛屿的面积大于最大面积
				res = area // 更新最大面积
			}
		}
	}
	return res // 返回最大面积
}

func isInGrid(grid [][]int, x, y int) bool { // 定义一个函数，输入是一个二维矩阵和一个坐标，输出是一个布尔值
	return x >= 0 && x < len(grid) && y >= 0 && y < len(grid[0]) // 判断坐标是否在矩阵范围内，返回真或假
}

func areaOfIsland(grid [][]int, x, y int) int { // 定义一个函数，输入是一个二维矩阵和一个坐标，输出是一个整数
	if !isInGrid(grid, x, y) || grid[x][y] == 0 { // 如果坐标不在矩阵范围内或当前位置是0，返回0
		return 0
	}
	grid[x][y] = 0 // 把当前位置的1变成0，表示已经访问过
	total := 1 // 定义一个变量，用于存储当前岛屿的面积，初始为1
	for i := 0; i < 4; i++ { // 遍历四个方向
		nx := x + dir[i][0] // 计算新的横坐标
		ny := y + dir[i][1] // 计算新的纵坐标
		total += areaOfIsland(grid, nx, ny) // 调用辅函数计算新位置的岛屿面积，并累加到总面积中
	}
	return total // 返回当前岛屿的面积
}

func main() { // 定义主程序入口函数
	grid := [][]int{ // 定义一个二维矩阵，用花括号包含每一行的元素，用逗号分隔
		{1, 1, 0, 0, 0},
		{1, 1, 0, 1, 1},
		{0, 0, 0, 1, 1},
		{0, 0, 0, 0, 0},
	}
	println(maxAreaOfIsland(grid)) // 调用函数并打印结果到控制台
}
```
执行用时：12 ms, 在所有 Go 提交中击败了59.36%的用户

内存消耗：4.8 MB, 在所有 Go 提交中击败了68.72%的用户

通过测试用例：728 / 728


深度优先搜索（DFS）是一种用于遍历或搜索树或图的算法。这个算法会尽可能深的搜索树的分支。当节点v的所在边都己被探寻过，搜索将回溯到发现节点v的那条边的起始节点。这一过程一直进行到已发现从源节点可达的所有节点为止。如果还存在未被发现的节点，则选择其中一个作为源节点并重复以上过程，整个进程反复进行直到所有节点都被访问为止。属于盲目搜索。
