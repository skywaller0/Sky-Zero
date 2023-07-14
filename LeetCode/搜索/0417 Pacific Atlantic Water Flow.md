# [417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

求一个二维矩阵中哪些位置的水能同时流向太平洋和大西洋。矩阵中的元素表示高度，水只能从高处流向低处或同高处。代码中有两个函数，一个是主函数，一个是辅函数。主函数遍历矩阵中的每个位置，用两个布尔矩阵分别记录能否流向太平洋和大西洋，然后从四条边界开始调用辅函数进行深度优先搜索（dfs），找出所有能流向对应海洋的位置，并把它们标记为真。最后遍历两个布尔矩阵，找出同时为真的位置，即能流向两个海洋的位置，并把它们加入到答案中。辅函数用递归的方式，把当前位置标记为真，并向四个方向扩展，如果新的位置在矩阵范围内，并且高度不低于当前位置，就继续调用辅函数进行深度优先搜索。


```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量库

using namespace std; // 使用标准命名空间

void dfs(const vector<vector<int>> &matrix, vector<vector<bool>> &can_reach,
         int r, int c); // 声明辅函数

vector<int> direction{-1, 0, 1, 0, -1}; // 定义一个方向数组，用于辅函数中的扩展

// 主函数
vector<vector<int>> pacificAtlantic(vector<vector<int>> &matrix) { // 定义一个函数，输入是一个二维矩阵，输出是一个二维向量
    if (matrix.empty() || matrix[0].empty()) { // 如果矩阵为空，直接返回空向量
        return {};
    }
    vector<vector<int>> ans; // 定义一个二维向量，用于存储答案
    int m = matrix.size(), n = matrix[0].size(); // 定义两个变量，分别表示矩阵的行数和列数
    vector<vector<bool>> can_reach_p(m, vector<bool>(n, false)); // 定义一个布尔矩阵，用于记录每个位置能否流向太平洋，初始为false
    vector<vector<bool>> can_reach_a(m, vector<bool>(n, false)); // 定义一个布尔矩阵，用于记录每个位置能否流向大西洋，初始为false
    for (int i = 0; i < m; ++i) { // 遍历矩阵的每一行，i表示行索引
        dfs(matrix, can_reach_p, i, 0); // 从第一列开始调用辅函数进行深度优先搜索，找出所有能流向太平洋的位置，并把它们标记为true
        dfs(matrix, can_reach_a, i, n - 1); // 从最后一列开始调用辅函数进行深度优先搜索，找出所有能流向大西洋的位置，并把它们标记为true
    }
    for (int i = 0; i < n; ++i) { // 遍历矩阵的每一列，i表示列索引
        dfs(matrix, can_reach_p, 0, i); // 从第一行开始调用辅函数进行深度优先搜索，找出所有能流向太平洋的位置，并把它们标记为true
        dfs(matrix, can_reach_a, m - 1, i); // 从最后一行开始调用辅函数进行深度优先搜索，找出所有能流向大西洋的位置，并把它们标记为true
    }
    for (int i = 0; i < m; i++) { // 遍历矩阵的每一行，i表示行索引
        for (int j = 0; j < n; ++j) { // 遍历矩阵的每一列，j表示列索引
            if (can_reach_p[i][j] && can_reach_a[i][j]) { // 如果当前位置既能流向太平洋又能流向大西洋
                ans.push_back(vector<int>{i, j}); // 把当前位置的坐标加入到答案中
            }
        }
    }
    return ans; // 返回答案
}

// 辅函数
void dfs(const vector<vector<int>> &matrix, vector<vector<bool>> &can_reach,
         int r, int c) { // 定义一个函数，输入是一个二维矩阵，一个布尔矩阵和一个坐标，无输出
    if (can_reach[r][c]) { // 如果当前位置已经被访问过，直接返回
        return;
    }
    can_reach[r][c] = true; // 把当前位置标记为已访问
    int x, y; // 定义两个变量，用于表示新的坐标
    for (int i = 0; i < 4; ++i) { // 遍历四个方向
        x = r + direction[i], y = c + direction[i + 1]; // 计算新的坐标
        if (x >= 0 && x < matrix.size()
            && y >= 0 && y < matrix[0].size() &&
            matrix[r][c] <= matrix[x][y]) { // 如果新的坐标在矩阵范围内，并且高度不低于当前位置
            dfs(matrix, can_reach, x, y); // 调用辅函数进行深度优先搜索，找出所有能流向同一个海洋的位置，并把它们标记为已访问
        }
    }
}

int main() { // 定义主程序入口函数
    vector<vector<int>> matrix = {{1, 2, 2, 3, 5}, // 定义一个二维矩阵，用花括号包含每一行的元素，用逗号分隔
                                  {3, 2, 3, 4, 4},
                                  {2, 4, 5, 3, 1},
                                  {6, 7, 1, 4, 5},
                                  {5, 1, 1, 2, 4}};
    vector<vector<int>> ans = pacificAtlantic(matrix); // 调用函数并把结果赋值给一个二维向量
    for (auto &i: ans) { // 遍历答案中的每一个元素，i表示一个坐标
        cout << i[0] << " " << i[1] << endl; // 打印坐标到控制台，用空格分隔
    }
    return 0; // 返回0表示程序正常结束
}
```
执行用时：40 ms, 在所有 C++ 提交中击败了50.16%的用户

