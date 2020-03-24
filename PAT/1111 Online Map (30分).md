# [1111 Online Map (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805358663417856)

- 用两个`Dijkstra`分别求最短路径和最快路径
  - 含第二标尺的比较

### AC代码1

- 用一个`vector`数组维护前驱节点，之后再`DFS`找出节点数最少的路


```c++
#include <iostream>
#include <vector>
using namespace std;

struct E {
    int next;
    int length;
    int time;
};

int n, m, s, d;
vector<E> edge[510];
int dis[510], dpre[510];
int tim[510];
vector<int> tpre[510];
bool mark[510];
vector<int> dpath, tpath;
const int inf = 123123123;

vector<int> tmpv;
void dfs (int node) { // 处理tpath，找到节点最少的路径
    tmpv.push_back(node);
    if (node == s) {
        if (tpath.size() == 0 || tmpv.size() < tpath.size()) {
            tpath.clear();
            for (int i = 0; i < tmpv.size(); i ++) { // 倒序存储
                tpath.push_back(tmpv[i]);
            }
        }
    }
    else {
        for (int i = 0; i < tpre[node].size(); i ++) {
            dfs(tpre[node][i]);
            tmpv.pop_back(); // 维护路径
        }
    }
}

int main () {
    scanf("%d %d", &n, &m); // 处理输入数据
    for (int i = 0; i < m; i ++) {
        int a, b, oneway, len, t;
        scanf("%d %d %d %d %d", &a, &b, &oneway, &len, &t);
        E e;
        e.next = b;
        e.length = len;
        e.time = t;
        edge[a].push_back(e);
        if (oneway == 0) { // 如果不是单行道
            e.next = a;
            edge[b].push_back(e);
        }
    }
    scanf("%d %d", &s, &d);
    for (int i = 0; i < n; i ++) { // 初始化
        mark[i] = false;
        dis[i] = inf;
        tim[i] = inf;
        dpre[i] = -1;
    }
    dis[s] = 0; // dis的Dijkstra
    for (int i = 0; i < n; i ++) {
        int cur = -1;
        int dmax = inf;
        for (int j = 0; j < n; j ++) {
            if (!mark[j] && dis[j] < dmax) {
                dmax = dis[j];
                cur = j;
            }
        }
        if (cur == -1) {
            break;
        }
        mark[cur] = true;
        for (int j = 0; j < edge[cur].size(); j ++) {
            int u = edge[cur][j].next;
            int di = edge[cur][j].length + dis[cur];
            int ti = edge[cur][j].time + tim[cur];
            if (mark[u]) {
                continue;
            }
            if (di < dis[u] || (di == dis[u] && ti < tim[u])) {
                dis[u] = di;
                tim[u] = ti;
                dpre[u] = cur;
            }
        }
    }
    for (int i = 0; i < n; i ++) { // 初始化
        mark[i] = false;
        tim[i] = inf;
    }
    tim[s] = 0; // time的Dijkstra
    for (int i = 0; i < n; i ++) {
        int cur = -1;
        int tmax = inf;
        for (int j = 0; j < n; j ++) {
            if (!mark[j] && tim[j] < tmax) {
                tmax = tim[j];
                cur = j;
            }
        }
        if (cur == -1) {
            break;
        }
        mark[cur] = true;
        for (int j = 0; j < edge[cur].size(); j ++) {
            int u = edge[cur][j].next;
            int ti = edge[cur][j].time + tim[cur];
            if (mark[u]) {
                continue;
            }
            if (ti < tim[u]) {
                tim[u] = ti;
                tpre[u].clear();
                tpre[u].push_back(cur);
            }
            else if (ti == tim[u]) {
                tpre[u].push_back(cur);
            }
        }
    }
    int idx = d; // 处理dpath
    while (idx != -1) {
        dpath.push_back(idx); // 倒序存储
        idx = dpre[idx];
    }
    dfs(d); // 处理tpath
    bool isIdentical = true; // 判断两条路是否相同
    if (dpath.size() != tpath.size()) {
        isIdentical = false;
    }
    else {
        int size = dpath.size();
        for (int i = 0; i < size; i ++) {
            if (dpath[i] != tpath[i]) {
                isIdentical = false;
                break;
            }
        }
    }
    if (isIdentical) { // 输出第一条路的前半部分
        printf("Distance = %d; Time = %d: ", dis[d], tim[d]);
    }
    else {
        printf("Distance = %d: ", dis[d]);
    }
    for (int i = dpath.size() - 1; i >= 0; i --) { // 输出第一条路的后半部分
        printf("%d", dpath[i]);
        if (i != 0) {
            printf(" -> ");
        }
        else {
            printf("\n");
        }
    }
    if (!isIdentical) { // 如果两条路不同，则还要输出第二条
        printf("Time = %d: ", tim[d]);
        for (int i = tpath.size() - 1; i >= 0; i --) {
            printf("%d", tpath[i]);
            if (i != 0) {
                printf(" -> ");
            }
            else {
                printf("\n");
            }
        }
    }
    return 0;
}

```

### AC代码2

- 用两个`int`数组分别维护前驱节点和节点的个数，直接在`Dijkstra`算法块中处理
- 关键代码：

```c++
int tim[510], tpre[510], num[510]; // 最快路径的时间、前驱节点、到达该节点时的节点个数

if (ti < tim[u] || (ti == tim[u] && num[cur] + 1 < num[u])) {
  tim[u] = ti;
  num[u] = num[cur] + 1;
  tpre[u] = cur;
}
```

