# [1099 Build A Binary Search Tree (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805367987355648)

- 给定一个树的结构和对应个树的关键字，构造出唯一一棵二叉搜索树，并输出其层次遍历
- 按二叉搜索树的中序遍历从小到大依次赋值关键字即可，最后`BFS`层次遍历

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <queue>
using namespace std;

struct N {
    int key;
    int left, right;
};

int n;
int list[105];
N tree[105];
int pos;

void inorder(int nid) {
    if (tree[nid].left != -1) {
        inorder(tree[nid].left);
    }
    tree[nid].key = list[pos ++];
    if (tree[nid].right != -1) {
        inorder(tree[nid].right);
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d %d", &tree[i].left, &tree[i].right);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
    }
    sort(list, list + n);
    pos = 0;
    inorder(0);
    bool first = true;
    queue<N*> q;
    q.push(&tree[0]);
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            N* cur = q.front();
            q.pop();
            if (!first) {
                printf(" ");
            }
            printf("%d", cur->key);
            first = false;
            if (cur->left != -1) {
                q.push(&tree[cur->left]);
            }
            if (cur->right != -1) {
                q.push(&tree[cur->right]);
            }
        }
    }
    return 0;
}

```

