# [633. Sum of Square Numbers](https://leetcode.cn/problems/sum-of-square-numbers/)

接受一个整数c作为参数，返回c是否能够表示为两个平方数的和，比如5 = 11 + 22，如果能则返回true，否则返回false。函数使用两个变量，low和high，来表示两个平方数的平方根的可能值。它从low = 0和high = sqrt©开始，这是其中一个平方根的最大可能值。然后它用一个while循环来检查lowlow + highhigh是否等于c，如果等于则返回true。如果lowlow + highhigh小于c，说明low太小了，所以把low加1。如果lowlow + highhigh大于c，说明high太大了，所以把high减1。循环结束时，low大于high，说明没有两个平方数能够加起来等于c，函数返回false。

```c++
#include <iostream> // 引入输入输出流库
#include <cmath> // 引入数学库

using namespace std; // 使用标准命名空间

bool judgeSquareSum(int c) { // 定义一个函数，接受一个整数c作为参数，返回c是否能够表示为两个平方数的和
    long long low = 0, high = sqrt(c); // 定义两个长整型变量，low和high，分别表示两个平方数的平方根的可能值，初始化为0和sqrt(c)
    while (low <= high) { // 当low小于等于high时，循环执行以下语句
        if (low*low + high*high < c) { // 如果low的平方加上high的平方小于c
            low++; // 把low加1
        } else if (low*low + high*high > c) { // 否则，如果low的平方加上high的平方大于c
            high--; // 把high减1
        } else { // 否则，说明low的平方加上high的平方等于c
            return true; // 返回true
        }
    }
    return false; // 循环结束后，说明没有两个平方数能够加起来等于c，返回false
}

int main() { // 定义主函数
    int c; // 定义一个整型变量c
    cin >> c; // 从标准输入读取c的值
    cout << judgeSquareSum(c) << endl; // 调用judgeSquareSum函数，并把结果输出到标准输出，换行
    return 0; // 返回0，表示程序正常结束
}

```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.8 MB, 在所有 C++ 提交中击败了58.68%的用户

通过测试用例：127 / 127


```go
package main // 声明包名为main

import ( // 引入两个包
	"fmt" // 标准输入输出包
	"math" // 数学包
)

func judgeSquareSum(c int) bool { // 定义一个函数，接受一个整数c作为参数，返回c是否能够表示为两个平方数的和
	low, high := 0, int(math.Sqrt(float64(c))) // 定义两个整型变量，low和high，分别表示两个平方数的平方根的可能值，初始化为0和sqrt(c)，注意类型转换
	for low <= high { // 当low小于等于high时，循环执行以下语句
		if low*low+high*high < c { // 如果low的平方加上high的平方小于c
			low++ // 把low加1
		} else if low*low+high*high > c { // 否则，如果low的平方加上high的平方大于c
			high-- // 把high减1
		} else { // 否则，说明low的平方加上high的平方等于c
			return true // 返回true
		}
	}
	return false // 循环结束后，说明没有两个平方数能够加起来等于c，返回false
}

func main() { // 定义主函数
	var c int // 声明一个整型变量c
	fmt.Scanf("%d", &c) // 从标准输入读取c的值，注意使用指针
	fmt.Println(judgeSquareSum(c)) // 调用judgeSquareSum函数，并把结果输出到标准输出，换行
}
```
执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

内存消耗：1.8 MB, 在所有 Go 提交中击败了68.67%的用户

通过测试用例：127 / 127


用二分搜索来解答这道题。判断题意，依次计算 low * low + high * high 和 c 是否相等。从 [0, sqrt(n)] 区间内进行二分，若能找到则返回 true，找不到就返回 false 。

