# [406. Queue Reconstruction By Height](https://leetcode.cn/problems/queue-reconstruction-by-height/)
使用了一种贪心算法，即先安排身高最高的人，然后依次安排身高次高的人，每次插入到他们应该在的位置。这样可以保证每个人前面有正确数量的比他高或者一样高的人。这个函数与上一个函数的区别是使用

- 定义一个静态函数，名为cmp，接受两个整数向量的引用作为参数，返回一个布尔值。这个函数用于比较两个向量的大小，按照第一个元素降序，如果第一个元素相同，则按照第二个元素升序。
- 定义一个函数，名为reconstructQueue，接受一个整数向量的引用作为参数，返回一个整数向量。这个函数的目的是根据给定的人群信息，重建队列。每个人的信息由两个整数表示，第一个是身高，第二个是前面有多少人比他高或者一样高。
- 在函数内部，首先对人群信息进行排序，使用cmp函数作为比较器。
- 然后定义一个空的链表，名为que，用于存储重建后的队列。
- 使用一个for循环，从0开始，到人群信息的大小结束，每次增加1，遍历每个人的信息。
- 在循环内部，定义一个变量position，等于当前人的第二个元素，表示他应该在队列中的位置。
- 定义一个迭代器it，指向que的开始位置。
- 使用一个while循环，从position开始，到0结束，每次减少1，移动it到插入位置。
- 使用insert函数，在it位置插入当前人的信息。
- 循环结束后，返回que转换成向量后的结果。



```c++
#include <iostream> // 引入输入输出流库
#include <vector> // 引入向量容器库
#include <algorithm> // 引入算法库

using namespace std; // 使用标准命名空间

static bool cmp(const vector<int> &a, const vector<int> &b) { // 定义一个静态函数，名为cmp，接受两个整数向量的引用作为参数，返回一个布尔值。这个函数用于比较两个向量的大小，按照第一个元素降序，如果第一个元素相同，则按照第二个元素升序。
    if (a[0] == b[0]) return a[1] < b[1]; // 如果两个向量的第一个元素相等，则返回第二个元素的比较结果
    return a[0] > b[0]; // 否则，返回第一个元素的比较结果
}

vector<vector<int>> reconstructQueue(vector<vector<int>> &people) { // 定义一个函数，名为reconstructQueue，接受一个整数向量的引用作为参数，返回一个整数向量。这个函数的目的是根据给定的人群信息，重建队列。每个人的信息由两个整数表示，第一个是身高，第二个是前面有多少人比他高或者一样高。
    sort(people.begin(), people.end(), cmp); // 对人群信息进行排序，使用cmp函数作为比较器
    vector<vector<int>> que; // 定义一个空的向量，名为que，用于存储重建后的队列
    for (int i = 0; i < people.size(); i++) { // 使用一个for循环，从0开始，到人群信息的大小结束，每次增加1，遍历每个人的信息
        int position = people[i][1]; // 定义一个变量position，等于当前人的第二个元素，表示他应该在队列中的位置
        que.insert(que.begin() + position, people[i]); // 使用insert函数，在que的position位置插入当前人的信息
    }
    return que; // 返回重建后的队列
}

int main() { // 定义主函数
    vector<vector<int>> people = {{7, 0}, // 定义一个向量，存储人群信息
                                  {4, 4},
                                  {7, 1},
                                  {5, 0},
                                  {6, 1},
                                  {5, 2}};
    vector<vector<int>> que = reconstructQueue(people); // 调用reconstructQueue函数，传入people作为参数，得到重建后的队列，存储在que中
    for (int i = 0; i < que.size(); i++) { // 使用一个for循环，从0开始，到que的大小结束，每次增加1，遍历每个队列中的人
        cout << que[i][0] << " " << que[i][1] << endl; // 输出每个人的身高和前面有多少人比他高或者一样高
    }
    return 0; // 返回0，表示程序正常结束
}

```



```go
package main // 定义包名为main

import (
	"sort" // 引入排序库
)

func cmp(a, b []int) bool { // 定义一个函数，名为cmp，接受两个整数切片作为参数，返回一个布尔值。这个函数用于比较两个切片的大小，按照第一个元素降序，如果第一个元素相同，则按照第二个元素升序。
	if a[0] == b[0] { // 如果两个切片的第一个元素相等
		return a[1] < b[1] // 则返回第二个元素的比较结果
	}
	return a[0] > b[0] // 否则，返回第一个元素的比较结果
}

func reconstructQueue(people [][]int) [][]int { // 定义一个函数，名为reconstructQueue，接受一个整数切片的切片作为参数，返回一个整数切片的切片。这个函数的目的是根据给定的人群信息，重建队列。每个人的信息由两个整数表示，第一个是身高，第二个是前面有多少人比他高或者一样高。
	sort.Slice(people, func(i, j int) bool { // 对人群信息进行排序，使用sort.Slice函数，并传入一个匿名函数作为比较器
		return cmp(people[i], people[j]) // 调用cmp函数来比较两个切片
	})
	que := make([][]int, 0) // 定义一个空的切片，名为que，用于存储重建后的队列
	for i := 0; i < len(people); i++ { // 使用一个for循环，从0开始，到人群信息的长度结束，每次增加1，遍历每个人的信息
		position := people[i][1] // 定义一个变量position，等于当前人的第二个元素，表示他应该在队列中的位置
		que = append(que[:position], append([][]int{people[i]}, que[position:]...)...) // 使用append函数，在que的position位置插入当前人的信息，并更新que
	}
	return que // 返回重建后的队列
}
```
按照身高排序之后，优先按身高高的people的k来插入，后序插入节点也不会影响前面已经插入的节点，最终按照k的规则完成了队列。
所以在按照身高从大到小排序后：
局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性
全局最优：最后都做完插入操作，整个队列满足题目队列属性







