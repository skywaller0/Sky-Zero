# [455. Candy](https://leetcode.com/problems/candy/)

这段代码是用来解决一个贪心算法的问题，题目是这样的：

有一群孩子站成一排，每个孩子有一个评分。你要给这些孩子分配糖果，要求如下：

- 每个孩子至少要有一颗糖果。
- 评分高的孩子要比他相邻的评分低的孩子得到更多的糖果。
- 返回你需要的最少糖果数量。

代码的思路是这样的：

- 首先获取评分数组的大小，如果小于 2，直接返回数组大小，因为每个孩子至少有一颗糖果。
- 然后创建一个和评分数组大小相同的糖果数组，初始值都为 1，表示每个孩子至少有一颗糖果。
- 接着从左到右遍历评分数组，如果当前孩子的评分比前一个孩子高，那么就给当前孩子多一颗糖果，保证满足条件。
- 再从右到左遍历评分数组，如果当前孩子的评分比后一个孩子高，并且当前孩子的糖果数不大于后一个孩子，那么就给当前孩子多一颗糖果，保证满足条件。
- 最后用一个内置的函数 accumulate 来求和糖果数组，得到最少需要的糖果数量。

```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件
#include <numeric> // 导入数值算法头文件

using namespace std; // 使用标准命名空间

int candy(vector<int> &ratings) { // 定义一个函数，参数是一个整数向量的引用 ratings，返回值是一个整数
    int size = ratings.size(); // 声明一个变量 size，赋值为 ratings 的大小，表示孩子的个数
    if (size < 2) { // 如果 size 小于 2，表示没有孩子或者只有一个孩子
        return size; // 直接返回 size，表示需要的糖果数
    }
    vector<int> candies(size, 1); // 创建一个和 ratings 等长的整数向量 candies，并初始化每个元素为 1，表示每个孩子分配的糖果数
    for (int i = 1; i < size; ++i) { // 从左到右遍历 ratings 中的每个元素，从第二个开始，用变量 i 表示索引
        if (ratings[i] > ratings[i - 1]) { // 如果当前孩子的评分高于前一个孩子的评分
            candies[i] = candies[i - 1] + 1; // 将 candies[i] 更新为 candies[i-1] 加一，表示比前一个孩子多分配一个糖果
        }
    }
    for (int i = size - 2; i >= 0; --i) { // 从右到左遍历 ratings 中的每个元素，从倒数第二个开始，用变量 i 表示索引
        if (ratings[i] > ratings[i + 1] && candies[i] <= candies[i+1] ) { // 如果当前孩子的评分高于后一个孩子的评分，并且当前孩子分配的糖果数不多于后一个孩子的糖果数
            candies[i] = candies[i+1] + 1; // 将 candies[i] 更新为 candies[i+1] 加一，表示比后一个孩子多分配一个糖果
        }
    }
    return accumulate(candies.begin(), candies.end(), 0); // 返回 candies 中所有元素的和，表示总共需要的糖果数
}

int main() { // 定义主函数
    vector<int> ratings = {1,0,2}; // 定义一个向量 ratings，并初始化为 {1,0,2}
    cout << candy(ratings) << endl; // 调用 candy 函数，并输出结果，换行
    return 0; // 返回 0，表示程序正常结束
}
```
执行用时：16 ms, 在所有 C++ 提交中击败了85.12%的用户

内存消耗：17.3 MB, 在所有 C++ 提交中击败了54.45%的用户

通过测试用例：48 / 48


```go
package main // 声明包名为 main

import "fmt" // 导入 fmt 包，用于输出

func candy(ratings []int) int { // 定义一个函数，参数是一个整数切片 ratings，返回值是一个整数
	candies := make([]int, len(ratings)) // 创建一个和 ratings 等长的整数切片 candies，用于存储每个孩子分配的糖果数
	for i := 1; i < len(ratings); i++ { // 从左到右遍历 ratings 中的每个元素，从第二个开始，用变量 i 表示索引
		if ratings[i] > ratings[i-1] { // 如果当前孩子的评分高于前一个孩子的评分
			candies[i] += candies[i-1] + 1 // 将 candies[i] 更新为 candies[i-1] 加一，表示比前一个孩子多分配一个糖果
		}
	}
	for i := len(ratings) - 2; i >= 0; i-- { // 从右到左遍历 ratings 中的每个元素，从倒数第二个开始，用变量 i 表示索引
		if ratings[i] > ratings[i+1] && candies[i] <= candies[i+1] { // 如果当前孩子的评分高于后一个孩子的评分，并且当前孩子分配的糖果数不多于后一个孩子的糖果数
			candies[i] = candies[i+1] + 1 // 将 candies[i] 更新为 candies[i+1] 加一，表示比后一个孩子多分配一个糖果
		}
	}
	total := 0 // 声明一个变量 total，表示总共需要的糖果数，初始为 0
	for _, candy := range candies { // 遍历 candies 中的每个元素，用变量 candy 表示值
		total += candy + 1 // 将 total 加上 candy 加一，表示每个孩子至少需要一个糖果
	}
	return total // 返回 total
}

func main() { // 定义主函数
	fmt.Println(candy([]int{1, 0, 2})) // 调用 candy 函数，并输出结果
}
```

执行用时：12 ms, 在所有 Go 提交中击败了95.45%的用户

内存消耗：6.2 MB, 在所有 Go 提交中击败了52.73%的用户

通过测试用例：48 / 48

把所有孩子的糖果数初始化为1；
先从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的
糖果数加1；再从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数
不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加1。通过这两次遍历，
分配的糖果就可以满足题目要求了。这里的贪心策略即为，在每次遍历中，只考虑并更新相邻一
侧的大小关系。


