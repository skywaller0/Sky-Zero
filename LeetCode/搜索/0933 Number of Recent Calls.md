# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

最短桥问题是指在一个n×n的二进制矩阵grid中，1表示陆地，0表示水域。一个岛屿是一组四方向相连的1，不与其他1相连。grid中恰好有两个岛屿。你可以把0变成1，使得两个岛屿连接成一个岛屿。返回你必须翻转的0的最小数目

代码的主要思路是使用**深度优先搜索（DFS）**和**广度优先搜索（BFS）**的组合。首先使用DFS找到第一个岛屿，并把所有的1变成2，表示已访问过。然后使用BFS从第一个岛屿出发，逐层扩展，直到遇到第二个岛屿，此时的层数就是最短桥的长度。

代码中使用了一个一维整数数组`direction`来表示四个方向，即上下左右。代码中定义了三个函数：`shortestBridge`，`dfs`和`generateBoard`。

代码中的`shortestBridge`函数是主函数，它接受一个二维整数数组`grid`作为参数，返回一个整数表示最短桥的长度。它首先获取矩阵的行数和列数，并定义一个队列`points`用来存储第一个岛屿的边界点。然后使用一个布尔变量`flipped`来标记是否已经找到第一个岛屿，并用双重循环遍历矩阵中的每个元素。如果遇到一个值为1的元素，就调用`dfs`函数，把它所在的岛屿所有的1都变成2，并把边界点加入到队列中，然后把`flipped`设为真，并跳出循环。接下来使用一个整数变量`level`来记录当前的层数，并初始化为0。然后使用一个循环来进行BFS操作，直到队列为空或者找到第二个岛屿为止。在每一轮循环中，先把`level`加一，然后获取当前队列的大小，并用另一个循环来处理当前队列中的所有点。对于每个点，先把它从队列中弹出，并把它所在位置设为2，表示已访问过。然后检查它的四个邻居，如果邻居是有效的并且值为0，就把邻居加入到队列中，并把邻居所在位置设为2；如果邻居是有效的并且值为1，就说明找到了第二个岛屿，此时返回当前的`level`作为答案。如果循环结束后还没有找到第二个岛屿，就返回0。



```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量头文件
#include <queue> // 引入队列头文件

using namespace std; // 使用标准命名空间
vector<int> direction{-1, 0, 1, 0, -1}; // 定义一个一维整数向量direction，用来表示四个方向，即上下左右
void dfs(queue<pair<int, int>>& points, vector<vector<int>>& grid, int m, int n
        , int i, int j); // 声明dfs函数，用来寻找第一个岛屿，并把1全部赋值为2
// 主函数
int shortestBridge(vector<vector<int>>& grid) { // 定义一个函数shortestBridge，接受一个二维整数向量grid作为参数，返回一个整数表示最短桥的长度
    int m = grid.size(), n = grid[0].size(); // 定义两个整数m和n，分别表示grid的行数和列数
    queue<pair<int, int>> points; // 定义一个队列points，用来存储第一个岛屿的边界点
// dfs寻找第一个岛屿，并把1全部赋值为2
    bool flipped = false; // 定义一个布尔变量flipped，用来标记是否已经找到第一个岛屿，并初始化为false
    for (int i = 0; i < m; ++i) { // 用一个循环遍历grid的每一行
        if (flipped) break; // 如果已经找到第一个岛屿，就跳出循环
        for (int j = 0; j < n; ++j) { // 用另一个循环遍历grid的每一列
            if (grid[i][j] == 1) { // 如果当前位置是陆地，就调用dfs函数，把它所在的岛屿所有的1都变成2，并把边界点加入到队列中
                dfs(points, grid, m, n, i, j);
                flipped = true; // 把flipped设为true，表示已经找到第一个岛屿
                break; // 跳出循环
            }
        }
    }
// bfs寻找第二个岛屿，并把过程中经过的0赋值为2
    int x, y; // 定义两个整数x和y，用来表示当前要访问的位置的坐标
    int level = 0; // 定义一个整数level，用来记录当前的层数，并初始化为0
    while (!points.empty()){ // 使用一个循环进行bfs操作，直到队列为空或者找到第二个岛屿为止
        ++level; // 把level加一，表示进入下一层
        int n_points = points.size(); // 定义一个整数n_points，表示当前队列中的点的个数，并获取队列的大小赋值给它
        while (n_points--) { // 使用另一个循环处理当前队列中的所有点
            auto [r, c] = points.front(); // 定义两个整数r和c，分别表示队首元素的行号和列号，并获取队首元素赋值给它们
            grid[r][c] = 2; // 把当前位置设为2，表示已访问过
            points.pop(); // 把队首元素弹出队列
            for (int k = 0; k < 4; ++k) { // 使用又一个循环遍历当前位置的四个邻居
                x = r + direction[k], y = c + direction[k+1]; // 计算邻居的坐标并赋值给x和y
                if (x >= 0 && y >= 0 && x < m && y < n) { // 如果邻居是有效的，即没有越界
                    if (grid[x][y] == 2) { // 如果邻居已经访问过，就跳过这个邻居，继续下一个邻居
                        continue;
                    }
                    if (grid[x][y] == 1) { // 如果邻居是陆地，就说明找到了第二个岛屿，此时返回当前的level作为答案
                        return level;
                    }
                    points.push({x, y}); // 如果邻居是水域，就把它加入到队列中
                    grid[x][y] = 2; // 把邻居所在位置设为2，表示已访问过
                }
            }
        }
    }
    return 0; // 如果循环结束后还没有找到第二个岛屿，就返回0
}
// 辅函数
void dfs(queue<pair<int, int>>& points, vector<vector<int>>& grid, int m, int n
        , int i, int j) { // 定义一个函数dfs，接受六个参数：一个队列points，用来存储第一个岛屿的边界点；一个二维整数向量grid，用来表示矩阵；两个整数m和n，用来表示矩阵的行数和列数；两个整数i和j，用来表示当前要访问的位置
    if (i < 0 || j < 0 || i == m || j == n || grid[i][j] == 2) { // 如果当前位置越界或者已经访问过，就直接返回
        return;
    }
    if (grid[i][j] == 0) { // 如果当前位置是水域，就把它加入到队列中，并返回
        points.push({i, j});
        return;
    }
    grid[i][j] = 2; // 如果当前位置是陆地，就把它设为2，表示已访问过
    dfs(points, grid, m, n, i - 1, j); // 递归地访问上方的位置
    dfs(points, grid, m, n, i + 1, j); // 递归地访问下方的位置
    dfs(points, grid, m, n, i, j - 1); // 递归地访问左方的位置
    dfs(points, grid, m, n, i, j + 1); // 递归地访问右方的位置
}

int main() { // 定义主函数
    vector<vector<int>> grid{{0,1,0},{0,0,0},{0,0,1}}; // 定义一个二维整数向量grid，并初始化为一个示例矩阵
    cout << shortestBridge(grid) << endl; // 调用shortestBridge函数，并把结果输出到控制台，并换行
    return 0; // 程序正常结束，返回0
}

```
执行用时：36 ms, 在所有 C++ 提交中击败了82.85%的用户

内存消耗：18.5 MB, 在所有 C++ 提交中击败了71.04%的用户

通过测试用例：97 / 97


```go

```
执行用时：20 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：7 MB, 在所有 Go 提交中击败了78.13%的用户

通过测试用例：97 / 97



