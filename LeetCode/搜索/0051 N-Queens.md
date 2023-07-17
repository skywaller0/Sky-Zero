# [51. N-Queens](https://leetcode.com/problems/n-queens/)

**回溯法**，从左到右依次尝试在每一列放置一个皇后，同时检查是否和已经放置的皇后有冲突。如果没有冲突，就继续递归地在下一列放置皇后。如果有冲突，就回溯到上一列，把皇后移动到下一行。如果所有的行都试过了，就说明当前列无法放置皇后，返回失败。如果所有的列都成功放置了皇后，就打印出棋盘的解法。

代码中使用了一个二维数组`board`来表示棋盘，其中`board[i][j]`为1表示第i行第j列有一个皇后，为0表示没有。还使用了三个一维数组`column`，`ldiag`和`rdiag`来记录每一列，每一条左对角线和每一条右对角线上是否有皇后。其中`column[i]`为真表示第i列有皇后，为假表示没有；`ldiag[i]`为真表示从右上到左下的第i条对角线有皇后，为假表示没有；`rdiag[i]`为真表示从左上到右下的第i条对角线有皇后，为假表示没有。

代码中的`solveNQueens`函数是主函数，它接受一个整数n作为参数，返回一个包含所有解法的二维数组。它首先初始化一个空的棋盘和三个辅助数组，然后调用辅函数`backtracking`来递归地在每一列放置皇后。

代码中的`backtracking`函数是辅函数，它接受六个参数：一个二维数组`ans`用来存储所有的解法；一个二维数组`board`用来表示当前的棋盘状态；三个一维数组`column`，`ldiag`和`rdiag`用来记录每一列和每一条对角线上是否有皇后；一个整数`row`用来表示当前要放置皇后的行号；一个整数n用来表示棋盘的大小。它首先判断是否所有的列都已经放置了皇后，如果是，就把当前的棋盘加入到解法数组中，并返回真。然后遍历当前列的每一行，检查是否可以放置皇后。如果可以，就修改当前节点的状态，即把棋盘上相应位置设为1，并更新三个辅助数组。然后递归地在下一列放置皇后。如果递归成功，就返回真。否则就回改当前节点的状态，即把棋盘上相应位置设为0，并恢复三个辅助数组。最后返回假。

代码中的`isSafe`函数是判断函数，它接受四个参数：一个二维数组`board`用来表示当前的棋盘状态；

```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量头文件

using namespace std; // 使用标准命名空间
void backtracking(vector<vector<string>> &ans, vector<string> &board, vector<
        bool> &column, vector<bool> &ldiag, vector<bool> &rdiag, int row, int n); // 声明回溯函数
// 主函数
vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> ans; // 定义一个二维向量ans，用来存储所有的解法
    if (n == 0) { // 如果n为0，直接返回空的ans
        return ans;}
    vector<string> board(n, string(n, '.')); // 定义一个二维字符串board，用来表示当前的棋盘状态，初始值为全'.'
    vector<bool> column(n, false), ldiag(2*n-1, false), rdiag(2*n-1, false); // 定义三个一维布尔向量column，ldiag和rdiag，用来记录每一列和每一条对角线上是否有皇后，初始值为全false
    backtracking(ans, board, column, ldiag, rdiag, 0, n); // 调用回溯函数，在每一列放置皇后，并把解法存入ans中
    return ans; // 返回ans
}
// 辅函数
void backtracking(vector<vector<string>> &ans, vector<string> &board, vector<
        bool> &column, vector<bool> &ldiag, vector<bool> &rdiag, int row, int n) {
    if (row == n) { // 如果当前行等于n，说明所有的列都已经放置了皇后，把当前的棋盘加入到ans中，并返回
        ans.push_back(board);
        return;
    }
    for (int i = 0; i < n; ++i) { // 遍历当前列的每一行
        if (column[i] || ldiag[n-row+i-1] || rdiag[row+i]) { // 如果当前位置和已经放置的皇后有冲突，就跳过这一行，继续下一行
            continue;
        }
// 修改当前节点状态
        board[row][i] = 'Q'; // 把当前位置设为'Q'，表示放置了一个皇后
        column[i] = ldiag[n-row+i-1] = rdiag[row+i] = true; // 更新三个辅助向量，表示当前列和对角线上有皇后
// 递归子节点
        backtracking(ans, board, column, ldiag, rdiag, row+1, n); // 递归地在下一列放置皇后，并把解法存入ans中
// 回改当前节点状态
        board[row][i] = '.'; // 把当前位置设为'.'，表示移除了一个皇后
        column[i] = ldiag[n-row+i-1] = rdiag[row+i] = false; // 恢复三个辅助向量，表示当前列和对角线上没有皇后
    }
}

int main() {
    vector<vector<string>> ans; // 定义一个二维向量ans，用来存储所有的解法
    ans = solveNQueens(4); // 调用主函数，传入参数4，表示求解4皇后问题，并把结果赋值给ans
    for (int i = 0; i < ans.size(); ++i) { // 遍历ans中的每一个解法
        for (int j = 0; j < ans[0].size(); ++j) { // 遍历每一个解法中的每一行
            cout << ans[i][j] << endl; // 输出当前行，并换行
        }
        cout << endl; // 输出一个空行，表示分隔两个解法
    }
    return 0; // 程序正常结束，返回0
}
```

