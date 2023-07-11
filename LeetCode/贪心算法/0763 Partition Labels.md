# [763. Partition Lables](https://leetcode.com/problems/partition-labels/)

求解一个问题：给定一个字符串 S，将 S 划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个片段长度的列表。函数的参数是一个字符串 S，函数的返回值是一个整数切片。

函数的思路是先遍历一遍字符串，记录每个字母最后出现的索引，用一个数组 lastIndexOf 存储。然后再遍历一遍字符串，用两个变量 start 和 end 表示当前片段的起始和结束位置，初始都为 0。对于每个位置 i，找到当前字母最后出现的索引 lastIndexOf[S[i]-‘a’]，如果这个索引大于 end，说明当前字母不能放在当前片段，需要更新 end 为这个索引。当 i 等于 end 时，说明找到了一个完整的片段，将其长度加入到结果数组 arr 中，并将 start 更新为 end + 1，继续寻找下一个片段。最后返回 arr 即可。
```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件

using namespace std; // 使用标准命名空间
vector<int> partitionLabels(string S) { // 定义一个函数，参数是一个字符串 S，返回值是一个整数向量
    // 创建一个数组，存储每个字母最后出现的索引
    int lastIndexOf[26];
    for (int i = 0; i < S.size(); i++) {
        lastIndexOf[S[i] - 'a'] = i;
    }

    // 创建一个向量，存储每个片段的长度
    vector<int> arr;
    for (int start = 0, end = 0; start < S.size(); start = end + 1) {
        // 找到当前片段的最右边界
        end = lastIndexOf[S[start] - 'a'];
        for (int i = start; i < end; i++) {
            // 如果有字母超过了当前边界，更新边界
            if (end < lastIndexOf[S[i] - 'a']) {
                end = lastIndexOf[S[i] - 'a'];
            }
        }
        // 将当前片段的长度加入向量
        arr.push_back(end - start + 1);
    }
    return arr;
}


int main() { // 定义主函数
    string S = "ababcbacadefegdehijhklij"; // 定义一个字符串 S
    vector<int> arr = partitionLabels(S); // 调用 partitionLabels 函数，并将结果赋值给向量 arr
    for (int i = 0; i < arr.size(); i++) { // 遍历向量 arr 中的每个元素
        cout << arr[i] << " "; // 输出元素和空格
    }
    return 0; // 返回 0，表示程序正常结束
}

```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.4 MB, 在所有 C++ 提交中击败了81.08%的用户

通过测试用例：118 / 118


```go
package main // 声明包名为 main

import "fmt" // 导入 fmt 包，用于输出

func partitionLabels(S string) []int { // 定义一个函数，参数是一个字符串 S，返回值是一个整数切片
	var lastIndexOf [26]int // 声明一个长度为 26 的整数数组，用于存储每个字母最后出现的索引
	for i, v := range S {   // 遍历字符串 S，i 是索引，v 是字符
		lastIndexOf[v-'a'] = i // 将字符 v 对应的数组元素赋值为 i
	}

	var arr []int                                             // 声明一个整数切片，用于存储每个片段的长度
	for start, end := 0, 0; start < len(S); start = end + 1 { // 用两个变量 start 和 end 表示当前片段的起始和结束位置，初始都为 0，每次循环后将 start 更新为 end + 1，直到 start 等于字符串长度
		end = lastIndexOf[S[start]-'a'] // 将 end 更新为当前字母最后出现的索引
		for i := start; i < end; i++ {  // 遍历当前片段中的每个位置 i
			if end < lastIndexOf[S[i]-'a'] { // 如果当前位置的字母最后出现的索引大于 end
				end = lastIndexOf[S[i]-'a'] // 将 end 更新为这个索引
			}
		}
		arr = append(arr, end-start+1) // 将当前片段的长度加入到切片 arr 中
	}
	return arr // 返回切片 arr
}
func main() { // 定义主函数
	fmt.Println(partitionLabels("ababcbacadefegdehijhklij")) // 调用 partitionLabels 函数，并输出结果
}


```
执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：2 MB, 在所有 Go 提交中击败了50.66%的用户

通过测试用例：118 / 118

这个就是一直在索引，找到完整片段所在的索引
