# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

给定一个字符串 s，按照字符出现的频率从高到低排序，并返回一个新的字符串。例如，如果 s = “tree”，那么返回的字符串是 “eert” 或者 “eetr”。

这个代码使用了两个映射（map）来存储字符和出现次数的关系。第一个映射是一个哈希表（unordered_map），它可以快速地查找和更新每个字符的出现次数。第二个映射是一个有序映射（map），它可以按照键（即出现次数）从小到大排序，并存储每个出现次数对应的字符列表。这样，我们就可以从后往前遍历有序映射，按照出现次数从高到低取出字符，并根据出现次数重复添加到结果字符串中。最后，我们就得到了一个按照字符频率排序的新字符串。

```c++
#include <iostream>
#include <string>
#include <unordered_map>
#include <map>
#include <vector>
using namespace std;

string frequencySort(string s) {
    if (s == "") { // 如果字符串为空
        return ""; // 直接返回空字符串
    }
    unordered_map<char, int> sMap; // 定义一个哈希表，用于存放字符和出现次数的映射
    map<int, vector<char>> cMap; // 定义一个有序映射，用于存放出现次数和字符的映射，按照出现次数从小到大排序
    for (char c : s) { // 遍历字符串中的每个字符 c
        sMap[c]++; // 将 c 的出现次数加一
    }
    for (auto& p : sMap) { // 遍历哈希表中的每个键值对 p
        cMap[p.second].push_back(p.first); // 将 p 转换为出现次数和字符的键值对，并加入到有序映射中
    }

    string res; // 定义一个字符串，用于存放结果
    for (auto it = cMap.rbegin(); it != cMap.rend(); it++) { // 从有序映射的末尾开始遍历每个键值对，即按照出现次数从大到小遍历
        for (char c : it->second) { // 遍历每个出现次数相同的字符 c
            for (int i = 0; i < it->first; i++) { // 根据出现次数重复添加 c 到结果中
                res += c;
            }
        }
    }
    return res; // 返回结果
}

int main() {
    string s = "tree";
    string res = frequencySort(s);
    cout<<res<<endl;
    return 0;
}
```

执行用时：8 ms, 在所有 C++ 提交中击败了90.97%的用户

内存消耗：8.3 MB, 在所有 C++ 提交中击败了45.72%的用户

通过测试用例：33 / 33



```go
package main // 定义包名为 main

import (
	"sort" // 引入 sort 包，用于对切片进行排序
)

func frequencySort(s string) string { // 定义一个函数，用于按照字符出现的频率从高到低排序一个字符串，并返回一个新的字符串
	if s == "" { // 如果字符串为空
		return "" // 直接返回空字符串
	}
	sMap := map[byte]int{} // 定义一个映射，用于存放字符和出现次数的键值对
	cMap := map[int][]byte{} // 定义一个映射，用于存放出现次数和字符的键值对
	sb := []byte(s) // 将字符串转换为字节切片
	for _, b := range sb { // 遍历字节切片中的每个字节 b
		sMap[b]++ // 将 b 的出现次数加一
	}
	for key, value := range sMap { // 遍历字符和出现次数的映射中的每个键值对
		cMap[value] = append(cMap[value], key) // 将键值对转换为出现次数和字符的键值对，并加入到另一个映射中
	}

	var keys []int // 定义一个整数切片，用于存放所有的出现次数
	for k := range cMap { // 遍历出现次数和字符的映射中的每个键
		keys = append(keys, k) // 将键加入到整数切片中
	}
	sort.Sort(sort.Reverse(sort.IntSlice(keys))) // 对整数切片进行逆序排序，即按照出现次数从大到小排序
	res := make([]byte, 0) // 定义一个字节切片，用于存放结果
	for _, k := range keys { // 遍历排序后的整数切片中的每个元素 k，即每个出现次数
		for i := 0; i < len(cMap[k]); i++ { // 遍历每个出现次数对应的字符切片中的每个元素 i，即每个字符
			for j := 0; j < k; j++ { // 根据出现次数重复添加字符到结果中
				res = append(res, cMap[k][i])
			}
		}
	}
	return string(res) // 将结果转换为字符串并返回
}

func main() { // 定义主函数，程序入口点
	var s = "tree" // 定义一个字符串变量，存放要排序的字符串
	println(frequencySort(s)) // 输出 frequencySort 函数的返回值
}
```
执行用时：4 ms, 在所有 Go 提交中击败了92.55%的用户

内存消耗：5.5 MB, 在所有 Go 提交中击败了38.30%的用户

通过测试用例：33 / 33

用两个表，先对字符串进行排序，然后再按照字符出现的频率从高到低排序，最后再将字符串转换为字节切片并返回。

