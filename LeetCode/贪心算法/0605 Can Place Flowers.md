# [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)

判断一个花坛数组中是否能够种下n朵花，要求相邻的两朵花不能种在相邻的位置。它的思路是从左到右遍历花坛数组，每次跳过两个位置，如果当前位置是空的，就尝试种一朵花，然后检查下一个位置是否也是空的，如果不是，就再跳过一个位置。每种一朵花，就把n减一，直到n为零或者遍历完数组。最后判断n是否为零，如果是，就返回true，否则返回false。

```c++
#include <iostream> // 导入输入输出流头文件
#include <vector> // 导入向量头文件
#include <algorithm> // 导入算法头文件

using namespace std; // 使用标准命名空间

bool canPlaceFlowers(vector<int> &flowerbed, int n) { // 定义一个函数，参数是一个整数向量的引用 flowerbed 和一个整数 n，返回值是一个布尔值
    int lenth = flowerbed.size(); // 声明一个变量 lenth，赋值为 flowerbed 的大小
    for (int i = 0; i < lenth && n > 0; i += 2) { // 用一个变量 i 表示当前位置，初始为 0，每次循环后增加 2，直到 i 等于或超过 lenth 或者 n 等于或小于 0
        if (flowerbed[i] == 0) { // 如果当前位置没有种花
            if (i + 1 == lenth || flowerbed[i + 1] == 0) { // 如果当前位置是最后一个位置或者下一个位置也没有种花
                n--; // 将 n 减一，表示可以在当前位置种花
            } else { // 否则
                i++; // 将 i 加一，表示跳过下一个位置
            }
        }
    }
    if (n == 0) { // 如果 n 等于 0，表示可以满足要求
        return true; // 返回 true
    }
    return false; // 否则返回 false
}

int main() { // 定义主函数
    vector<int> flowerbed = {1, 0, 0, 0, 1}; // 定义一个向量 flowerbed，并初始化为 {1, 0, 0, 0, 1}
    int n = 1; // 定义一个整数 n，并赋值为 1
    cout << canPlaceFlowers(flowerbed, n) << endl; // 调用 canPlaceFlowers 函数，并输出结果，换行
    return 0; // 返回 0，表示程序正常结束
}

```

```go
package main // 声明包名为 main

import (
	"fmt" // 导入 fmt 包，用于输出
)

func canPlaceFlowers(flowerbed []int, n int) bool { // 定义一个函数，参数是一个整数切片 flowerbed 和一个整数 n，返回值是一个布尔值
	lenth := len(flowerbed) // 声明一个变量 lenth，赋值为 flowerbed 的长度
	for i := 0; i < lenth && n > 0; i += 2 { // 用一个变量 i 表示当前位置，初始为 0，每次循环后增加 2，直到 i 等于或超过 lenth 或者 n 等于或小于 0
		if flowerbed[i] == 0 { // 如果当前位置没有种花
			if i+1 == lenth || flowerbed[i+1] == 0 { // 如果当前位置是最后一个位置或者下一个位置也没有种花
				n-- // 将 n 减一，表示可以在当前位置种花
			} else { // 否则
				i++ // 将 i 加一，表示跳过下一个位置
			}
		}
	}
	if n == 0 { // 如果 n 等于 0，表示可以满足要求
		return true // 返回 true
	}
	return false // 否则返回 false
}

func main() { // 定义主函数
	fmt.Println(canPlaceFlowers([]int{1, 0, 0, 0, 1}, 1)) // 调用 canPlaceFlowers 函数，并输出结果
}

```
可以种花的基本单元是 00，然后考虑边界情况：末尾的情况需要单独判断，如果末尾为 0，也可以种花。
