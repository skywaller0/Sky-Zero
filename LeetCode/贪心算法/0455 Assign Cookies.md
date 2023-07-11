# [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

给你一个数组 children，表示每个孩子的胃口值，以及一个数组 cookies，表示每块饼干的大小。你的任务是尽可能地满足更多的孩子，但是每个孩子最多只能吃一块饼干，而且只有当饼干的大小大于等于孩子的胃口值时，孩子才能吃到饼干。返回你能满足的孩子的最大数量。

代码的思路是这样的：

- 首先对两个数组进行升序排序，这样可以保证从小到大地分配饼干。
- 然后用两个指针 child 和 cookie 分别指向 children 和 cookies 的起始位置。
- 接着进行一个循环，直到有一个指针超出了数组的范围为止。
- 在循环中，如果当前的饼干大小大于等于当前的孩子胃口值，说明可以满足这个孩子，那么就让 child 指针后移一位，表示已经分配了一块饼干给这个孩子。
- 不管是否满足了孩子，都要让 cookie 指针后移一位，表示已经考虑了这块饼干。
- 最后返回 child 指针的值，就是能满足的孩子的数量。

```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件

int findContentChildren(vector<int>& children, vector<int>& cookies) { // 定义一个函数，参数是两个整数向量的引用 children 和 cookies，返回值是一个整数
    sort(children.begin(), children.end()); // 对 children 向量进行升序排序
    sort(cookies.begin(), cookies.end()); // 对 cookies 向量进行升序排序
    int child = 0, cookie = 0; // 声明两个变量 child 和 cookie，分别表示当前分配的孩子和饼干的索引，初始都为 0
    while (child < children.size() && cookie < cookies.size()) { // 当 child 和 cookie 都没有超过向量的大小时，循环执行
       if (children[child] <= cookies[cookie]) ++child; // 如果当前孩子的胃口小于等于当前饼干的大小，说明可以满足这个孩子，将 child 加一
       ++cookie; // 不管是否满足孩子，都将 cookie 加一，表示分配下一个饼干
    }
    return child; // 返回 child，表示最多可以满足多少个孩子
}

int main() { // 定义主函数
	vector<int> children = {1,2,3}; // 定义一个向量 children，并初始化为 {1,2,3}
	vector<int> cookies = {1,1}; // 定义一个向量 cookies，并初始化为 {1,1}
	cout << findContentChildren(children, cookies) << endl; // 调用 findContentChildren 函数，并输出结果，换行
	return 0; // 返回 0，表示程序正常结束
}

```
执行用时：20 ms, 在所有 C++ 提交中击败了95.18%的用户

内存消耗：17 MB, 在所有 C++ 提交中击败了68.04%的用户

通过测试用例：21 / 21


```go
package main // 声明包名为 main

import (
	"fmt" // 导入 fmt 包，用于输出
	"sort" // 导入 sort 包，用于排序
)

func findContentChildren(children []int, cookies []int) int { // 定义一个函数，参数是两个整数切片 children 和 cookies，返回值是一个整数
	sort.Ints(children) // 对 children 切片进行升序排序
	sort.Ints(cookies) // 对 cookies 切片进行升序排序
	child, cookie := 0, 0 // 声明两个变量 child 和 cookie，分别表示当前分配的孩子和饼干的索引，初始都为 0
	for child < len(children) && cookie < len(cookies) { // 当 child 和 cookie 都没有超过切片的长度时，循环执行
		if cookies[cookie] >= children[child] { // 如果当前饼干的大小大于等于当前孩子的胃口，说明可以满足这个孩子
			cookie++ // 将 cookie 加一，表示分配下一个饼干
			child++ // 将 child 加一，表示满足下一个孩子
		} else { // 否则
			cookie++ // 将 cookie 加一，表示分配下一个饼干
		}
	}
	return child // 返回 child，表示最多可以满足多少个孩子
}

func main() { // 定义主函数
	fmt.Println(findContentChildren([]int{1, 2, 3}, []int{1, 1})) // 调用 findContentChildren 函数，并输出结果
}

```
执行用时：28 ms, 在所有 Go 提交中击败了72.31%的用户

内存消耗：6.1 MB, 在所有 Go 提交中击败了99.17%的用户

通过测试用例：21 / 21

这个题目大致的思路就是这样，但是这个题目还有一个进阶的问题，就是如何让分配的饼干尽可能地大，这样才能满足更多的孩子。这个问题可以用贪心算法来解决，具体的思路是这样的：

这个问题的一个可能的解法是：

- 首先对两个数组进行降序排序，这样可以保证从大到小地分配饼干。
- 然后用两个指针 child 和 cookie 分别指向 children 和 cookies 的起始位置。
- 接着进行一个循环，直到有一个指针超出了数组的范围为止。
- 在循环中，如果当前的饼干大小大于等于当前的孩子胃口值，说明可以满足这个孩子，那么就让 child 指针后移一位，表示已经分配了一块饼干给这个孩子。
- 不管是否满足了孩子，都要让 cookie 指针后移一位，表示已经考虑了这块饼干。
- 最后返回 child 指针的值，就是能满足的孩子的数量。

这样做的好处是，每次都优先分配最大的饼干给最贪心的孩子，这样可以减少浪费饼干的可能性，也可以提高满足孩子的概率。



