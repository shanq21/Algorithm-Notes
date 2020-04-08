# [1110 Complete Binary Tree (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805359372255232)

- 判断一棵树是不是完全二叉树
- `BFS`层次遍历这棵树，只需判断两个地方：
  - 如果一个节点有右孩子却没有左孩子，则不是完全二叉树
  - 如果连续的最后一个父节点没有左孩子，却还没有找出所有的节点，则不是完全二叉树

### AC代码

```c++
#include <iostream>
#include <queue>
using namespace std;

struct TN {
    int id;
    int left, right;
};

int n, root, lastnode;
TN list[21];
bool mark[21]; // 标记非根节点
bool isCBT = true;

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++) {
        mark[i] = false;
    }
    for (int i = 0; i < n; i ++) {
        string l, r;
        cin >> l >> r;
        if (l == "-") {
            list[i].left = -1;
        }
        else {
            list[i].left = stoi(l);
            mark[list[i].left] = true;
        }
        if (r == "-") {
            list[i].right = -1;
        }
        else {
            list[i].right = stoi(r);
            mark[list[i].right] = true;
        }
    }
    for (int i = 0; i < n; i ++) { // 寻找根节点
        if (mark[i] == false) {
            root = i;
            break;
        }
    }
    queue<int> q;
    q.push(root);
    int cnt = 1;
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (list[cur].left != -1) {
                cnt ++;
                q.push(list[cur].left);
            }
            else { // 如果没有左孩子
                if (cnt < n) { // 如果还没到结尾，说明不是完全二叉树
                    isCBT = false;
                    break;
                }
                else { // 如果已经到结尾，则最后一个节点为队列的末尾
                    lastnode = q.back();
                }
            }
            if (list[cur].right != -1) {
                if (list[cur].left == -1) { // 如果有右孩子却没有左孩子，说明不是完全二叉树
                    isCBT = false;
                    break;
                }
                cnt ++;
                q.push(list[cur].right);
            }
        }
        if (!isCBT) {
            break;
        }
    }
    if (isCBT) {
        cout << "YES " << lastnode << endl;
    }
    else {
        cout << "NO " << root << endl;
    }
    return 0;
}

```

