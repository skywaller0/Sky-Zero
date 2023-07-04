# [455. Candy](https://leetcode.com/problems/candy/)

这段代码是用来解决一个贪心算法的问题，题目是这样的：

有一群孩子站成一排，每个孩子有一个评分。你要给这些孩子分配糖果，要求如下：

- 每个孩子至少要有一颗糖果。
- 评分高的孩子要比他相邻的评分低的孩子得到更多的糖果。
- 返回你需要的最少糖果数量。

代码的思路是这样的：

- 首先获取评分数组的大小，如果小于 2，直接返回数组大小，因为每个孩子至少有一颗糖果。
- 然后创建一个和评分数组大小相同的糖果数组，初始值都为 1，表示每个孩子至少有一颗糖果。
- 接着从左到右遍历评分数组，如果当前孩子的评分比前一个孩子高，那么就给当前孩子多一颗糖果，保证满足条件。
- 再从右到左遍历评分数组，如果当前孩子的评分比后一个孩子高，并且当前孩子的糖果数不大于后一个孩子，那么就给当前孩子多一颗糖果，保证满足条件。
- 最后用一个内置的函数 accumulate 来求和糖果数组，得到最少需要的糖果数量。

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>

using namespace std;

int candy(vector<int> &ratings) {
    int size = ratings.size();
    if (size < 2) {
        return size;
    }
    vector<int> candies(size, 1);
    for (int i = 1; i < size; ++i) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }
    for (int i = size - 2; i >= 0; --i) {
        if (ratings[i] > ratings[i + 1] && candies[i] <= candies[i+1] ) {
            candies[i] = candies[i+1] + 1;
        }
    }
    return accumulate(candies.begin(), candies.end(), 0);
}

int main() {
    vector<int> ratings = {1,0,2};
    cout << candy(ratings) << endl;
    return 0;
}
```



```go
package main

import "fmt"

func candy(ratings []int) int {
	candies := make([]int, len(ratings))
	for i := 1; i < len(ratings); i++ {
		if ratings[i] > ratings[i-1] {
			candies[i] += candies[i-1] + 1
		}
	}
	for i := len(ratings) - 2; i >= 0; i-- { 
		if ratings[i] > ratings[i+1] && candies[i] <= candies[i+1] { 
			candies[i] = candies[i+1] + 1 
		}
	}
	total := 0
	for _, candy := range candies {
		total += candy + 1
	}
	return total
}

func main() {
	fmt.Println(candy([]int{1, 0, 2}))
}

```

把所有孩子的糖果数初始化为1；
先从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的
糖果数加1；再从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数
不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加1。通过这两次遍历，
分配的糖果就可以满足题目要求了。这里的贪心策略即为，在每次遍历中，只考虑并更新相邻一
侧的大小关系。