内存消耗：16.9 MB, 在所有 C++ 提交中击败了91.81%的用户

通过测试用例：113 / 113


```go
package main // 定义包名为main

import (
	"fmt" // 引入格式化输出库
	"math" // 引入数学库
)

func pacificAtlantic(matrix [][]int) [][]int { // 定义一个函数，输入是一个二维矩阵，输出是一个二维切片
	if len(matrix) == 0 || len(matrix[0]) == 0 { // 如果矩阵为空，直接返回nil
		return nil
	}
	row, col, res := len(matrix), len(matrix[0]), make([][]int, 0) // 定义三个变量，分别表示矩阵的行数，列数和答案，答案初始为空切片
	pacific, atlantic := make([][]bool, row), make([][]bool, row) // 定义两个布尔矩阵，分别表示每个位置能否流向太平洋和大西洋，初始为false
	for i := 0; i < row; i++ { // 遍历矩阵的每一行，i表示行索引
		pacific[i] = make([]bool, col) // 初始化太平洋矩阵的每一行
		atlantic[i] = make([]bool, col) // 初始化大西洋矩阵的每一行
	}
	for i := 0; i < row; i++ { // 遍历矩阵的每一行，i表示行索引
		dfs(matrix, i, 0, &pacific, math.MinInt32) // 从第一列开始调用辅函数进行深度优先搜索，找出所有能流向太平洋的位置，并把它们标记为true
		dfs(matrix, i, col-1, &atlantic, math.MinInt32) // 从最后一列开始调用辅函数进行深度优先搜索，找出所有能流向大西洋的位置，并把它们标记为true
	}
	for j := 0; j < col; j++ { // 遍历矩阵的每一列，j表示列索引
		dfs(matrix, 0, j, &pacific, math.MinInt32) // 从第一行开始调用辅函数进行深度优先搜索，找出所有能流向太平洋的位置，并把它们标记为true
		dfs(matrix, row-1, j, &atlantic, math.MinInt32) // 从最后一行开始调用辅函数进行深度优先搜索，找出所有能流向大西洋的位置，并把它们标记为true
	}
	for i := 0; i < row; i++ { // 遍历矩阵的每一行，i表示行索引
		for j := 0; j < col; j++ { // 遍历矩阵的每一列，j表示列索引
			if atlantic[i][j] && pacific[i][j] { // 如果当前位置既能流向太平洋又能流向大西洋
				res = append(res, []int{i, j}) // 把当前位置的坐标加入到答案中
			}
		}
	}
	return res // 返回答案
}

func dfs(matrix [][]int, row, col int, visited *[][]bool, height int) { // 定义一个函数，输入是一个二维矩阵，一个坐标，一个布尔矩阵和一个高度，无输出
	if row < 0 || row >= len(matrix) || col < 0 || col >= len(matrix[0]) { // 如果坐标不在矩阵范围内，直接返回
		return
	}
	if (*visited)[row][col] || matrix[row][col] < height { // 如果当前位置已经被访问过或高度低于之前的位置，直接返回
		return
	}
	(*visited)[row][col] = true // 把当前位置标记为已访问
	dfs(matrix, row+1, col, visited, matrix[row][col]) // 向下调用辅函数进行深度优先搜索，找出所有能流向同一个海洋的位置，并把它们标记为已访问
	dfs(matrix, row-1, col, visited, matrix[row][col]) // 向上调用辅函数进行深度优先搜索，找出所有能流向同一个海洋的位置，并把它们标记为已访问
	dfs(matrix, row, col+1, visited, matrix[row][col]) // 向右调用辅函数进行深度优先搜索，找出所有能流向同一个海洋的位置，并把它们标记为已访问
	dfs(matrix, row, col-1, visited, matrix[row][col]) // 向左调用辅函数进行深度优先搜索，找出所有能流向同一个海洋的位置，并把它们标记为已访问
}
func main() { // 定义主程序入口函数
	matrix := [][]int{ // 定义一个二维矩阵，用花括号包含每一行的元素，用逗号分隔
		{1, 2, 2, 3, 5},
		{3, 2, 3, 4, 4},
		{2, 4, 5, 3, 1},
		{6, 7, 1, 4, 5},
		{5, 1, 1, 2, 4},
	}
	for _, v := range pacificAtlantic(matrix) { // 遍历答案中的每一个元素，v表示一个坐标
		fmt.Println(v) // 打印坐标到控制台，用换行符分隔
	}
}
```
执行用时：28 ms, 在所有 Go 提交中击败了81.58%的用户

内存消耗：6.7 MB, 在所有 Go 提交中击败了92.11%的用户

通过测试用例：113 / 113


虽然题目要求的是满足向下流能到达两个大洋的位置，如果我们对所有的位置进行搜索，那
么在不剪枝的情况下复杂度会很高。因此我们可以反过来想，从两个大洋开始向上流，这样我们
只需要对矩形四条边进行搜索。搜索完成后，只需遍历一遍矩阵，满足条件的位置即为两个大洋
向上流都能到达的位置。

