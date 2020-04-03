# [1090 Highest Price in Supply Chain (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805376476626944)

- 计算供应链（树）中的最高单价，简单的`DFS`

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n;
double p, r;
vector<int> tree[100005];
int root;

double ans = 0.0;
int cnt = 0;

void dfs(int id, double price) {
    if (tree[id].empty()) {
        if (price > ans) {
            ans = price;
            cnt = 1;
        }
        else if (price == ans) {
            cnt ++;
        }
    }
    else {
        for (int i = 0; i < tree[id].size(); i ++) {
            dfs(tree[id][i], price * (1 + r / 100.0));
        }
    }
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        if (a == -1) {
            root = i;
        }
        else {
            tree[a].push_back(i);
        }
    }
    dfs(root, p);
    printf("%.2f %d", ans, cnt);
    return 0;
}

```

