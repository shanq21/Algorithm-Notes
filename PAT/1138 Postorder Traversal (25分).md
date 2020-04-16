# [1138 Postorder Traversal (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805345078067200)

- 给定前序遍历和中序遍历，求后序遍历的第一个值
- `DFS`：
  - 如果有左子树，就搜索左子树
  - 如果没有左子树但有右子树，就搜索右子树
  - 如果左右子树都没有，当前节点即为所求

### AC代码

```c++
#include <iostream>

int n;
int pre[50000], in[50000];
int ans;

void findl(int prei, int prej, int ini, int inj) {
    int pos = ini;
    while (in[pos] != pre[prei]) { // 找到中序遍历中根节点的位置
        pos ++;
    }
    if (pos > ini) { // 如果有左子树
        findl(prei + 1, prei + pos - ini, ini, pos - 1);
    }
    else if (pos < inj) { // 如果没有左子树，但有右子树
        findl(prej - inj + pos + 1, prej, pos + 1, inj);
    }
    else { // 如果左右子树都没有
        ans = pre[prei];
        return;
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &pre[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &in[i]);
    }
    findl(0, n - 1, 0, n - 1);
    printf("%d", ans);
    return 0;
}

```

