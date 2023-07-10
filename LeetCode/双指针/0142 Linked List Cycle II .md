# [142. Linked List Cycle II](https://leetcode.com/problems/assign-cookies/)



```c++
#include <iostream> // 引入输入输出流的头文件
#include <vector> // 引入向量的头文件
#include <tuple> // 引入元组的头文件

using namespace std; // 使用标准命名空间

struct ListNode { // 定义一个结构体，表示链表的节点
    int val; // 节点的值
    ListNode *next; // 节点的下一个指针

    ListNode(int x) : val(x), next(nullptr) {} // 节点的构造函数，用给定的值x初始化节点，并把下一个指针设为nullptr
};

pair<bool, ListNode *> hasCycle(ListNode *head) { // 定义一个函数，接受一个链表的头节点作为参数，返回一个布尔值和一个链表节点作为结果
    ListNode *fast = head; // 定义一个指针fast，指向头节点
    ListNode *slow = head; // 定义一个指针slow，指向头节点
    while (slow != nullptr && fast != nullptr && fast->next != nullptr) { // 当slow、fast和fast的下一个指针都不为空时，循环执行以下代码
        fast = fast->next->next; // 把fast指向它的下两个节点
        slow = slow->next; // 把slow指向它的下一个节点
        if (fast == slow) { // 如果fast和slow相等，即相遇了
            return {true, slow}; // 就返回true和slow，表示有环并返回相遇的节点
        }
    }
    return {false, nullptr}; // 如果循环结束后没有相遇，就返回false和nullptr，表示没有环
}

ListNode *detectCycle(ListNode *head) { // 定义一个函数，接受一个链表的头节点作为参数，返回一个链表的节点作为结果
    if (head == nullptr || head->next == nullptr) { // 如果头节点为空或者头节点的下一个指针为空
        return nullptr; // 就返回nullptr
    }
    bool isCycle; // 定义一个布尔变量isCycle，用来存储是否有环
    ListNode *slow; // 定义一个指针slow，用来存储相遇的节点
    tie(isCycle, slow) = hasCycle(head); // 调用hasCycle函数，传入头节点作为参数，并把返回的两个值分别赋给isCycle和slow
    if (!isCycle) { // 如果isCycle为假，即链表没有环
        return nullptr; // 就返回nullptr
    }
    ListNode *fast = head; // 定义一个指针fast，指向头节点
    while (fast != slow) { // 当fast和slow不相等时，循环执行以下代码
        fast = fast->next; // 把fast指向它的下一个节点
        slow = slow->next; // 把slow指向它的下一个节点
    }
    return fast; // 返回fast，即环的入口节点
}

int main() { // 定义主函数
    auto *head = new ListNode(3); // 用new运算符创建一个链表节点，并初始化为{3, nullptr}，把它赋给head指针，并用auto推断类型为ListNode*
    auto *node1 = new ListNode(2); // 用new运算符创建另一个链表节点，并初始化为{2, nullptr}，把它赋给node1指针，并用auto推断类型为ListNode*
    auto *node2 = new ListNode(0); // 用new运算符创建另一个链表节点，并初始化为{0, nullptr}，把它赋给node2指针，并用auto推断类型为ListNode*
    auto *node3 = new ListNode(-4); // 用new运算符创建另一个链表节点，并初始化为{-4, nullptr}，把它赋给node3指针，并用auto推断类型为ListNode*
    head->next = node1; // 把head的下一个指针指向node1，形成链表{3, 2}
    node1->next = node2; // 把node1的下一个指针指向node2，形成链表{3, 2, 0}
    node2->next = node3; // 把node2的下一个指针指向node3，形成链表{3, 2, 0, -4}
    node3->next = node1; // 把node3的下一个指针指向node1，形成环{-4, 2}
    ListNode *res = detectCycle(head); // 调用detectCycle函数，传入head作为参数，并得到结果赋值给res
    cout << res->val << endl; // 输出res的值，即环的入口节点的值
    return 0; // 主函数返回0，表示程序正常结束
}
```



