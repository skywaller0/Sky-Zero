# [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)

**Word Ladder**问题的，但是它使用了另一种算法，叫做**双向广度优先搜索**（Bidirectional Breadth-First Search）。这种算法的思想是，同时从开始单词和结束单词出发，每次扩展一层，直到两个方向的搜索相遇，这样可以减少搜索的空间和时间。同时，这段代码还使用了一个回溯图（backtracking graph），用来记录每个单词的前驱单词，以便于最后构造出所有可能的最短转换序列。

具体来说，这段代码的工作流程如下：

- 引入输入输出流、向量、队列、无序映射和无序集合的头文件。
- 使用标准命名空间。
- 声明一个辅助函数backtracking，用于回溯搜索转换序列。
- 定义一个主函数findLadders，接受一个开始单词，一个结束单词，和一个字典，返回所有可能的最短转换序列。
  - 定义一个向量ans，用于存储答案。
  - 定义一个无序集合dict，用于存储字典。
  - 遍历字典中的每个单词w，把它插入到dict中。
  - 如果dict中没有结束单词，那么转换序列不存在，返回空答案。
  - 从dict中删除开始单词和结束单词。
  - 定义两个无序集合q1和q2，分别包含开始单词和结束单词，用于双向广度优先搜索。
  - 定义一个无序映射next，用于存储每个单词对应的前驱单词的列表。
  - 定义两个布尔变量reversed和found，分别表示是否反转搜索方向和是否找到转换序列。
  - 当q1不为空时，重复以下步骤：
    - 定义一个空的无序集合q，用于存储下一层的单词。
    - 对于q1中的每个单词w，执行以下步骤：
      - 把w复制到一个字符串s。
      - 对于s的每个字符i，执行以下步骤：
        - 保存i的值。
        - 把i从a变到z试一遍。
        - 如果i在q2中，说明找到了转换序列。根据reversed的值，把w加到next[i]中或者把i加到next[w]中。把found设为true。
        - 如果i在dict中，说明可以转换为下一个单词。根据reversed的值，把w加到next[i]中或者把i加到next[w]中。把i加到q中。
        - 把i恢复原值。
    - 如果found为true，说明找到了转换序列。跳出循环。
    - 把q中的每个单词从dict中删除，避免重复访问。
    - 如果q的大小小于等于q2的大小，说明从q1向q2搜索更快。把q赋值给q1，作为下一层的单词。
    - 否则，说明从q2向q1搜索更快。反转reversed的值，改变搜索方向。把q2赋值给q1，作为下一层的单词。把q赋值给q2。
  - 如果found为true，使用辅助函数backtracking，从结束单词开始递归地搜索转换序列，并保存到ans中。
  - 返回ans。
- 定义辅助函数backtracking，接受一个当前单词src，一个目标单词dst，一个回溯图next，一个路径path，和一个答案ans。
  - 如果src等于dst，说明找到了一个转换序列。把path加到ans中并结束。
  - 否则，对于next[src]中的每个单词s，执行以下步骤：
    - 把s加到path中。
    - 调用backtracking(s, dst, next, path, ans)。
    - 把s从path中删除。
- 定义主函数main，用于测试findLadders函数。
  - 定义一个开始单词beginWord为"hit"。
  - 定义一个结束单词endWord为"cog"。
  - 定义一个字典wordList为["hot", "dot", "dog", "lot", "log", "cog"]。
  - 调用findLadders函数，把结果赋值给ans。
  - 遍历ans中的每个向量v，打印出其中的每个字符串s，并换行。
  - 返回0。
