# [76. Minimum Window Substring](https://leetcode.cn/problems/minimum-window-substring/)

给定两个字符串S和T，找出S中包含T中所有字符的最短子字符串，如果不存在，就返回空字符串。代码的逻辑是，先用一个数组chars来统计T中每个字符出现的次数，用另一个数组flag来标记T中出现过的字符。然后用一个滑动窗口[l, r]来遍历S，不断更新chars和一个计数变量cnt，表示当前窗口中已经匹配了多少个T中的字符。如果cnt等于T的长度，说明当前窗口已经包含了T中所有字符，那么就尝试将左边界l右移，缩小窗口的大小，同时更新最小长度min_size和最小左边界min_l。最后返回S的子字符串[min_l, min_l + min_size - 1]，或者空字符串。

```c++
#include <iostream> // 引入输入输出流的头文件
#include <vector> // 引入向量的头文件
#include <tuple> // 引入元组的头文件

using namespace std; // 使用标准命名空间

string minWindow(string S, string T) { // 定义一个函数，接受两个字符串S和T作为参数，返回一个字符串作为结果
    vector<int> chars(128, 0); // 定义一个大小为128，初始值为0的向量chars，用来存储T中每个字符出现的次数
    vector<bool> flag(128, false); // 定义一个大小为128，初始值为false的向量flag，用来标记T中出现过的字符
    // 先统计T中的字符情况
    for (int i = 0; i < T.size(); ++i) { // 用一个循环遍历T中的每个字符
        flag[T[i]] = true; // 把对应的flag位置为true
        ++chars[T[i]]; // 把对应的chars加一
    }
    // 移动滑动窗口，不断更改统计数据
    int cnt = 0, l = 0, min_l = 0, min_size = S.size() + 1; // 定义四个整数变量，分别表示当前窗口匹配了多少个T中的字符，窗口的左边界，最小左边界和最小	长度
    for (int r = 0; r < S.size(); ++r) { // 用一个循环遍历S中的每个字符，把r作为窗口的右边界
        if (flag[S[r]]) { // 如果当前字符在T中出现过
            if (--chars[S[r]] >= 0) { // 就把对应的chars减一，如果减一后仍然大于等于0，说明这个字符是有效匹配的
                ++cnt; // 就把cnt加一
            }
            // 若目前滑动窗口已包含T中全部字符，
            // 则尝试将l右移，在不影响结果的情况下获得最短子字符串
            while (cnt == T.size()) { // 当cnt等于T的长度时，说明当前窗口已经包含了T中所有字符，那么就尝试缩小窗口的大小
                if (r - l + 1 < min_size) { // 如果当前窗口的长度小于之前记录的最小长度
                    min_l = l; // 就更新最小左边界为l
                    min_size = r - l + 1; // 更新最小长度为r - l + 1
                }
                if (flag[S[l]] && ++chars[S[l]] > 0) { // 如果窗口的左边界对应的字符在T中出现过，并且把对应的chars加一后大于0，说明这个字符是有效匹配的
                    --cnt; // 就把cnt减一，表示失去了一个匹配
                }
                ++l; // 把l加一，右移窗口的左边界
            }
        }
    }
    return min_size > S.size() ? "" : S.substr(min_l, min_size); // 如果最小长度大于S的长度，说明没有找到合适的子字符串，就返回空字符串；否则返回S的子字符串[min_l, min_l + min_size - 1]
}

int main() { // 定义主函数
    string s = "ADOBECODEBANC"; // 定义一个字符串s，并初始化为"ADOBECODEBANC"
    string t = "ABC"; // 定义一个字符串t，并初始化为"ABC"
    string result = minWindow(s, t); // 调用minWindow函数，传入s和t作为参数，并得到结果赋值给result
    cout << result << endl; // 输出result，即最短子字符串"BANC"
    return 0; // 主函数返回0，表示程序正常结束
}
```



```go
package main // 定义包名为main

import "fmt" // 引入格式化输入输出的包

func minWindow(s string, t string) string { // 定义一个函数，接受两个字符串s和t作为参数，返回一个字符串作为结果
	if s == "" || t == "" { // 如果s或t为空字符串
		return "" // 就返回空字符串
	}
	var tFreq, sFreq [256]int // 定义两个大小为256的整数数组，分别用来存储t和s中每个字符出现的次数
	result, left, right, finalLeft, finalRight, minW, count := "", 0, -1, -1, -1, len(s)+1, 0 // 定义七个变量，分别表示最终结果，滑动窗口的左右边界，最小长度和最小左边界，当前窗口匹配了多少个t中的字符

	for i := 0; i < len(t); i++ { // 用一个循环遍历t中的每个字符
		tFreq[t[i]-'a']++ // 把对应的tFreq加一
	}

	for left < len(s) { // 当左边界小于s的长度时，循环执行以下代码
		if right+1 < len(s) && count < len(t) { // 如果右边界加一小于s的长度，并且匹配的字符数小于t的长度
			sFreq[s[right+1]-'a']++ // 就把对应的sFreq加一
			if sFreq[s[right+1]-'a'] <= tFreq[s[right+1]-'a'] { // 如果sFreq不大于tFreq，说明这个字符是有效匹配的
				count++ // 就把count加一
			}
			right++ // 把右边界加一，扩大窗口的大小
		} else { // 否则，说明当前窗口已经包含了t中所有字符，或者右边界已经到达s的末尾
			if right-left+1 < minW && count == len(t) { // 如果当前窗口的长度小于之前记录的最小长度，并且匹配的字符数等于t的长度
				minW = right - left + 1 // 就更新最小长度为当前窗口的长度
				finalLeft = left // 更新最小左边界为当前左边界
				finalRight = right // 更新最小右边界为当前右边界
			}
			if sFreq[s[left]-'a'] == tFreq[s[left]-'a'] { // 如果左边界对应的字符在t中出现过，并且sFreq等于tFreq，说明这个字符是有效匹配的
				count-- // 就把count减一，表示失去了一个匹配
			}
			sFreq[s[left]-'a']-- // 把对应的sFreq减一
			left++ // 把左边界加一，缩小窗口的大小
		}
	}
	if finalLeft != -1 { // 如果最小左边界不等于-1，说明找到了合适的子字符串
		result = string(s[finalLeft : finalRight+1]) // 就把s的子字符串赋值给result
	}
	return result // 返回result作为函数的结果
}

func main() { // 定义主函数
	s := "ADOBECODEBANC" // 定义一个字符串s，并初始化为"ADOBECODEBANC"
	t := "ABC"           // 定义一个字符串t，并初始化为"ABC"
	fmt.Println(minWindow(s, t)) // 调用minWindow函数，传入s和t作为参数，并打印返回值，即最短子字符串"BANC"
}
```

本题使用滑动窗口求解，即两个指针l 和r 都是从最左端向最右端移动，且l 的位置一定在r 的左边或重合。注意本题虽然在for 循环里出现了一个while 循环，但是因为while 循环负责移动l 指针，且l 只会从左到右移动一次，因此总时间复杂度仍然是O¹nº。本题使用了长度为128的数组来映射字符，也可以用哈希表替代；其中chars 表示目前每个字符缺少的数量，flag 表示每个字符是否在T 中存在。