```go
package main // 定义包名为main

import "fmt" // 引入格式化输入输出的包

type ListNode struct { // 定义一个结构体，表示链表的节点
	Val  int       // 节点的值
	Next *ListNode // 节点的下一个指针
}

func detectCycle(head *ListNode) *ListNode { // 定义一个函数，接受一个链表的头节点作为参数，返回一个链表的节点作为结果
	if head == nil || head.Next == nil { // 如果头节点为空或者头节点的下一个指针为空
		return nil // 就返回nil
	}
	isCycle, slow := hasCycle(head) // 调用hasCycle函数，传入头节点作为参数，得到一个布尔值和一个链表节点作为结果
	if !isCycle {                      // 如果布尔值为假，即链表没有环
		return nil // 就返回nil
	}
	fast := head             // 定义一个指针fast，指向头节点
	for fast != slow {       // 当fast和slow不相等时，循环执行以下代码
		fast = fast.Next     // 把fast指向它的下一个节点
		slow = slow.Next     // 把slow指向它的下一个节点
	}
	return fast // 返回fast，即环的入口节点
}

func hasCycle(head *ListNode) (bool, *ListNode) { // 定义一个函数，接受一个链表的头节点作为参数，返回一个布尔值和一个链表节点作为结果
	fast := head                             // 定义一个指针fast，指向头节点
	slow := head                             // 定义一个指针slow，指向头节点
	for slow != nil && fast != nil && fast.Next != nil { // 当slow、fast和fast的下一个指针都不为空时，循环执行以下代码
		fast = fast.Next.Next                // 把fast指向它的下两个节点
		slow = slow.Next                     // 把slow指向它的下一个节点
		if fast == slow {                    // 如果fast和slow相等，即相遇了
			return true, slow                 // 就返回true和slow，表示有环并返回相遇的节点
		}
	}
	return false, nil // 如果循环结束后没有相遇，就返回false和nil，表示没有环
}

func main() {                          // 定义主函数
	head := &ListNode{3, nil}          // 定义一个链表节点，并初始化为{3, nil}
	node1 := &ListNode{2, nil}         // 定义另一个链表节点，并初始化为{2, nil}
	node2 := &ListNode{0, nil}         // 定义另一个链表节点，并初始化为{0, nil}
	node3 := &ListNode{-4, nil}        // 定义另一个链表节点，并初始化为{-4, nil}
	head.Next = node1                  // 把head的下一个指针指向node1，形成链表{3, 2}
	node1.Next = node2                 // 把node1的下一个指针指向node2，形成链表{3, 2, 0}
	node2.Next = node3                 // 把node2的下一个指针指向node3，形成链表{3, 2, 0, -4}
	node3.Next = node1                 // 把node3的下一个指针指向node1，形成环{-4, 2}
	res := detectCycle(head)           // 调用detectCycle函数，传入head作为参数，并得到结果赋值给res
	fmt.Println(res.Val)               // 打印res的值，即环的入口节点的值
}
```



在判断是否有环的基础上，还需要输出环的第一个点。

分析一下判断环的原理。fast 指针一次都 2 步，slow 指针一次走 1 步。令链表 head 到环的一个点需要 x1 步，从环的第一个点到相遇点需要 x2 步，从环中相遇点回到环的第一个点需要 x3 步。那么环的总长度是 x2 + x3 步。

fast 和 slow 会相遇，说明他们走的时间是相同的，可以知道他们走的路程有以下的关系：


```c++
fast 的 t = (x1 + x2 + x3 + x2) / 2
slow 的 t = (x1 + x2) / 1

x1 + x2 + x3 + x2 = 2 * (x1 + x2)

所以 x1 = x3
```

所以 2 个指针相遇以后，如果 slow 继续往前走，fast 指针回到起点 head，两者都每次走一步，那么必定会在环的起点相遇，相遇以后输出这个点即是结果。

知道上面的原理以后，代码就很简单了。这题属于快慢指针的经典题目。

