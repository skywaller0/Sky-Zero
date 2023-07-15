# [46. Permutations](https://leetcode.com/problems/permutations/)

用**回溯算法**来生成一个数组的所有排列的。回溯算法是一种递归的问题解决算法，它尝试逐步构建解决方案，同时剪枝掉那些不能满足约束条件的解决方案回溯算法常用于解决组合优化问题，如数独、八皇后、子集和等。

代码中的主函数`permute`接受一个数组`nums`作为参数，然后调用辅函数`backtracking`来生成所有可能的排列，并将它们存储在一个二维数组`ans`中，最后返回`ans`。辅函数`backtracking`接受三个参数：数组`nums`，当前的层级`level`，和二维数组`ans`。它的基本思路是：

- 如果当前层级等于数组长度减一，说明已经到达了最后一个元素，那么就将当前的数组加入到`ans`中，然后返回。
- 否则，从当前层级开始，遍历数组中的每个元素，将它与当前层级的元素交换（修改当前节点状态），然后递归地调用自己，将层级加一（递归子节点），最后再将元素交换回来（回改当前节点状态）。

这样，每次递归都会生成一个不同的排列，并且保证了每个排列中的元素都不重复。例如，如果输入的数组是[1, 2, 3]，那么输出的二维数组是：

[ [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1] 



```c++
#include <iostream> // 引入输入输出流头文件
#include <vector> // 引入向量容器头文件

using namespace std; // 使用标准命名空间
void backtracking(vector<int> &nums, int level, vector<vector<int>> &ans); // 声明辅函数
// 主函数
vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> ans; // 定义一个二维向量容器，用于存储结果
    backtracking(nums, 0, ans); // 调用辅函数，从第0层开始回溯
    return ans; // 返回结果
}
// 辅函数
void backtracking(vector<int> &nums, int level, vector<vector<int>> &ans) {
    if (level == nums.size() - 1) { // 如果当前层级等于数组长度减一，说明已经到达了最后一个元素
        ans.push_back(nums); // 将当前的数组加入到结果中
        return; // 返回
    }
    for (int i = level; i < nums.size(); i++) { // 否则，从当前层级开始，遍历数组中的每个元素
        swap(nums[i], nums[level]); // 将它与当前层级的元素交换（修改当前节点状态）
        backtracking(nums, level+1, ans); // 递归地调用自己，将层级加一（递归子节点）
        swap(nums[i], nums[level]); // 再将元素交换回来（回改当前节点状态）
    }
}

int main() {
    vector<int> nums = {1, 2, 3}; // 定义一个一维向量容器，用于存储输入的数组
    vector<vector<int>> ans = permute(nums); // 调用主函数，得到输出的二维向量容器
    for (auto &i : ans) { // 遍历二维向量容器中的每个一维向量容器
        for (auto &j : i) { // 遍历一维向量容器中的每个元素
            cout << j << " "; // 输出元素，并在后面加上一个空格
        }
        cout << endl; // 输出换行符
    }
    return 0; // 返回0，表示程序正常结束
}
```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：7.4 MB, 在所有 C++ 提交中击败了94.05%的用户

通过测试用例：26 / 26


```go
package main // 定义包名为main

func permute(nums []int) [][]int { // 定义一个函数permute，接受一个整型切片nums作为参数，返回一个二维整型切片
	if len(nums) == 0 { // 如果nums的长度为0，说明没有元素
		return [][]int{} // 返回一个空的二维切片
	}
	used, p, res := make([]bool, len(nums)), []int{}, [][]int{} // 定义三个变量：used是一个布尔型切片，用于记录nums中的元素是否被使用过；p是一个整型切片，用于存储当前的排列；res是一个二维整型切片，用于存储最终的结果
	generatePermutation(nums, 0, p, &res, &used) // 调用辅函数generatePermutation，从第0层开始生成排列，并将res和used的地址作为参数传递
	return res // 返回结果
}

func generatePermutation(nums []int, index int, p []int, res *[][]int, used *[]bool) { // 定义一个辅函数generatePermutation，接受五个参数：nums是输入的整型切片；index是当前的层级；p是当前的排列；res是最终的结果的地址；used是记录元素使用情况的地址
	if index == len(nums) { // 如果当前层级等于nums的长度，说明已经到达了最后一个元素
		temp := make([]int, len(p)) // 定义一个临时整型切片，用于复制p中的元素
		copy(temp, p) // 复制p中的元素到temp中
		*res = append(*res, temp) // 将temp加入到res所指向的二维切片中
		return // 返回
	}
	for i := 0; i < len(nums); i++ { // 否则，从0开始，遍历nums中的每个元素
		if !(*used)[i] { // 如果该元素没有被使用过
			(*used)[i] = true // 将used所指向的切片中对应位置设为true（表示已使用）
			p = append(p, nums[i]) // 将该元素加入到p中（修改当前节点状态）
			generatePermutation(nums, index+1, p, res, used) // 递归地调用自己，将层级加一（递归子节点）
			p = p[:len(p)-1] // 将p中最后一个元素去掉（回改当前节点状态）
			(*used)[i] = false // 将used所指向的切片中对应位置设为false（表示未使用）
		}
	}
	return // 返回
}

func main() { // 定义主函数
	nums := []int{1, 2, 3} // 定义一个整型切片nums，用于存储输入的数据
	for _, v := range permute(nums) { // 遍历permute函数返回的二维切片中的每个一维切片v
		for _, vv := range v { // 遍历v中的每个元素vv
			print(vv) // 输出vv，并在后面加上一个空格
		}
		println() // 输出换行符
	}
}
```


执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：2.5 MB, 在所有 Go 提交中击败了34.92%的用户

通过测试用例：26 / 26


回溯法相当于一直在尝试，如果发现不行，就回退到上一步，再尝试其他的可能性。这种方法的好处是不用考虑约束条件，因为每次都是尝试所有的可能性，而不是根据约束条件来决定下一步怎么走。