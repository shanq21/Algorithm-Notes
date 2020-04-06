# [1106 Lowest Price in Supply Chain (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805362341822464)

- 求供应链网络中最低的价格，`BFS`找离得最近的叶子节点即可

### AC代码

```c++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct N {
    vector<int> child;
    bool isLeaf;
};

int n;
double p, r;
N list[100005];

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    for (int i = 0; i < n; i ++) {
        int m;
        scanf("%d", &m);
        if (m == 0) {
            list[i].isLeaf = true;
        }
        else {
            list[i].isLeaf = false;
            for (int j = 0; j < m; j ++) {
                int cid;
                scanf("%d", &cid);
                list[i].child.push_back(cid);
            }
        }
    }
    double ans = p;
    queue<int> q;
    q.push(0);
    bool found = false;
    int cnt = 0;
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (list[cur].isLeaf) {
                found = true;
                cnt ++;
            }
            if (found) {
                continue;
            }
            for (int j = 0; j < list[cur].child.size(); j ++) {
                q.push(list[cur].child[j]);
            }
        }
        if (found) {
            break;
        }
        ans *= (1 + r / 100.0);
    }
    printf("%.4f %d", ans, cnt);
    return 0;
}

```