```c++
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>

using namespace std;
using Set = unordered_set<string_view>;
using Graph = unordered_map<string_view, vector<string_view>>;
vector<string_view> path;
vector<vector<string>> res;
// search in backtrack graph and build the path
void dfs(Graph & g, const string_view & cur, int mindis) {
    if (path.size() == mindis) {
        // not need to check if the current string is target, it must be
        vector<string> fullpath(mindis);
        for (int i = mindis - 1; i >= 0; i--)
            fullpath[mindis - i - 1] = string(path[i]);
        res.push_back(move(fullpath));
    }
    else {
        for (auto & prev : g[cur]) {
            path.push_back(prev);
            dfs(g, prev, mindis);
            path.pop_back();
        }
    }
}
vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
    Set words(wordList.begin(), wordList.end());
    if (!words.count(endWord)) return {};
    // both queue share a common visited set
    Set visited = {beginWord, endWord}, nextlevel;
    // two queues for two directions
    Set qst = {beginWord}, qed = {endWord};
    // backtracking graph with reverse link(ed -> st2 -> st1)
    Graph g;

    int stepst = 1, steped = 1, mindis = -1;
    while (qst.size() && qed.size()) {
        // expanding from the queue with the minimum size
        bool checkst = qst.size() <= qed.size();
        auto & q1 = checkst ? qst : qed;
        auto & q2 = checkst ? qed : qst;
        int & step = checkst ? stepst : steped;

        bool intersect = false;
        for (auto & src : q1) {
            auto cur = string(src);
            for (auto & c : cur) {
                char back = c;
                for (char nc = 'a'; nc <= 'z'; nc++) {
                    if (nc == back) continue; c = nc;
                    if (!words.count(cur)) continue;
                    bool curintersec = q2.count(cur);
                    if (curintersec) intersect = true;
                    // ensure strings do not connect back(loop)
                    // intersect with elements in another ends is not a loop.
                    if (curintersec || !visited.count(cur)) {
                        // get a nontemporary source
                        auto tgt = *words.find(cur);
                        // build bactrack graph
                        if (checkst)
                            g[tgt].push_back(src);
                        else
                            g[src].push_back(tgt);
                        // can not directly add into visited
                        // other wise some strings may not have changes to build graph
                        nextlevel.insert(tgt);
                    }
                }
                c = back;
            }
        }
        if (intersect) {
            mindis = stepst + steped; break;
        }
        // not it's time to update visited.
        visited.insert(nextlevel.begin(), nextlevel.end());
        q1.clear(); swap(q1, nextlevel);
        step++;
    }
    if (mindis == -1) return {};
    path.push_back(endWord);
    dfs(g, endWord, mindis);

    return res;
}

int main() {
    string beginWord = "hit";
    string endWord = "cog";
    vector<string> wordList = {"hot", "dot", "dog", "lot", "log", "cog"};
    vector<vector<string>> ans = findLadders(beginWord, endWord, wordList);
    for (auto &v: ans) {
        for (auto &s: v) {
            cout << s << " ";
        }
        cout << endl;
    }
    return 0;
}
```
执行用时：8 ms, 在所有 C++ 提交中击败了99.82%的用户

内存消耗：9.1 MB, 在所有 C++ 提交中击败了76.49%的用户

通过测试用例：36 / 36


```go
package main // 声明包名为main

import "fmt" // 引入list包，用来实现队列

func findLadders(beginWord string, endWord string, wordList []string) [][]string {
	result, wordMap := make([][]string, 0), make(map[string]bool)
	for _, w := range wordList {
		wordMap[w] = true
	}
	if !wordMap[endWord] {
		return result
	}
	// create a queue, track the path
	queue := make([][]string, 0)
	queue = append(queue, []string{beginWord})
	// queueLen is used to track how many slices in queue are in the same level
	// if found a result, I still need to finish checking current level cause I need to return all possible paths
	queueLen := 1
	// use to track strings that this level has visited
	// when queueLen == 0, remove levelMap keys in wordMap
	levelMap := make(map[string]bool)
	for len(queue) > 0 {
		path := queue[0]
		queue = queue[1:]
		lastWord := path[len(path)-1]
		for i := 0; i < len(lastWord); i++ {
			for c := 'a'; c <= 'z'; c++ {
				nextWord := lastWord[:i] + string(c) + lastWord[i+1:]
				if nextWord == endWord {
					path = append(path, endWord)
					result = append(result, path)
					continue
				}
				if wordMap[nextWord] {
					// different from word ladder, don't remove the word from wordMap immediately
					// same level could reuse the key.
					// delete from wordMap only when currently level is done.
					levelMap[nextWord] = true
					newPath := make([]string, len(path))
					copy(newPath, path)
					newPath = append(newPath, nextWord)
					queue = append(queue, newPath)
				}
			}
		}
		queueLen--
		// if queueLen is 0, means finish traversing current level. if result is not empty, return result
		if queueLen == 0 {
			if len(result) > 0 {
				return result
			}
			for k := range levelMap {
				delete(wordMap, k)
			}
			// clear levelMap
			levelMap = make(map[string]bool)
			queueLen = len(queue)
		}
	}
	return result
}

func main() {
	// 执行findLadders函数
	fmt.Println(findLadders("hit", "cog", []string{"hot", "dot", "dog", "lot", "log", "cog"}))
}

```




