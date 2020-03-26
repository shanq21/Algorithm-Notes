# [1013 Battle Over Cities (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805500414115840)

- 简单的并查集计算连通分量个数

### AC代码

```c++
#include <iostream>
using namespace std;

int n, m, k;
int tree[1000];
bool edge[1000][1000];
int check[1000];

int findRoot (int x) {
    if (tree[x] == -1) {
        return x;
    }
    else {
        int tmp = findRoot(tree[x]);
        tree[x] = tmp;
        return tmp;
    }
}

int main () {
    scanf("%d %d %d", &n, &m, &k);
    for (int i = 1; i <= n; i ++) { // 初始化edge
        for (int j = 1; j <= n; j ++) {
            edge[i][j] = false;
        }
    }
    for (int i = 0; i < m; i ++) { // 处理输入数据
        int a, b;
        scanf("%d %d", &a, &b);
        edge[a][b] = true;
        edge[b][a] = true;
    }
    for (int i = 0; i < k; i ++) {
        scanf("%d", &check[i]);
    }
    for (int i = 0; i < k; i ++) { // 遍历checkd
        int lost = check[i];
        for (int j = 1; j <= n; j ++) { // 初始化并查集
            tree[j] = -1;
        }
        for (int j = 1; j <= n; j ++) { // 根据edge信息建立并查集
            if (j == lost) {
                continue;
            }
            for (int k = 1; k <= n; k ++) {
                if (k == lost || !edge[j][k]) {
                    continue;
                }
                int a = findRoot(j);
                int b = findRoot(k);
                if (a != b) { // 如果节点j和k不在同一集合中，则合并
                    tree[a] = b;
                }
            }
        }
        int cnt = 0;
        for (int j = 1; j <= n; j ++) { // 查找连通分量个数
            if (tree[j] == -1) {
                cnt ++;
            }
        }
        cout << cnt - 2 << endl; // 结果为连通分量减去lost再减一
    }
    
    return 0;
}

```

