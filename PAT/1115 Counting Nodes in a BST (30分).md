# [1115 Counting Nodes in a BST (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805355987451904)

- 根据整数序列建立二叉搜索树（注意左右孩子的条件），计算最后一层和倒数第二层的节点个数
- 简单，用`BFS`层次遍历一遍即可

### AC代码

```c++
#include <iostream>
#include <queue>
using namespace std;

struct TN {
    int key;
    int left, right;
};

int n;
TN list[1001];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i].key);
        list[i].left = -1;
        list[i].right = -1;
        if (i == 0) {
            continue;
        }
        int cur = 0;
        while (1) {
            if (list[i].key <= list[cur].key) {
                if (list[cur].left == -1) {
                    list[cur].left = i;
                    break;
                }
                else {
                    cur = list[cur].left;
                }
            }
            else {
                if (list[cur].right == -1) {
                    list[cur].right = i;
                    break;
                }
                else {
                    cur = list[cur].right;
                }
            }
        }
    }
    queue<int> q;
    q.push(0);
    int precnt = 0, cnt = 0;
    while (!q.empty()) {
        int size = q.size();
        precnt = cnt;
        cnt = size;
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (list[cur].left != -1) {
                q.push(list[cur].left);
            }
            if (list[cur].right != -1) {
                q.push(list[cur].right);
            }
        }
    }
    printf("%d + %d = %d", cnt, precnt, cnt + precnt);
    return 0;
}

```

