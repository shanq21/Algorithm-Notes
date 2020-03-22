# [1076 Forwards on Weibo (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805392092020736)

- 很简单的`BFS`（所以为啥是30分= =）
- 邻接矩阵存储，有向图

### AC代码

```c++
#include <iostream>
#include <queue>
using namespace std;

int n, l, k;
bool edge[1010][1010]; // 邻接矩阵
queue<int> Q;
int mark[1010];

int main () {
    scanf("%d %d", &n, &l); // 处理输入数据
    for (int i = 1; i <= n; i ++) { // 初始化edge
        for (int j = 1; j <= n; j ++) {
            edge[i][j] = false;
        }
    }
    for (int i = 1; i <= n; i ++) {
        int m;
        scanf("%d", &m);
        for (int j = 0; j < m; j ++) {
            int a;
            scanf("%d", &a);
            edge[a][i] = true; // a followed by i
        }
    }
    scanf("%d", &k);
    for (int i = 0; i < k; i ++) {
        int u;
        scanf("%d", &u);
        for (int j = 1; j <= n; j ++) { // 初始化标记数组
            mark[j] = false;
        }
        int step = 0; // 层数
        int cnt = 0; // 转发数
        Q.push(u);
        mark[u] = true;
        while (!Q.empty()) {
            step ++;
            int size = Q.size();
            for (int j = 0; j < size; j ++) {
                int cur = Q.front();
                Q.pop();
                for (int k = 1; k <= n; k ++) {
                    if (!mark[k] && edge[cur][k] == true) { // 存在边且未标记
                        Q.push(k);
                        mark[k] = true;
                        cnt ++;
                    }
                }
            }
            if (step == l) {
                break;
            }
        }
        printf("%d\n", cnt);
    }
    return 0;
}

```

