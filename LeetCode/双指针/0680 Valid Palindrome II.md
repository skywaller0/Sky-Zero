# [680. Valid Palindrome II](https://leetcode.cn/problems/valid-palindrome-ii/)
给定一个字符串s，判断它是否是一个有效的回文。回文是指一个字符串从左到右和从右到左读起来都一样，比如"racecar"或"madam"。有效的回文是指一个字符串可以通过删除最多一个字符来变成回文，比如"abca"可以删除第一个或最后一个"a"来变成回文。

代码的思路是，使用两个指针i和j，分别从字符串的两端向中间移动，比较两个指针所指的字符是否相等。如果相等，就继续移动指针，直到i大于等于j，说明字符串是回文。如果不相等，就尝试删除左边或右边的一个字符，然后判断剩下的子串是否是回文。如果是，就返回true，否则返回false。为了判断子串是否是回文，代码定义了一个辅助函数palindrome，它接受一个字符串s和两个下标i和j，然后用同样的方法遍历子串[i,j]，判断是否是回文。


```c++
#include <iostream> // 引入输入输出流头文件

using namespace std; // 使用标准命名空间

// 定义一个函数，判断字符串s的子串[i,j]是否是回文
bool palindrome(const string &s, int i, int j) {
    // 从两端向中间遍历，如果两个字符不相等，跳出循环
    for (; i < j && s[i] == s[j]; ++i, --j);
    // 如果i大于等于j，说明子串是回文，返回true，否则返回false
    return i >= j;
}

// 定义一个函数，判断字符串s是否是有效回文
bool validPalindrome(string s) {
    // 初始化两个指针，分别指向字符串的首尾
    int i = 0, j = s.size() - 1;
    // 当i小于j时，继续循环
    while (i < j) {
        // 如果两个字符不相等
        if (s[i] != s[j]) {
            // 判断去掉左边或右边一个字符后的子串是否是回文，如果是，返回true，否则返回false
            return palindrome(s, i, j - 1) || palindrome(s, i + 1, j);
        }
        // 移动左指针
        ++i;
        // 移动右指针
        --j;
    }
    // 如果循环结束，说明字符串是回文，返回true
    return true;
}

// 主函数
int main() {
    // 定义一个字符串变量s，并赋值为"abca"
    string s = "abca";
    // 调用validPalindrome函数，并输出结果到标准输出流，换行
    cout << validPalindrome(s) << endl;
    // 返回0，表示程序正常结束
    return 0;
}
```

执行用时：44 ms, 在所有 C++ 提交中击败了49.97%的用户

内存消耗：19.1 MB, 在所有 C++ 提交中击败了86.54%的用户

通过测试用例：469 / 469

```go
package main

import "fmt"

// 定义一个函数，判断字符串s的子串[i,j]是否是回文
func palindrome(s string, i int, j int) bool {
	// 从两端向中间遍历，如果两个字符不相等，跳出循环
	for i < j && s[i] == s[j] {
		i++
		j--
	}
	// 如果i大于等于j，说明子串是回文，返回true，否则返回false
	return i >= j
}

// 定义一个函数，判断字符串s是否是有效回文
func validPalindrome(s string) bool {
	// 初始化两个指针，分别指向字符串的首尾
	i := 0
	j := len(s) - 1
	// 当i小于j时，继续循环
	for i < j {
		// 如果两个字符不相等
		if s[i] != s[j] {
			// 判断去掉左边或右边一个字符后的子串是否是回文，如果是，返回true，否则返回false
			return palindrome(s, i, j-1) || palindrome(s, i+1, j)
		}
		// 移动左指针
		i++
		// 移动右指针
		j--
	}
	// 如果循环结束，说明字符串是回文，返回true
	return true
}

// 主函数
func main() {
	// 定义一个字符串变量s，并赋值为"abca"
	s := "abca"
	// 调用validPalindrome函数，并输出结果到标准输出流，换行
	fmt.Println(validPalindrome(s))
}
```
执行用时：8 ms, 在所有 Go 提交中击败了99.30%的用户

内存消耗：6.4 MB, 在所有 Go 提交中击败了67.13%的用户

通过测试用例：469 / 469

这个就是判断回文串的升级，先判断回文串，如果不是，再判断去掉左边或右边一个字符后的子串是否是回文串，如果是，返回true，否则返回false。




