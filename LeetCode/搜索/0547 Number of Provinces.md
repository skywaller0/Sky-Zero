# [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
求一个二维矩阵中有多少个朋友圈。朋友圈是指一组互相认识的人，矩阵中的元素表示两个人是否是朋友，1表示是，0表示不是。代码中有两个函数，一个是主函数，一个是辅函数。主函数遍历矩阵中的每个人，如果没有被访问过，就调用辅函数进行深度优先搜索（dfs），找出所有和他是朋友的人，并把他们标记为已访问，然后增加朋友圈的数量。辅函数用递归的方式，把当前人的朋友都访问一遍，并把他们标记为已访问。最后返回朋友圈的数量。


```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量库

using namespace std; // 使用标准命名空间
void dfs(vector<vector<int>>& friends, int i, vector<bool>& visited); // 声明辅函数
int findCircleNum(vector<vector<int>>& friends) { // 定义一个函数，输入是一个二维矩阵，输出是一个整数
    int n = friends.size(), count = 0; // 定义两个变量，分别表示矩阵的大小和朋友圈的数量，初始为0
    vector<bool> visited(n, false); // 定义一个布尔向量，用于记录每个人是否被访问过，初始为false
    for (int i = 0; i < n; ++i) { // 遍历矩阵的每一行，i表示人的编号
        if (!visited[i]) { // 如果当前人没有被访问过
            dfs(friends, i, visited); // 调用辅函数进行深度优先搜索，找出所有和他是朋友的人，并把他们标记为已访问
            ++count; // 增加朋友圈的数量
        }
    }
    return count; // 返回朋友圈的数量
}
// 辅函数
void dfs(vector<vector<int>>& friends, int i, vector<bool>& visited) { // 定义一个函数，输入是一个二维矩阵，一个编号和一个布尔向量，无输出
    visited[i] = true; // 把当前人标记为已访问
    for (int k = 0; k < friends.size(); ++k) { // 遍历矩阵的每一列，k表示另一个人的编号
        if (friends[i][k] == 1 && !visited[k]) { // 如果当前人和另一个人是朋友，并且另一个人没有被访问过
            dfs(friends, k, visited); // 调用辅函数进行深度优先搜索，找出所有和另一个人是朋友的人，并把他们标记为已访问
        }
    }
}

int main() { // 定义主程序入口函数
    vector<vector<int>> friends = {{1,1,0},{1,1,0},{0,0,1}}; // 定义一个二维矩阵，用花括号包含每一行的元素，用逗号分隔
    cout << findCircleNum(friends) << endl; // 调用函数并打印结果到控制台
    return 0; // 返回0表示程序正常结束
}
```



```go
package main // 定义包名为main

func findCircleNum(M [][]int) int { // 定义一个函数，输入是一个二维矩阵，输出是一个整数
	if len(M) == 0 { // 如果矩阵为空，直接返回0
		return 0
	}
	visited := make([]bool, len(M)) // 定义一个布尔切片，用于记录每个人是否被访问过，初始为false
	res := 0 // 定义一个变量，用于存储朋友圈的数量，初始为0
	for i := range M { // 遍历矩阵的每一行，i表示人的编号
		if !visited[i] { // 如果当前人没有被访问过
			dfs(M, i, visited) // 调用辅函数进行深度优先搜索，找出所有和他是朋友的人，并把他们标记为已访问
			res++ // 增加朋友圈的数量
		}
	}
	return res // 返回朋友圈的数量
}

func dfs(M [][]int, cur int, visited []bool) { // 定义一个函数，输入是一个二维矩阵，一个编号和一个布尔切片，无输出
	visited[cur] = true // 把当前人标记为已访问
	for j := 0; j < len(M[cur]); j++ { // 遍历矩阵的每一列，j表示另一个人的编号
		if !visited[j] && M[cur][j] == 1 { // 如果当前人和另一个人是朋友，并且另一个人没有被访问过
			dfs(M, j, visited) // 调用辅函数进行深度优先搜索，找出所有和另一个人是朋友的人，并把他们标记为已访问
		}
	}
}

func main() { // 定义主程序入口函数
	M := [][]int{{1, 1, 0}, {1, 1, 0}, {0, 0, 1}} // 定义一个二维矩阵，用花括号包含每一行的元素，用逗号分隔
	println(findCircleNum(M)) // 调用函数并打印结果到控制台
}
```
执行用时：28 ms, 在所有 Go 提交中击败了17.78%的用户

内存消耗：6.6 MB, 在所有 Go 提交中击败了26.82%的用户

通过测试用例：114 / 114


这道题目跟695岛屿差不多，只要先每行遍历，然后属于同一个圈子的全部变为true，然后深度优先，再看第二个是false的，再变为true，cout一直加一，直到全部变为true，就是圈子的数量了。

