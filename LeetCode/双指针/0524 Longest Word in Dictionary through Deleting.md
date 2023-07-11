# [524. Longest Word in Dictionary through Deleting](https://leetcode.cn/problems/longest-word-in-dictionary-through-deleting/)

给定一个字符串s和一个字符串数组d，从d中找出一个最长的字符串，使得它是s的子序列。子序列是指一个字符串可以通过删除一些字符（不改变顺序）来得到的另一个字符串，比如"abc"是"abracadabra"的子序列。如果有多个最长的字符串，就返回字典序最小的那个，比如"apple"和"apply"都是"abpcplea"的子序列，但是返回"apple"。

代码的思路是，遍历d中的每个字符串，用两个指针pointS和pointD分别指向s和d[i]的当前位置，然后同时向右移动指针，如果两个指针所指的字符相等，就说明找到了一个匹配的字符，pointD向右移动一位，否则只移动pointS。如果pointD移动到了d[i]的末尾，就说明d[i]是s的子序列。然后比较d[i]和当前的结果res，如果d[i]更长或者长度相同但字典序更小，就更新res为d[i]。最后返回res。

```c++
#include <iostream> // 引入输入输出流头文件
#include <string> // 引入字符串头文件
#include <vector> // 引入向量头文件

using namespace std; // 使用标准命名空间

// 定义一个函数，接受一个字符串s和一个字符串向量d，返回d中最长的s的子序列
string findLongestWord(string s, vector<string> d) {
    string res = ""; // 初始化结果为空字符串
    for (int i = 0; i < d.size(); i++) { // 遍历d中的每个字符串
        int pointS = 0; // 初始化s的指针为0
        int pointD = 0; // 初始化d[i]的指针为0
        while (pointS < s.size() && pointD < d[i].size()) { // 当两个指针都没有越界时，继续循环
            if (s[pointS] == d[i][pointD]) { // 如果两个指针所指的字符相等，说明找到了一个匹配的字符
                pointD++; // 移动d[i]的指针向右一位
            }
            pointS++; // 移动s的指针向右一位
        }
        if (pointD == d[i].size() && (res.size() < d[i].size() || (res.size() == d[i].size() && res > d[i]))) { // 如果d[i]的指针移动到了末尾，说明d[i]是s的子序列，并且如果d[i]比当前的结果更长或者长度相同但字典序更小，就更新结果为d[i]
            res = d[i]; // 更新结果为d[i]
        }
    }
    return res; // 返回结果
}

// 主函数
int main() {
    string s = "abpcplea"; // 定义一个字符串变量s，并赋值为"abpcplea"
    vector<string> d = {"ale", "apple", "monkey", "plea"}; // 定义一个字符串向量变量d，并赋值为{"ale", "apple", "monkey", "plea"}
    cout << findLongestWord(s, d) << endl; // 调用findLongestWord函数，并将结果输出到标准输出流，换行
    return 0; // 返回0，表示程序正常结束
}

```
执行用时：80 ms, 在所有 C++ 提交中击败了12.91%的用户

内存消耗：17.4 MB, 在所有 C++ 提交中击败了29.95%的用户

通过测试用例：31 / 31


```go
package main // 声明包名为main

import "fmt" // 引入fmt包，用于格式化输入输出

// 定义一个函数，接受一个字符串s和一个字符串数组d，返回d中最长的s的子序列
func findLongestWord(s string, d []string) string {
	res := "" // 初始化结果为空字符串
	for i := 0; i < len(d); i++ { // 遍历d中的每个字符串
		pointS := 0 // 初始化s的指针为0
		pointD := 0 // 初始化d[i]的指针为0
		for pointS < len(s) && pointD < len(d[i]) { // 当两个指针都没有越界时，继续循环
			if s[pointS] == d[i][pointD] { // 如果两个指针所指的字符相等，说明找到了一个匹配的字符
				pointD++ // 移动d[i]的指针向右一位
			}
			pointS++ // 移动s的指针向右一位
		}
		if pointD == len(d[i]) && (len(res) < len(d[i]) || (len(res) == len(d[i]) && res > d[i])) { // 如果d[i]的指针移动到了末尾，说明d[i]是s的子序列，并且如果d[i]比当前的结果更长或者长度相同但字典序更小，就更新结果为d[i]
			res = d[i] // 更新结果为d[i]
		}
	}
	return res // 返回结果
}

// 主函数
func main() {
	s := "abpcplea" // 定义一个字符串变量s，并赋值为"abpcplea"
	d := []string{"ale", "apple", "monkey", "plea"} // 定义一个字符串数组变量d，并赋值为{"ale", "apple", "monkey", "plea"}
	fmt.Println(findLongestWord(s, d)) // 调用findLongestWord函数，并将结果输出到标准输出流，换行
}
```
执行用时：16 ms, 在所有 Go 提交中击败了75.51%的用户

内存消耗：7.7 MB, 在所有 Go 提交中击败了83.67%的用户

通过测试用例：31 / 31




这题就是双指针遍历，一个遍历字符串，另一个遍历字典，当跑完字典时，判断是不是子序列，是的话就就更新结果。也不算很难