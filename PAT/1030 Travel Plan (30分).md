# [1030 Travel Plan (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805464397627392)

- 简单的`Dijkstra`算法，边上除了`dis`还有`cost`，直接在遍历时比较即可
  - 注：两个参数可以直接比较，两个以上则不行，参见 [1018 Public Bike Management](https://github.com/shanq21/notes/blob/master/PAT/1018%20Public%20Bike%20Management%20(30%E5%88%86).md)

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

struct E {
    int next;
    int dis;
    int cost;
};

int n, m, s, d;
vector<E> edge[510];
int pre[510];
bool mark[510];
int dis[510], cost[510];
const int inf = 123123123;

int main () {
    scanf("%d %d %d %d", &n, &m, &s, &d);
    for (int i = 0; i < m; i ++) {
        int a, b, c, d;
        scanf("%d %d %d %d", &a, &b, &c, &d);
        E e;
        e.next = b;
        e.dis = c;
        e.cost = d;
        edge[a].push_back(e);
        e.next = a; //无向图
        edge[b].push_back(e);
    }
    for (int i = 0; i < n; i ++) { // 初始化
        pre[i] = -1;
        mark[i] = false;
        dis[i] = inf;
        cost[i] = inf;
    }
    dis[s] = 0;
    cost[s] = 0;
    for (int i = 0; i < n; i ++) {
        int dmin = dis[d];
        int cur = -1;
        for (int j = 0; j < n; j ++) { // 寻找下一个最优点
            if (mark[j]) {
                continue;
            }
            if (dis[j] < dmin) {
                dmin = dis[j];
                cur = j;
            }
        }
        if (cur == -1) {
            break;
        }
        mark[cur] = true;
        for (int j = 0; j < edge[cur].size(); j ++) {
            int ne = edge[cur][j].next;
            int di = edge[cur][j].dis + dis[cur];
            int co = edge[cur][j].cost + cost[cur];
            if (mark[ne]) {
                continue;
            }
            if (di < dis[ne] || (di == dis[ne] && co < cost[ne])) {
                pre[ne] = cur;
                dis[ne] = di;
                cost[ne] = co;
            }
        }
    }
    vector<int> ans;
    int x = d;
    while (x != -1) {
        ans.push_back(x);
        x = pre[x];
    }
    for (int i = ans.size() - 1; i >= 0; i --) { // ans是倒序
        printf("%d ", ans[i]);
    }
    printf("%d %d\n", dis[d], cost[d]);
    return 0;
}

```
