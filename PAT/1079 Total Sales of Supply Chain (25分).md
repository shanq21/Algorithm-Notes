# [1079 Total Sales of Supply Chain (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805388447170560)

- 典型的树`DFS`遍历

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

struct N {
    vector<int> child;
    bool isLeaf;
    int product;
};

int n;
double p, r;
N list[100005];
double sales = 0;

void dfs(int id, double price) {
    if (list[id].isLeaf) {
        sales += list[id].product * price;
    }
    else {
        for (int i = 0; i < list[id].child.size(); i ++) {
            dfs(list[id].child[i], price * (1 + r / 100.0));
        }
    }
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);
    for (int i = 0; i < n; i ++) {
        int m;
        scanf("%d", &m);
        if (m == 0) {
            list[i].isLeaf = true;
            scanf("%d", &list[i].product);
        }
        else {
            for (int j = 0; j < m; j ++) {
                int id;
                scanf("%d", &id);
                list[i].child.push_back(id);
            }
            list[i].isLeaf = false;
        }
    }
    dfs(0, p);
    printf("%.1f", sales);
    return 0;
}

```