执行用时：4 ms, 在所有 C++ 提交中击败了94.63%的用户

内存消耗：6.9 MB, 在所有 C++ 提交中击败了95.34%的用户

通过测试用例：9 / 9

```go
package main // 声明包名为main

func solveNQueens(n int) [][]string { // 定义一个函数solveNQueens，接受一个整数n作为参数，返回一个二维字符串数组
	col, dia1, dia2, row, res := make([]bool, n), make([]bool, 2*n-1), make([]bool, 2*n-1), []int{}, [][]string{} // 定义五个变量，分别是一个一维布尔数组col，用来记录每一列是否有皇后；两个一维布尔数组dia1和dia2，用来记录每一条对角线是否有皇后；一个一维整数数组row，用来记录每一行皇后的位置；一个二维字符串数组res，用来存储所有的解法
	putQueen(n, 0, &col, &dia1, &dia2, &row, &res) // 调用putQueen函数，在每一列放置皇后，并把解法存入res中
	return res // 返回res
}

// 尝试在一个n皇后问题中, 摆放第index行的皇后位置
func putQueen(n, index int, col, dia1, dia2 *[]bool, row *[]int, res *[][]string) { // 定义一个函数putQueen，接受七个参数，分别是棋盘的大小n，当前要放置皇后的行号index，以及之前定义的五个变量的指针
	if index == n { // 如果当前行等于n，说明所有的列都已经放置了皇后，把当前的棋盘加入到res中，并返回
		*res = append(*res, generateBoard(n, row)) // 调用generateBoard函数，根据row数组生成一个棋盘字符串数组，并追加到res中
		return // 返回
	}
	for i := 0; i < n; i++ { // 遍历当前列的每一行
		// 尝试将第index行的皇后摆放在第i列
		if !(*col)[i] && !(*dia1)[index+i] && !(*dia2)[index-i+n-1] { // 如果当前位置和已经放置的皇后没有冲突，就继续下面的操作；否则跳过这一行，继续下一行
			*row = append(*row, i) // 把当前位置的列号追加到row数组中
			(*col)[i] = true // 把当前位置所在的列设为true，表示有皇后
			(*dia1)[index+i] = true // 把当前位置所在的左对角线设为true，表示有皇后
			(*dia2)[index-i+n-1] = true // 把当前位置所在的右对角线设为true，表示有皇后
			putQueen(n, index+1, col, dia1, dia2, row, res) // 递归地在下一列放置皇后，并把解法存入res中
			(*col)[i] = false // 把当前位置所在的列设为false，表示没有皇后
			(*dia1)[index+i] = false // 把当前位置所在的左对角线设为false，表示没有皇后
			(*dia2)[index-i+n-1] = false // 把当前位置所在的右对角线设为false，表示没有皇后
			*row = (*row)[:len(*row)-1] // 把row数组的最后一个元素删除，表示移除了一个皇后
		}
	}
	return // 返回
}

func generateBoard(n int, row *[]int) []string { // 定义一个函数generateBoard，接受两个参数，分别是棋盘的大小n和row数组的指针，返回一个字符串数组表示棋盘
	board := []string{} // 定义一个字符串数组board，用来存储棋盘
	res := "" // 定义一个字符串res，用来表示一行
	for i := 0; i < n; i++ { // 遍历每一列
		res += "." // 在res后面追加一个'.'
	}
	for i := 0; i < n; i++ { // 遍历每一行
		board = append(board, res) // 把res追加到board中，表示一行全是'.'
	}
	for i := 0; i < n; i++ { // 遍历每一行
		tmp := []byte(board[i]) // 把board中的第i个字符串转换为字节数组tmp
		tmp[(*row)[i]] = 'Q' // 把tmp中的第row[i]个字节设为'Q'，表示放置了一个皇后
		board[i] = string(tmp) // 把tmp转换为字符串，赋值给board中的第i个字符串
	}
	return board // 返回board
}

func main() { // 定义主函数
	for _, v := range solveNQueens(4) { // 调用solveNQueens函数，传入参数4，表示求解4皇后问题，并遍历返回的结果中的每一个解法v
		for _, vv := range v { // 遍历每一个解法中的每一行vv
			println(vv) // 输出当前行，并换行
		}
		println() // 输出一个空行，表示分隔两个解法
	}
}
```


执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：3.2 MB, 在所有 Go 提交中击败了38.40%的用户

通过测试用例：9 / 9


