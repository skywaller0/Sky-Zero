# [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
找出在给定的股票价格向量中，可以获得的最大利润。它假设可以在任何一天买入或卖出股票，并且可以进行多次交易。它使用了一种贪心算法，即只要有利可图，就进行交易
- 定义一个函数，名为maxProfit，接受一个整数向量作为参数，返回一个整数作为最大利润。
- 在函数内部，定义一个变量profit，初始值为0，表示利润。
- 使用一个for循环，从0开始，到向量的大小减一结束，每次增加1，遍历向量中的每个元素，除了最后一个。
- 在循环内部，使用一个if语句，判断下一个元素是否大于当前元素。
- 如果是，则将两者的差值累加到profit中，表示买入当前元素，卖出下一个元素，获得利润。
- 如果不是，则跳过这一步，继续循环。
- 循环结束后，返回profit作为最大利润。


```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量容器库
#include <algorithm> // 引入算法库

using namespace std; // 使用标准命名空间

int maxProfit(vector<int> &prices) { // 定义一个函数，接受一个整数向量作为参数，返回一个整数作为最大利润
    int profit = 0; // 初始化利润为0
    for (int i = 0; i < prices.size() - 1; i++) { // 遍历向量中的每个元素，除了最后一个
        if (prices[i + 1] > prices[i]) { // 如果下一个元素大于当前元素
            profit += prices[i + 1] - prices[i]; // 则将差值累加到利润中
        }
    }
    return profit; // 返回利润
}

int main() { // 定义主函数
    vector<int> prices = {7, 1, 5, 3, 6, 4}; // 定义一个向量，存储股票价格
    int profit = maxProfit(prices); // 调用函数，计算最大利润
    cout << profit << endl; // 输出最大利润
    return 0; // 返回0，表示程序正常结束
}
```



```go
package main // 定义包名为main

import "fmt" // 引入格式化输入输出库

func maxProfit(prices []int) int { // 定义一个函数，接受一个整数切片作为参数，返回一个整数作为最大利润
	profit := 0 // 初始化利润为0
	for i := 0; i < len(prices)-1; i++ { // 遍历切片中的每个元素，除了最后一个
		if prices[i+1] > prices[i] { // 如果下一个元素大于当前元素
			profit += prices[i+1] - prices[i] // 则将差值累加到利润中
		}
	}
	return profit // 返回利润
}
func main() { // 定义主函数
	prices := []int{7, 1, 5, 3, 6, 4} // 定义一个切片，存储股票价格
	fmt.Println(maxProfit(prices)) // 调用函数，计算最大利润，并打印到标准输出
}
```

最大收益来源，必然是每次跌了就买入，涨到顶峰的时候就抛出。只要有涨峰就开始计算赚的钱，连续涨可以用两两相减累加来计算，两两相减累加，相当于涨到波峰的最大值减去谷底的值。这一点看通以后，题目非常简单。



