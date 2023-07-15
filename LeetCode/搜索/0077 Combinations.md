# [77. Combinations](https://leetcode.com/problems/assign-cookies/)

代码中的主函数`combine`接受两个整数n和k作为参数，然后定义一个二维向量容器`ans`用于存储结果，一个一维向量容器`comb`用于存储当前的组合，一个整数变量`count`用于记录当前组合中的元素个数。然后调用辅函数`backtracking`来生成所有可能的组合，并将它们存储在`ans`中，最后返回`ans`。辅函数`backtracking`接受六个参数：二维向量容器`ans`的引用，一维向量容器`comb`的引用，整数变量`count`的引用，当前遍历的位置`pos`，整数n和k。它的基本思路是：

- 如果当前组合中的元素个数等于k，说明已经找到了一个有效的组合，那么就将当前的组合加入到`ans`中，然后返回。
- 否则，从当前位置开始，遍历从1到n的每个数，将它加入到当前组合中（修改当前节点状态），然后递归地调用自己，将位置加一（递归子节点），最后再将它从当前组合中移除（回改当前节点状态）。

这样，每次递归都会生成一个不同的组合，并且保证了每个组合中的元素都不重复且按照升序排列。例如，如果输入的n是4，k是2，那么输出的二维向量容器是：

[ [1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4] ]

```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量容器头文件

using namespace std; // 使用标准命名空间
void backtracking(vector<vector<int>>& ans, vector<int>& comb, int& count, int pos, int n, int k); // 声明辅函数
// 主函数
vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> ans; // 定义一个二维向量容器，用于存储结果
    vector<int> comb(k, 0); // 定义一个一维向量容器，用于存储当前的组合，初始值为0
    int count = 0; // 定义一个整数变量，用于记录当前组合中的元素个数，初始值为0
    backtracking(ans, comb, count, 1, n, k); // 调用辅函数，从第1个位置开始回溯
    return ans; // 返回结果
}
// 辅函数
void backtracking(vector<vector<int>>& ans, vector<int>& comb, int& count, int pos, int n, int k) {
    if (count == k) { // 如果当前组合中的元素个数等于k，说明已经找到了一个有效的组合
        ans.push_back(comb); // 将当前的组合加入到结果中
        return; // 返回
    }
    for (int i = pos; i <= n; ++i) { // 否则，从当前位置开始，遍历从1到n的每个数
        comb[count++] = i; // 将该数加入到当前组合中（修改当前节点状态），并将元素个数加一
        backtracking(ans, comb, count, i + 1, n, k); // 递归地调用自己，将位置加一（递归子节点）
        --count; // 将元素个数减一（回改当前节点状态）
    }
}

int main() {
    vector<vector<int>> ans = combine(4, 2); // 调用主函数，得到输出的二维向量容器，n为4，k为2
    for (int i = 0; i < ans.size(); ++i) { // 遍历二维向量容器中的每个一维向量容器
        cout << "["; // 输出左方括号
        for (int j = 0; j < ans[i].size(); ++j) { // 遍历一维向量容器中的每个元素
            cout << ans[i][j] << ","; // 输出元素，并在后面加上一个逗号
        }
        cout << "]" << endl; // 输出右方括号和换行符
    }
    return 0; // 返回0，表示程序正常结束
}
```
执行用时：12 ms, 在所有 C++ 提交中击败了75.59%的用户

内存消耗：8.9 MB, 在所有 C++ 提交中击败了89.30%的用户

通过测试用例：27 / 27


```go
package main // 定义包名为main

func combine(n int, k int) [][]int { // 定义一个函数combine，接受两个整数n和k作为参数，返回一个二维整型切片
	if n <= 0 || k <= 0 || k > n { // 如果n或k是非正数，或者k大于n，说明没有有效的组合
		return [][]int{} // 返回一个空的二维切片
	}
	c, res := []int{}, [][]int{} // 定义两个变量：c是一个整型切片，用于存储当前的组合；res是一个二维整型切片，用于存储最终的结果
	generateCombinations(n, k, 1, c, &res) // 调用辅函数generateCombinations，从第1个位置开始生成组合，并将res的地址作为参数传递
	return res // 返回结果
}

func generateCombinations(n, k, start int, c []int, res *[][]int) { // 定义一个辅函数generateCombinations，接受五个参数：n和k是输入的整数；start是当前遍历的位置；c是当前的组合；res是最终的结果的地址
	if len(c) == k { // 如果当前组合中的元素个数等于k，说明已经找到了一个有效的组合
		b := make([]int, len(c)) // 定义一个临时整型切片，用于复制c中的元素
		copy(b, c) // 复制c中的元素到b中
		*res = append(*res, b) // 将b加入到res所指向的二维切片中
		return // 返回
	}
	// i最多为n - (k - len(c)) + 1，这样可以避免重复和无效的组合
	for i := start; i <= n-(k-len(c))+1; i++ { // 从当前位置开始，遍历从1到n的每个数
		c = append(c, i) // 将该数加入到当前组合中（修改当前节点状态）
		generateCombinations(n, k, i+1, c, res) // 递归地调用自己，将位置加一（递归子节点）
		c = c[:len(c)-1] // 将当前组合中最后一个元素去掉（回改当前节点状态）
	}
	return // 返回
}

func main() { // 定义主函数
	for _, v := range combine(4, 2) { // 遍历combine函数返回的二维切片中的每个一维切片v，n为4，k为2
		for _, v2 := range v { // 遍历v中的每个元素v2
			print(v2, " ") // 输出v2，并在后面加上一个空格
		}
		println() // 输出换行符
	}
}
```
执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：5.9 MB, 在所有 Go 提交中击败了92.97%的用户

通过测试用例：27 / 27

跟46题差不多
类似于排列问题，我们也可以进行回溯。排列回溯的是交换的位置，而组合回溯的是是否把
当前的数字加入结果中。




