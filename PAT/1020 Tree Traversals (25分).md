# [1020 Tree Traversals (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805485033603072)

- 根据后序遍历和中序遍历还原树，并按层次遍历输出
- 还原用递归，层次遍历用`BFS`

### AC代码

```c++
#include <iostream>
#include <queue>
using namespace std;

struct TreeNode {
    int id;
    TreeNode* lchild;
    TreeNode* rchild;
};

int n;
int postorder[31], inorder[31];
TreeNode* root;

TreeNode* build (int posti, int postj, int ini, int inj) {
    if (posti > postj || ini > inj) {
        return NULL;
    }
    TreeNode* p = new TreeNode;
    p->id = postorder[postj]; // 后序遍历的最后一个节点即根节点
    int pos = ini;
    for (int i = ini; i <= inj; i ++) { // 在中序遍历中找到根节点的位置
        if (inorder[i] == p->id) {
            pos = i;
            break;
        }
    }
    p->lchild = build(posti, posti + pos - ini - 1, ini, pos - 1);
    p->rchild = build(posti + pos - ini, postj - 1, pos + 1, inj);
    return p;
}

int main () {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &postorder[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &inorder[i]);
    }
    root = build(0, n - 1, 0, n - 1);
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            TreeNode* cur = q.front();
            q.pop();
            if (cur != root) {
                printf(" ");
            }
            printf("%d", cur->id);
            if (cur->lchild) {
                q.push(cur->lchild);
            }
            if (cur->rchild) {
                q.push(cur->rchild);
            }
        }
    }
    return 0;
}

```

