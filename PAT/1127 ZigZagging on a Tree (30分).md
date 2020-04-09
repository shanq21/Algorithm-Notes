# [1127 ZigZagging on a Tree (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805349394006016)

- 根据中序遍历和后序遍历建立二叉树，再轮流从左到右从右到左层次遍历这棵树
- 设置一个`flag`变量，当从左往右时正常输出，当从右往左时先保存在一个栈里，最后再输出

### AC代码

```c++
#include <iostream>
#include <queue>
#include <stack>

struct TN {
    int left, right;
};

int n;
int inorder[31], postorder[31];
TN list[31];

int build(int ii, int ij, int pi, int pj) {
    if (ij < ii || pj < pi) {
        return -1;
    }
    int root = postorder[pj];
    int pos = ii;
    while (inorder[pos] != root) {
        pos ++;
    }
    list[root].left = build(ii, pos - 1, pi, pi + pos - 1 - ii);
    list[root].right = build(pos + 1, ij, pi + pos - ii, pj - 1);
    return root;
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &inorder[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &postorder[i]);
    }
    build(0, n - 1, 0, n - 1);
    int root = postorder[n - 1];
    std::queue<int> q;
    q.push(root);
    int flag = 1;
    while (!q.empty()) {
        int size = q.size();
        std::stack<int> sta;
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (flag == 0) {
                printf(" %d", cur);
            }
            else if (flag == 1) {
                sta.push(cur);
            }
            if (list[cur].left != -1) {
                q.push(list[cur].left);
            }
            if (list[cur].right != -1) {
                q.push(list[cur].right);
            }
        }
        if (flag == 0) {
            flag = 1;
        }
        else if (flag == 1) {
            while (!sta.empty()) {
                if (sta.top() != root) {
                    printf(" ");
                }
                printf("%d", sta.top());
                sta.pop();
            }
            flag = 0;
        }
    }
    return 0;
}

```

