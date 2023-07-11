# [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

给你一个字符串 s 和一个整数 k ，请你找出 至多 包含 k 个 不同 字符的最长子串，并返回该子串的长度

双指针的方法来写，具体思路如下：

- 定义一个哈希表map，用来记录当前子串中每个字符出现的次数。
- 定义两个指针left和right，分别表示子串的左右边界，初始时都指向0。
- 定义一个变量ans，用来记录最长子串的长度，初始时为0。
- 从左到右遍历字符串s，每次将right向右移动一位，并将s[right]加入到map中。
- 如果map的大小超过了k，说明当前子串包含了多余的字符，就需要将left向右移动一位，并将s[left]从map中删除。如果map[s[left]]变为0，就说明s[left]不再出现在当前子串中，就可以从map中擦除它。这样，保证了left和right之间的子串最多包含k个不同的字符。
- 然后，更新最长子串的长度为max(ans,right-left+1)，即当前子串的长度和之前记录的最大长度中较大的那个。
- 最后，返回ans作为答案。

```c++
#include <unordered_map> // 引入哈希表头文件
using namespace std; // 使用标准命名空间

int lengthOfLongestSubstringKDistinct(string s, int k) {
    unordered_map<char,int> map; // 定义一个哈希表map
    if (k == 0) {return 0;} // 如果k为0，直接返回0
    int n = s.size(); // 获取字符串s的长度
    int ans = 0; // 初始化最长子串的长度为0
    for (int left = 0, right = 0; right < n; right++) { // 遍历字符串s，使用两个指针left和right表示子串的左右边界
        map[s[right]]++; // 将s[right]加入到map中
        while (map.size() > k) { // 如果map的大小超过了k
            map[s[left]]--; // 将s[left]从map中删除
            if (map[s[left]] == 0) {map.erase(s[left]);} // 如果map[s[left]]变为0，就从map中擦除它
            left++; // 将left向右移动一位
        }
        ans = max(ans, right - left + 1); // 更新最长子串的长度
    }
    return ans; // 返回答案
}
```



```go
package main

import "fmt"

// 定义一个函数，接受一个字符串s和一个整数k，返回s中最长的至多包含k个不同字符的子串的长度
func lengthOfLongestSubstringKDistinct(s string, k int) int {
	m := make(map[byte]int) // 定义一个哈希表map
	if k == 0 {
		return 0
	} // 如果k为0，直接返回0
	n := len(s)                                   // 获取字符串s的长度
	ans := 0                                      // 初始化最长子串的长度为0
	for left, right := 0, 0; right < n; right++ { // 遍历字符串s，使用两个指针left和right表示子串的左右边界
		m[s[right]]++    // 将s[right]加入到map中
		for len(m) > k { // 如果map的大小超过了k
			m[s[left]]-- // 将s[left]从map中删除
			if m[s[left]] == 0 {
				delete(m, s[left])
			} // 如果map[s[left]]变为0，就从map中擦除它
			left++ // 将left向右移动一位
		}
		ans = max(ans, right-left+1) // 更新最长子串的长度
	}
	return ans // 返回答案
}

// 定义一个辅助函数，返回两个整数中较大的那个
func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

// 主函数
func main() {
	s := "eceba"                                         // 定义一个字符串变量s，并赋值为"eceba"
	k := 2                                               // 定义一个整数变量k，并赋值为2
	fmt.Println(lengthOfLongestSubstringKDistinct(s, k)) // 调用lengthOfLongestSubstringKDistinct函数，并将结果输出到标准输出流，换行
}
```
这题就是不断匹配字符串到哈希表，哈希表的大小一旦超过k就得马上进行处理。这里的处理是将左指针向右移动一位，并将哈希表中对应的字符的值减一，如果减到0就从哈希表中擦除它。这样，保证了左右指针之间的子串最多包含k个不同的字符。然后，更新最长子串的长度为max(ans,right-left+1)，即当前子串的长度和之前记录的最大长度中较大的那个。最后，返回ans作为答案。




