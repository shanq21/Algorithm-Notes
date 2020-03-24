# [1087 All Roads Lead to Rome (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805379664297984)

- 典型的`Dijkstra` + `DFS`，保存最短路径上的所有前驱节点，`Dijkstra`结束后再`DFS`寻找最优路径

### AC代码

```c++
#include <iostream>
#include <vector>
#include <map>
#include <cmath>
#include <algorithm>
using namespace std;

struct E {
    int next;
    int cost;
};

int n, k, st, fin;
int chappy[210]; // 常量happy
vector<E> edge[210]; // 邻接链表
int cost[210];
vector<int> pre[210]; // 前驱节点
bool mark[210];
vector<int> ansPath, tpath; // 结果路径，临时路径
int cntPath = 0; // 最短路径条数
int maxTotal = 0;
double maxAver = 0;
map<string, int> dict; // string->int
string itos[210]; // int->string
const int inf = 123123123;

int getId (string s) { // 查找string对应的id
    if (dict.find(s) != dict.end()) {
        return dict[s];
    }
    else {
        int id = dict.size();
        dict[s] = id;
        itos[id] = s;
        return id;
    }
}

void dfsPath (int node) { // dfs查找最优路径
    tpath.push_back(node);
    if (node == st) {
        cntPath ++; // 更新路径条数
        int totalHappy = 0;
        for (int i = 0; i < tpath.size() - 1; i ++) { // 计算总happy
            int u = tpath[i];
            totalHappy += chappy[u];
        }
        double averHappy = totalHappy / (tpath.size() - 1);
        if (totalHappy > maxTotal || (totalHappy == maxTotal && averHappy > maxAver)) {
            maxTotal = totalHappy;
            maxAver = averHappy;
            ansPath.clear();
            for (int i = 0; i < tpath.size(); i ++) { // 倒序存储
                ansPath.push_back(tpath[i]);
            }
        }
    }
    else {
        for (int i = 0; i < pre[node].size(); i ++) { // 查找所有前驱节点
            dfsPath(pre[node][i]);
            tpath.pop_back();
        }
    }
}

int main () {
    string tmps;
    cin >> n >> k >> tmps;
    dict[tmps] = 0; // 把起点设为0
    itos[0] = tmps;
    for (int i = 0; i < n - 1; i ++) {
        int h;
        cin >> tmps >> h;
        int id = getId(tmps);
        chappy[id] = h; // 存入chappy数组
    }
    for (int i = 0; i < k; i ++) {
        string s1, s2;
        int c;
        cin >> s1 >> s2 >> c;
        int id1 = getId(s1), id2 = getId(s2);
        E e; // 存入edge
        e.next = id2;
        e.cost = c;
        edge[id1].push_back(e);
        e.next = id1; // 无向图
        edge[id2].push_back(e);
    }
    fill(mark, mark + n, false); // 初始化
    fill(cost, cost + n, inf);
    cost[st] = 0;
    for (int i = 0; i < n; i ++) { // Dijkstra开始
        int u = -1;
        int cmax = inf;
        for (int j = 0; j < n; j ++) {
            if (!mark[j] && cost[j] < cmax) {
                cmax = cost[j];
                u = j;
            }
        }
        if (u == -1) {
            break;
        }
        mark[u] = true;
        for (int j = 0; j < edge[u].size(); j ++) {
            int v = edge[u][j].next;
            if (mark[v]) {
                continue;
            }
            int co = edge[u][j].cost + cost[u];
            if (co < cost[v]) {
                cost[v] = co;
                pre[v].clear();
                pre[v].push_back(u);
            }
            else if (co == cost[v]) {
                pre[v].push_back(u);
            }
        }
    }
    fin = dict["ROM"]; // 获取终点罗马的id
    dfsPath(fin);
    cout << cntPath << " " << cost[fin] << " " << maxTotal << " " << floor(maxAver) << endl;
    for (int i = ansPath.size() - 1; i >= 0; i --) {
        cout << itos[ansPath[i]];
        if (i > 0) {
            cout << "->";
        }
        else {
            cout << endl;
        }
    }
    return 0;
}

```

