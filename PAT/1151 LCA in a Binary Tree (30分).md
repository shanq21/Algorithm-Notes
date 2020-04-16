# [1151 LCA in a Binary Tree (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/1038430130011897856)

- 寻找二叉树的`LCA`，模板题
- 用的方法比较原始：
  - 建树
  - `set`存关键字判断是否存在
  - `DFS`求两个关键字的路径，比较找出`LCA`

- 本题debug很久，原因是最后输出时把下标写错了……（所以为什么只错了1个测试点？？）

### AC代码

```c++
#include <iostream>
#include <vector>
#include <set>
using namespace std;

struct TN {
    int key;
    int left, right, root;
};

int m, n;
int preorder[10000], inorder[10000];
TN tree[10000];
int a, b;
set<int> key_s;
vector<int> av, bv, apath, bpath;

int build(int ii, int ij, int pi, int pj) {
    if (ij < ii || pj < pi) {
        return -1;
    }
    int root = preorder[pi];
    int pos = ii;
    while (inorder[pos] != root) {
        pos ++;
    }
    tree[pi].key = root;
    tree[pi].left = build(ii, pos - 1, pi + 1, pos - ii + pi);
    tree[pi].right = build(pos + 1, ij, pos - ii + pi + 1, pj);
    return pi;
}

void dfs(int idx) {
    av.push_back(idx);
    bv.push_back(idx);
    if (tree[idx].key == a) {
        apath = av;
    }
    if (tree[idx].key == b) {
        bpath = bv;
    }
    if (tree[idx].left != -1) {
        dfs(tree[idx].left);
    }
    if (tree[idx].right != -1) {
        dfs(tree[idx].right);
    }
    av.pop_back();
    bv.pop_back();
}

int main() {
    scanf("%d %d", &m, &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &inorder[i]);
        key_s.insert(inorder[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &preorder[i]);
    }
    build(0, n - 1, 0, n - 1);
    while (m --) {
        scanf("%d %d", &a, &b);
        if (key_s.find(a) == key_s.end() && key_s.find(b) == key_s.end()) {
            printf("ERROR: %d and %d are not found.\n", a, b);
        }
        else if (key_s.find(a) == key_s.end()) {
            printf("ERROR: %d is not found.\n", a);
        }
        else if (key_s.find(b) == key_s.end()) {
            printf("ERROR: %d is not found.\n", b);
        }
        else {
            dfs(0);
            int pos = 0;
            while (pos < apath.size() && pos < bpath.size()) {
                if (apath[pos] != bpath[pos]) {
                    break;
                }
                pos ++;
            }
            if (pos == apath.size()) {
                printf("%d is an ancestor of %d.\n", a, b);
            }
            else if (pos == bpath.size()) {
                printf("%d is an ancestor of %d.\n", b, a);
            }
            else {
                printf("LCA of %d and %d is %d.\n", a, b, tree[apath[pos - 1]].key);
            }
        }
    }
    return 0;
}

```

