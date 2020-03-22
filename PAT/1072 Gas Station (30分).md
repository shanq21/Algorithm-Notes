# [1072 Gas Station (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805396953219072)

- 简单的`Dijkstra`，但需要处理一下结果
- 注意：计算某个候选点到居民房的最短路径时，需要把剩余的候选点也作为节点考虑进去

### AC代码

```c++
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct E {
    int next;
    int dist;
};

struct Res {
    int minDist;
    double averDist;
};

int n, m, k, ds;
vector<E> edge[1020];
int dis[1020];
bool mark[1020];
Res res[15]; // 记录加油站到最近居民楼之间的距离
const int inf = 123123123;

int main () {
    scanf("%d %d %d %d", &n, &m, &k, &ds); // 处理输入数据
    for (int i = 0; i < k; i ++) {
        string s1, s2;
        int d;
        cin >> s1 >> s2 >> d;
        int a = s1[0] == 'G' ? stoi(s1.substr(1)) + n : stoi(s1); // 加油站编号为 n + 1 ~ n + m
        int b = s2[0] == 'G' ? stoi(s2.substr(1)) + n : stoi(s2);
        E e;
        e.next = b;
        e.dist = d;
        edge[a].push_back(e);
        e.next = a;
        edge[b].push_back(e);
    }
    for (int i = 1; i <= m; i ++) {
        for (int j = 1; j <= n + m; j ++) { // 初始化
            dis[j] = inf;
            mark[j] = false;
        }
        dis[n + i] = 0; // 加油站n+i
        for (int j = 0; j < n + m; j ++) { // Dijkstra
            int cur = -1, dmin = inf;
            for (int k = 1; k <= n + m; k ++) {
                if (!mark[k] && dis[k] < dmin) {
                    dmin = dis[k];
                    cur = k;
                }
            }
            if (cur == -1) {
                break;
            }
            mark[cur] = true;
            for (int k = 0; k < edge[cur].size(); k ++) {
                int ne = edge[cur][k].next;
                int d = edge[cur][k].dist + dis[cur];
                if (!mark[ne] && d < dis[ne]) {
                    dis[ne] = d;
                }
            }
            
        }
        int dmin = inf; // 处理结果
        double total = 0;
        bool isValid = true;
        for (int j = 1; j <= n; j ++) {
            if (dis[j] > ds) { // 存在无法到达的节点则无效
                isValid = false;
                break;
            }
            total += dis[j];
            if (dis[j] < dmin) {
                dmin = dis[j];
            }
        }
        if (!isValid) {
            res[i].minDist = inf;
            continue;
        }
        res[i].minDist = dmin;
        res[i].averDist = total / n;
    }
    int dmax = 0;
    double dmin = inf;
    int ans = -1;
    for (int i = 1; i <= m; i ++) { // 找出最佳点
        if (res[i].minDist == inf) {
            continue;
        }
        if (res[i].minDist > dmax || (res[i].minDist == dmax && res[i].averDist < dmin)) {
            dmax = res[i].minDist;
            dmin = res[i].averDist;
            ans = i;
        }
    }
    if (ans == -1) {
        printf("No Solution\n");
    }
    else {
        double x = dmax; // 把int转成double
        printf("G%d\n%.1f %.1f\n", ans, x, dmin);
    }
    return 0;
}

```

