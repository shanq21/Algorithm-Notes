# [1102 Invert a Binary Tree (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805365537882112)

- 镜面反转一棵二叉树，输出层次遍历和中序遍历

### AC代码

```c++
#include <iostream>
#include <queue>

struct N {
    int left, right;
};

int n;
N tree[15];
int root;
bool isChild[15]; // 标记非根节点

void invert(int nid) {
    int tmp = tree[nid].left;
    tree[nid].left = tree[nid].right;
    tree[nid].right = tmp;
    if (tree[nid].left != -1) {
        invert(tree[nid].left);
    }
    if (tree[nid].right != -1) {
        invert(tree[nid].right);
    }
}

int first = true;
void inorder(int nid) {
    if (tree[nid].left != -1) {
        inorder(tree[nid].left);
    }
    if (!first) {
        printf(" ");
    }
    printf("%d", nid);
    first = false;
    if (tree[nid].right != -1) {
        inorder(tree[nid].right);
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        isChild[i] = false;
    }
    for (int i = 0; i < n; i ++) {
        getchar();
        char l, r;
        scanf("%c %c", &l, &r);
        if (l != '-') {
            tree[i].left = l - '0';
            isChild[tree[i].left] = true;
        }
        else {
            tree[i].left = -1;
        }
        if (r != '-') {
            tree[i].right = r - '0';
            isChild[tree[i].right] = true;
        }
        else {
            tree[i].right = -1;
        }
    }
    for (int i = 0; i < n; i ++) { // 找到根节点
        if (!isChild[i]) {
            root = i;
            break;
        }
    }
    invert(root);
    std::queue<int> q;
    q.push(root);
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (cur != root) {
                printf(" ");
            }
            printf("%d", cur);
            if (tree[cur].left != -1) {
                q.push(tree[cur].left);
            }
            if (tree[cur].right != -1) {
                q.push(tree[cur].right);
            }
        }
    }
    printf("\n");
    inorder(root);
    return 0;
}

```

