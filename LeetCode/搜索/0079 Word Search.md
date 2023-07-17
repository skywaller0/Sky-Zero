# [79. Word Search](https://leetcode.com/problems/word-search/)
在一个二维字符矩阵中寻找是否存在一个给定的单词。它使用了回溯法，即从每个位置开始，沿着上下左右四个方向搜索，如果匹配了单词的第一个字母，就继续搜索下一个字母，直到找到整个单词或者遇到边界或者已经访问过的位置。如果找到了单词，就返回true，否则返回false。


```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量容器头文件

using namespace std; // 使用标准命名空间

void backtracking(int i, int j, vector<vector<char>> &board, string &word, bool &find, vector<vector<bool>> &visited,
                  int pos); // 声明回溯函数

bool exist(vector<vector<char>> &board, string word) { // 定义一个函数，判断给定的二维字符矩阵中是否存在给定的单词
    if (board.empty()) return false; // 如果矩阵为空，返回false
    int m = board.size(), n = board[0].size(); // 获取矩阵的行数和列数
    vector<vector<bool>> visited(m, vector<bool>(n, false)); // 创建一个同样大小的二维布尔矩阵，记录每个位置是否被访问过
    bool find = false; // 创建一个布尔变量，记录是否找到单词
    for (int i = 0; i < m; ++i) { // 遍历矩阵的每一行
        for (int j = 0; j < n; ++j) { // 遍历矩阵的每一列
            backtracking(i, j, board, word, find, visited, 0); // 从当前位置开始回溯搜索单词
        }
    }
    return find; // 返回是否找到单词
}

// 辅函数
void backtracking(int i, int j, vector<vector<char>> &board, string &word, bool
&find, vector<vector<bool>> &visited, int pos) { // 定义一个辅助函数，用于回溯搜索单词
    if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size()) { // 如果当前位置越界，直接返回
        return;
    }
    if (visited[i][j] || find || board[i][j] != word[pos]) { // 如果当前位置已经被访问过，或者已经找到单词，或者当前字符不匹配单词的对应位置，直接返回
        return;
    }
    if (pos == word.size() - 1) { // 如果已经匹配到单词的最后一个字符，说明找到了单词，将find设为true并返回
        find = true;
        return;
    }
    visited[i][j] = true; // 修改当前节点状态为已访问
// 递归子节点，分别向上下左右四个方向搜索
    backtracking(i + 1, j, board, word, find, visited, pos + 1);
    backtracking(i - 1, j, board, word, find, visited, pos + 1);
    backtracking(i, j + 1, board, word, find, visited, pos + 1);
    backtracking(i, j - 1, board, word, find, visited, pos + 1);
    visited[i][j] = false; // 回改当前节点状态为未访问
}

int main() { // 主函数
    vector<vector<char>> board = {{'A', 'B', 'C', 'E'}, // 初始化一个二维字符矩阵
                                  {'S', 'F', 'C', 'S'},
                                  {'A', 'D', 'E', 'E'}};
    string word = "ABCCED"; // 初始化一个待搜索的单词
    cout << exist(board, word) << endl; // 调用exist函数，并输出结果到控制台
    return 0; // 返回0表示程序正常结束
}
```
执行用时：344 ms, 在所有 C++ 提交中击败了78.13%的用户

内存消耗：7.8 MB, 在所有 C++ 提交中击败了79.09%的用户

通过测试用例：85 / 85


```go
package main // 声明包名为main

var dir = [][]int{ // 定义一个二维整数数组，表示上下左右四个方向的偏移量
	[]int{-1, 0},
	[]int{0, 1},
	[]int{1, 0},
	[]int{0, -1},
}

func exist(board [][]byte, word string) bool { // 定义一个函数，判断给定的二维字节矩阵中是否存在给定的单词
	visited := make([][]bool, len(board)) // 创建一个同样大小的二维布尔矩阵，记录每个位置是否被访问过
	for i := 0; i < len(visited); i++ {
		visited[i] = make([]bool, len(board[0]))
	}
	for i, v := range board { // 遍历矩阵的每一行
		for j := range v { // 遍历矩阵的每一列
			if searchWord(board, visited, word, 0, i, j) { // 从当前位置开始搜索单词
				return true // 如果找到了单词，返回true
			}
		}
	}
	return false // 如果没有找到单词，返回false
}

func isInBoard(board [][]byte, x, y int) bool { // 定义一个函数，判断给定的坐标是否在矩阵范围内
	return x >= 0 && x < len(board) && y >= 0 && y < len(board[0])
}

func searchWord(board [][]byte, visited [][]bool, word string, index, x, y int) bool { // 定义一个辅助函数，用于搜索单词
	if index == len(word)-1 { // 如果已经匹配到单词的最后一个字符，返回当前字符是否与之相等
		return board[x][y] == word[index]
	}
	if board[x][y] == word[index] { // 如果当前字符与单词的对应位置相等
		visited[x][y] = true // 修改当前节点状态为已访问
		for i := 0; i < 4; i++ { // 遍历四个方向
			nx := x + dir[i][0] // 计算下一个节点的横坐标
			ny := y + dir[i][1] // 计算下一个节点的纵坐标
			if isInBoard(board, nx, ny) && !visited[nx][ny] && searchWord(board, visited, word, index+1, nx, ny) { // 如果下一个节点在矩阵内，且未被访问过，且能够匹配到单词的下一个字符，返回true
				return true
			}
		}
		visited[x][y] = false // 回改当前节点状态为未访问
	}
	return false // 如果当前字符与单词的对应位置不相等，或者没有找到匹配的路径，返回false
}

func main() { // 主函数
	board := [][]byte{ // 初始化一个二维字节矩阵
		[]byte{'a', 'b', 'c', 'e'},
		[]byte{'s', 'f', 'c', 's'},
		[]byte{'a', 'd', 'e', 'e'},
	}
	word := "abcced" // 初始化一个待搜索的单词
	println(exist(board, word)) // 调用exist函数，并输出结果到控制台
}
```

执行用时：168 ms, 在所有 Go 提交中击败了32.17%的用户

内存消耗：1.9 MB, 在所有 Go 提交中击败了38.46%的用户

通过测试用例：85 / 85


回溯，一直讨论状态，没什么好解释的。
