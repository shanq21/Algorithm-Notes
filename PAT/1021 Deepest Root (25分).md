# [1021 Deepest Root (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805482919673856)

- 并查集
  - 一是用来判断图是否有环
    - 当新输入边的顶点同集时，说明存在环
  - 二是用来计算图的连通分量个数
- 递归求树高
  - 注意代码顺序

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Res {
    int root;
    int depth;
};

int n;
vector<vector<int>> edge;
vector<int> root; // 能成为根的点
vector<int> ufind; // 并查集
vector<Res> res;

int findRoot (int x) { // 并查集配套算法，含压缩路径
    if (ufind[x] == -1) {
        return x;
    }
    else {
        int tmp = findRoot(ufind[x]);
        ufind[x] = tmp;
        return tmp;
    }
}

int depth (int cur, int pre) { // 查找深度
    int d = 0;
    for (int i = 0; i < edge[cur].size(); i ++) {
        int next = edge[cur][i];
        if (next != pre) {
            d = max(d, depth(next, cur));
        }
    }
    return d + 1;
}

bool cmp (Res a, Res b) {
    if (a.depth != b.depth) {
        return a.depth > b.depth;
    }
    else {
        return a.root < b.root;
    }
}

int main () {
    scanf("%d", &n);
    if (n == 1) {
        printf("1\n");
        return 0;
    }
    edge.resize(n + 1); // 初始化edge
    ufind.resize(n + 1); // 初始化并查集
    for (int i = 1; i <= n; i ++) {
        ufind[i] = -1;
    }
    bool hasRing = false;
    for (int i = 0; i < n - 1; i ++) { // 处理输入数据
        int a, b;
        scanf("%d %d", &a, &b);
        edge[a].push_back(b); // 无向图，存两次边
        edge[b].push_back(a);
        a = findRoot(a); // 建立并查集
        b = findRoot(b);
        if (a != b) {
            ufind[a] = b;
        }
        else { // 如果边的两点原先就连通，则说明存在环
            hasRing = true;
        }
    }
    int cnt = 0; // 查找连通分量的个数
    for (int i = 1; i <= n; i ++) {
        if (ufind[i] == -1) {
            cnt ++;
        }
    }
    if (cnt > 1 || hasRing == true) { // 连通分量大于1，或存在环，则非树
        printf("Error: %d components\n", cnt);
        return 0;
    }
    for (int i = 1; i <= n; i ++) { // 找出可以成为根的节点
        if (edge[i].size() == 1) {
            root.push_back(i);
        }
    }
    for (int i = 0; i < root.size(); i ++) { // 依次判断
        Res r;
        r.root = root[i];
        r.depth = depth(r.root, 0);
        res.push_back(r);
    }
    sort(res.begin(), res.end(), cmp); // 按需排序
    int d = res[0].depth; // 记录最大深度
    for (int i = 0; i < res.size(); i ++) {
        if (res[i].depth != d) { // 答案到此结束
            break;
        }
        printf("%d\n", res[i].root);
    }
    return 0;
}

```

