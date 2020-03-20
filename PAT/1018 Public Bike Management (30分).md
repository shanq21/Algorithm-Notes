# [1018 Public Bike Management (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805489282433024)

- 注意：节点上多出的bike只能向前补充，不能向后补充

- 单纯使用Dijkstra求最短路径时，dis拥有最优子结构，但send和back没有

  - 参考用例

  - ```in
    10 4 4 5
    4 8 9 0
    0 1 1
    1 2 1
    1 3 2
    2 3 1
    3 4 1
    ```

  - 在3更新的时候，应该选择从2过来，而不是从1过来，虽然从1过来可以在当时得到更少的back，但不是全局最优的，结果是从2过来的send更少
  - 故应使用Dijkstra找到最优路径后，再dfs分别求send和back

### AC代码

```c++

```

### 非AC代码

- 测试点7未过

```c++
#include <iostream>
#include <vector>
using namespace std;

struct E { // 边
    int next;
    int time;
};

int cap, n, sp, m;
int bike[510];
vector<E> edge[510]; // 邻接链表
int dis[510];
bool mark[510];
int pre[510]; // 前一个节点
int send[510]; // 到该节点为止，需要从PBMC中调出的数量
int back[510]; // 到该节点为止，需要回收的数量
const int inf = 123123123;

int main () {
    scanf("%d %d %d %d", &cap, &n, &sp, &m); // 处理输入数据
    for (int i = 1; i <= n; i ++) {
        scanf("%d", &bike[i]);
    }
    for (int i = 0; i < m; i ++) {
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        E tmp;
        tmp.next = b;
        tmp.time = c;
        edge[a].push_back(tmp); // 无向图
        tmp.next = a;
        edge[b].push_back(tmp);
    }
    for (int i = 0; i <= n; i++) { // 初始化
        dis[i] = inf;
        mark[i] = false;
        pre[i] = -1;
        send[i] = inf;
        back[i] = inf;
    }
    dis[0] = 0;
    send[0] = 0;
    back[0] = 0;
    for (int i = 0; i <= n; i ++) { // n+1个节点
        int dmin = dis[sp]; // 如果dis[sp]有值，则设其为基准
        int cur = -1;
        for (int j = 0; j <= n; j ++) { // 寻找下一个标记的点
            if (mark[j] || dis[j] == inf) {
                continue;
            }
            if (dis[j] < dmin) {
                dmin = dis[j];
                cur = j;
            }
        }
        if (cur == -1) { // 找不到比dmin更小的路则结束
            break;
        }
        mark[cur] = true;
        for (int j = 0; j < edge[cur].size(); j ++) { // 更新cur连接点的值
            int ne = edge[cur][j].next;
            int t = edge[cur][j].time;
            if (mark[ne]) {
                continue;
            }
            int tmps = send[cur], tmpb = back[cur]; // 临时变量，先计算出send[ne]和back[ne]以便之后比较
            if (bike[ne] > cap / 2) {
                tmpb = back[cur] + bike[ne] - cap / 2;
                tmps = send[cur];
            }
            else if (bike[ne] < cap / 2) {
                if (back[cur] == 0) { // 判断cur是否有剩余back，如果没有，则需要从PBMC调出
                    tmpb = back[cur];
                    tmps = send[cur] + cap / 2 - bike[ne];
                }
                else { // 如果cur有剩余back，则不会有剩余send，因此只需判断剩余的back是否能补足ne的send缺口
                    if (back[cur] >= cap / 2 - bike[ne]) { // 如果能补足，则不需要从PBMC再额外调出
                        tmpb = back[cur] - (cap / 2 - bike[ne]);
                        tmps = send[cur];
                    }
                    else { // 如果不能，则需要从PBMC额外调出
                        tmpb = 0;
                        tmps = send[cur] + cap / 2 - bike[ne] - back[cur];
                    }
                }
            }
            if (dis[cur] + t < dis[ne] // 距离小于，更新
                || (dis[cur] + t == dis[ne] && tmps < send[ne]) // 距离等于但send小，更新
                || (dis[cur] + t == dis[ne] && tmps == send[ne] && tmpb < back[ne])) { // 距离等于send等于但back小，更新
                pre[ne] = cur;
                dis[ne] = dis[cur] + t;
                send[ne] = tmps;
                back[ne] = tmpb;
            }
        }
    }
    vector<int> res; // 处理结果
    int x = sp;
    while (pre[x] != -1) {
        x = pre[x];
        res.push_back(x);
    }
    printf("%d ", send[sp]);
    for (int i = res.size() - 1; i >= 0; i --) {
        printf("%d->", res[i]);
    }
    printf("%d %d\n", sp, back[sp]);
    return 0;
}

```

