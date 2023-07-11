# [167.Two Sum II - Input array is sorted ](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

给定一个升序排列的整数数组和一个目标值，找出数组中两个数的下标，使得它们的和等于目标值。如果没有这样的两个数，就返回一个空的数组。代码的逻辑是，用两个指针i和j分别指向数组的首尾，然后不断地移动指针，直到找到满足条件的两个数或者指针相遇。如果两个数的和等于目标值，就返回它们的下标（注意下标从1开始）。如果两个数的和小于目标值，就把左指针i向右移动一位，因为数组是升序的，所以这样可以增大和。如果两个数的和大于目标值，就把右指针j向左移动一位，因为数组是升序的，所以这样可以减小和。

```c++
#include <iostream> // 引入输入输出流的头文件
#include <vector> // 引入向量的头文件

using namespace std; // 使用标准命名空间

vector<int> twoSum(vector<int> &numbers, int target) { // 定义一个函数，接受一个整数向量和一个目标值作为参数，返回一个整数向量作为结果
    int i = 0, j = numbers.size() - 1; // 定义两个指针i和j，分别指向向量的首尾
    while (i < j) { // 当i小于j时，循环执行以下代码
        if (numbers[i] + numbers[j] == target) { // 如果i和j指向的两个数的和等于目标值
            return {i + 1, j + 1}; // 就返回它们的下标（注意下标从1开始）
        }
        if (numbers[i] + numbers[j] < target) { // 如果i和j指向的两个数的和小于目标值
            i++; // 就把左指针i向右移动一位，因为向量是升序的，所以这样可以增大和
        } else { // 否则，即i和j指向的两个数的和大于目标值
            j--; // 就把右指针j向左移动一位，因为向量是升序的，所以这样可以减小和
        }
    }
    return {}; // 如果循环结束后没有找到满足条件的两个数，就返回一个空的向量
}

int main() { // 定义主函数
    vector<int> numbers = {2, 7, 11, 15}; // 定义一个整数向量，并初始化为{2, 7, 11, 15}
    int target = 9; // 定义一个目标值，并初始化为9
    vector<int> result = twoSum(numbers, target); // 调用twoSum函数，传入numbers和target作为参数，得到结果并赋值给result
    for (int i = 0; i < result.size(); i++) { // 遍历result中的每个元素
        cout << result[i] << " "; // 输出每个元素，并在后面加上一个空格
    }
    return 0; // 主函数返回0，表示程序正常结束
}
```
执行用时：8 ms, 在所有 C++ 提交中击败了92.56%的用户

内存消耗：15 MB, 在所有 C++ 提交中击败了99.62%的用户

通过测试用例：23 / 23


```go
package main // 定义包名为main

import "fmt" // 引入格式化输入输出的包

func twoSum(numbers []int, target int) []int { // 定义一个函数，接受一个整数切片和一个目标值作为参数，返回一个整数切片作为结果
	i, j := 0, len(numbers)-1 // 定义两个指针i和j，分别指向切片的首尾
	for i < j { // 当i小于j时，循环执行以下代码
		if numbers[i]+numbers[j] == target { // 如果i和j指向的两个数的和等于目标值
			return []int{i + 1, j + 1} // 就返回它们的下标（注意下标从1开始）
		}
		if numbers[i]+numbers[j] < target { // 如果i和j指向的两个数的和小于目标值
			i++ // 就把左指针i向右移动一位，因为切片是升序的，所以这样可以增大和
		} else { // 否则，即i和j指向的两个数的和大于目标值
			j-- // 就把右指针j向左移动一位，因为切片是升序的，所以这样可以减小和
		}
	}
	return nil // 如果循环结束后没有找到满足条件的两个数，就返回nil
}

func main() { // 定义主函数
	fmt.Println(twoSum([]int{2, 7, 11, 15}, 9)) // 调用twoSum函数，传入一个整数切片{2, 7, 11, 15}和一个目标值9作为参数，并打印结果
}
```
执行用时：12 ms, 在所有 Go 提交中击败了61.23%的用户

内存消耗：5.2 MB, 在所有 Go 提交中击败了99.54%的用户

通过测试用例：23 / 23



这题目很简单，双指针，前加后




