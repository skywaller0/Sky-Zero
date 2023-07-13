# [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

给定一个整数数组 nums 和一个整数 k，找出数组中出现次数最多的 k 个元素。例如，如果 nums = [1, 1, 1, 2, 2, 3]，k = 2，那么出现次数最多的两个元素是 1 和 2。

这个代码使用了一个叫做优先队列的数据结构，它可以按照元素的优先级从大到小或从小到大排列。这里使用了一个按照出现次数从大到小排列的优先队列，存放了每个元素和它的出现次数。然后从优先队列中取出前 k 个元素，就是出现次数最多的 k 个元素。

代码中的结构体 Item 定义了一个包含元素的值和出现次数的结构体。优先队列 PriorityQueue 是一个使用 vector 和 function 来实现的优先队列，它可以存放 Item 结构体，并按照出现次数从大到小排序。函数 topKFrequent 就是实现了这个算法，它首先遍历数组，用一个哈希表记录每个元素和它的出现次数，然后将哈希表中的键值对转换为 Item 结构体，并压入优先队列中，最后从优先队列中取出前 k 个元素，并将它们的值存入一个向量中，作为结果返回。

主函数 main 是程序的入口点，它定义了一个测试用例 nums = [1, 1, 1, 2, 2, 3] 和 k = 2，并调用 topKFrequent 函数来查找出现次数最多的两个元素，并将结果输出到控制台上。

```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量容器库
#include <unordered_map> // 引入哈希表容器库
#include <functional> // 引入函数对象库
#include <queue> // 引入优先队列容器库

using namespace std; // 使用标准命名空间

struct Item { // 定义一个结构体，用于存放元素的值和出现次数
    int key; // 存放元素的值
    int count; // 存放元素的出现次数
    Item(int k, int c) : key(k), count(c) {} // 构造函数，用于初始化结构体
};

// A PriorityQueue is a priority_queue that holds Items.
typedef priority_queue<Item, vector<Item>, function<bool(Item, Item)>> PriorityQueue; // 定义一个类型别名，表示一个存放 Item 结构体的优先队列，使用 vector 和 function 来实现

vector<int> topKFrequent(vector<int>& nums, int k) { // 定义一个函数，用于查找数组中出现次数最多的 k 个元素，并返回一个向量
    unordered_map<int, int> m; // 定义一个哈希表，用于存放元素和出现次数的映射
    for (int n : nums) { // 遍历数组中的每个元素 n
        m[n]++; // 将 n 的出现次数加一
    }
    PriorityQueue q([](Item a, Item b) { return a.count < b.count; }); // 定义一个优先队列，按照出现次数从大到小排序，使用 lambda 表达式来定义比较函数
    for (auto& p : m) { // 遍历哈希表中的每个键值对 p
        q.push(Item(p.first, p.second)); // 将 p 转换为 Item 结构体，并压入优先队列中
    }
    vector<int> result; // 定义一个向量，用于存放结果
    while (result.size() < k) { // 当结果的大小小于 k 时
        Item item = q.top(); // 取出优先队列的顶部元素，即出现次数最多的元素
        q.pop(); // 弹出优先队列的顶部元素
        result.push_back(item.key); // 将该元素的值加入结果中
    }
    return result; // 返回结果
}

int main() { // 定义主函数，程序入口点
    vector<int> nums = {1, 1, 1, 2, 2, 3}; // 定义一个向量，存放要查找的数组
    int k = 2; // 定义一个整数变量，存放要查找的第 k 大的元素
    vector<int> result = topKFrequent(nums, k); // 调用 topKFrequent 函数来查找出现次数最多的两个元素，并将其值赋给 result 变量
    for (int n : result) { // 遍历 result 中的每个元素 n
        cout << n << " "; // 输出 n 的值和一个空格符
    }
    cout << endl; // 输出一个换行符
    return 0; // 返回 0，表示程序正常结束
}
```
执行用时：12 ms, 在所有 C++ 提交中击败了84.47%的用户

内存消耗：13.3 MB, 在所有 C++ 提交中击败了48.77%的用户

通过测试用例：21 / 21


```go
package main // 定义包名为 main

import (
	"container/heap" // 引入 container/heap 包，用于实现堆操作
)

func topKFrequent(nums []int, k int) []int { // 定义一个函数，用于查找数组中出现次数最多的 k 个元素，并返回一个切片
	m := make(map[int]int) // 定义一个映射，用于存放元素和出现次数的键值对
	for _, n := range nums { // 遍历数组中的每个元素 n
		m[n]++ // 将 n 的出现次数加一
	}
	q := PriorityQueue{} // 定义一个优先队列，用于存放元素和出现次数的结构体
	for key, count := range m { // 遍历映射中的每个键值对
		heap.Push(&q, &Item{key: key, count: count}) // 将键值对转换为结构体，并压入优先队列中
	}
	var result []int // 定义一个切片，用于存放结果
	for len(result) < k { // 当结果的长度小于 k 时
		item := heap.Pop(&q).(*Item) // 取出优先队列的顶部元素，即出现次数最多的元素
		result = append(result, item.key) // 将该元素的值加入结果中
	}
	return result // 返回结果
}

// Item define
type Item struct {
	key   int // 存放元素的值
	count int // 存放元素的出现次数
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item // 定义一个类型别名，表示一个存放 Item 结构体的切片，实现了 heap.Interface 接口

func (pq PriorityQueue) Len() int { // 定义一个方法，用于返回优先队列的长度
	return len(pq)
}

func (pq PriorityQueue) Less(i, j int) bool { // 定义一个方法，用于比较优先队列中两个元素的优先级
	// 注意：因为 golang 中的 heap 默认是按最小堆组织的，所以 count 越大，Less() 越小，越靠近堆顶。这里采用 >，变为最大堆
	return pq[i].count > pq[j].count
}

func (pq PriorityQueue) Swap(i, j int) { // 定义一个方法，用于交换优先队列中两个元素的位置
	pq[i], pq[j] = pq[j], pq[i]
}

// Push define
func (pq *PriorityQueue) Push(x interface{}) { // 定义一个方法，用于压入一个元素到优先队列中
	item := x.(*Item) // 将 x 转换为 Item 结构体指针
	*pq = append(*pq, item) // 将 item 加入到切片末尾
}

// Pop define
func (pq *PriorityQueue) Pop() interface{} { // 定义一个方法，用于弹出优先队列中的顶部元素，并返回其值
	n := len(*pq) // 获取切片的长度
	item := (*pq)[n-1] // 获取切片末尾的元素，即要弹出的元素
	*pq = (*pq)[:n-1] // 将切片缩短到 n-1 的长度，即删除末尾的元素
	return item // 返回弹出的元素
}

func main() { // 定义主函数，程序入口点
	var nums = []int{1, 1, 1, 2, 2, 3} // 定义一个切片，存放要查找的数组
	var k = 2 // 定义一个整数变量，存放要查找的第 k 大的元素
	println(topKFrequent(nums, k)) // 输出 topKFrequent 函数的返回值
}
```

执行用时：12 ms, 在所有 Go 提交中击败了79.19%的用户

内存消耗：5.2 MB, 在所有 Go 提交中击败了43.74%的用户

通过测试用例：21 / 21

就是用哈希表统计字符出现的次数，然后用优先队列来排序，最后取出前 k 个元素。

