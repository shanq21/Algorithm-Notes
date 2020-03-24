# [1003 Emergency (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805523835109376)

- `Dijkstra`，用一个`int`数组维护到某点的最短路径条数


```c++
#include <iostream>
#include <vector>
using namespace std;

int n, m, c1, c2;
int edge[500][500], teams[500]; // 邻接矩阵
int dis[500], resc[500], mark[500], num[500];
const int inf = 123123123;

int main () {
    cin >> n >> m >> c1 >> c2;
    for (int i = 0; i < n; i ++) { // 处理输入数据
        cin >> teams[i];
    }
    for (int i = 0; i < m; i ++) {
        int a, b, c;
        cin >> a >> b >> c;
        edge[a][b] = c;
        edge[b][a] = c;
    }
    for (int i = 0; i < n; i ++) { // 初始化
        dis[i] = inf;
        resc[i] = 0;
        num[i] = 0;
        mark[i] = false;
    }
    dis[c1] = 0;
    resc[c1] = teams[c1];
    num[c1] = 1;
    for (int i = 0; i < n; i ++) {
        int dmin = dis[c2] > 0 ? dis[c2] : inf;
        int cur = -1;
        for (int j = 0; j < n; j ++) {
            if (!mark[j] && dis[j] < dmin) {
                dmin = dis[j];
                cur = j;
            }
        }
        if (cur == -1) { // 没有找到更短的路，循环结束
            break;
        }
        mark[cur] = true; // 标记为已访问
        for (int j = 0; j < n; j ++) { // 更新与cur相邻的点的值
            if (edge[cur][j] == 0 || mark[j]) { // 如果边不存在或者点已经标记过则不处理
                continue;
            }
            if (dis[j] == -1 || dis[j] > dis[cur] + edge[cur][j]) {
                num[j] = num[cur];
                dis[j] = dis[cur] + edge[cur][j];
                resc[j] = resc[cur] + teams[j];
            }
            else if (dis[j] == dis[cur] + edge[cur][j]) {
                num[j] += num[cur];
                if (resc[j] < resc[cur] + teams[j]) {
                    resc[j] = resc[cur] + teams[j];
                }
            }
        }
    }
    cout << num[c2] << " " << resc[c2] << endl;
    
    return 0;
}

```

