# [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

给你一个数组 children，表示每个孩子的胃口值，以及一个数组 cookies，表示每块饼干的大小。你的任务是尽可能地满足更多的孩子，但是每个孩子最多只能吃一块饼干，而且只有当饼干的大小大于等于孩子的胃口值时，孩子才能吃到饼干。返回你能满足的孩子的最大数量。

代码的思路是这样的：

- 首先对两个数组进行升序排序，这样可以保证从小到大地分配饼干。
- 然后用两个指针 child 和 cookie 分别指向 children 和 cookies 的起始位置。
- 接着进行一个循环，直到有一个指针超出了数组的范围为止。
- 在循环中，如果当前的饼干大小大于等于当前的孩子胃口值，说明可以满足这个孩子，那么就让 child 指针后移一位，表示已经分配了一块饼干给这个孩子。
- 不管是否满足了孩子，都要让 cookie 指针后移一位，表示已经考虑了这块饼干。
- 最后返回 child 指针的值，就是能满足的孩子的数量。

```c++
int findContentChildren(vector<int>& children, vector<int>& cookies) {
    sort(children.begin(), children.end());
    sort(cookies.begin(), cookies.end());
    int child = 0, cookie = 0;
    while (child < children.size() && cookie < cookies.size()) {
       if (children[child] <= cookies[cookie]) ++child;
       ++cookie;
    }
    return child;
}
```



```go
func findContentChildren(children []int, cookies []int) int {
	sort.Ints(children)
	sort.Ints(cookies)
	child, cookie := 0, 0
	for child < len(children) && cookie < len(cookies) {
		if cookies[cookie] >= children[child] {
			cookie++
			child++
		} else {
			cookie++
		}
	}
	return child
}
```


这个题目大致的思路就是这样，但是这个题目还有一个进阶的问题，就是如何让分配的饼干尽可能地大，这样才能满足更多的孩子。这个问题可以用贪心算法来解决，具体的思路是这样的：

这个问题的一个可能的解法是：

- 首先对两个数组进行降序排序，这样可以保证从大到小地分配饼干。
- 然后用两个指针 child 和 cookie 分别指向 children 和 cookies 的起始位置。
- 接着进行一个循环，直到有一个指针超出了数组的范围为止。
- 在循环中，如果当前的饼干大小大于等于当前的孩子胃口值，说明可以满足这个孩子，那么就让 child 指针后移一位，表示已经分配了一块饼干给这个孩子。
- 不管是否满足了孩子，都要让 cookie 指针后移一位，表示已经考虑了这块饼干。
- 最后返回 child 指针的值，就是能满足的孩子的数量。

这样做的好处是，每次都优先分配最大的饼干给最贪心的孩子，这样可以减少浪费饼干的可能性，也可以提高满足孩子的概率。



