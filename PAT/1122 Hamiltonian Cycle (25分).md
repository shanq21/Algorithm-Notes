# [1122 Hamiltonian Cycle (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805351814119424)

- 判断hamilton cycle，简单的图论
- 不知道为什么测试点1总是很诡异地过不去（目测是判断边是否存在那块有问题，但看来看去也没找到是哪里不对），遂参考[柳婼的代码](https://www.liuchuo.net/archives/2748)写了个`vector`就过了

### AC代码

```c++
#include <iostream>
#include <set>
#include <vector>
using namespace std;

int n, m;
bool edge[201][201];

int main() {
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= n; i ++) {
        for (int j = 1; j <= n; j ++) {
            edge[i][j] = false;
        }
    }
    for (int i = 0; i < m; i ++) {
        int a, b;
        scanf("%d %d", &a, &b);
        edge[a][b] = true;
        edge[b][a] = true;
    }
    int k;
    scanf("%d", &k);
    while (k --) {
        bool isHam = true;
        int l;
        scanf("%d", &l);
        set<int> s;
        vector<int> v;
        for (int j = 0; j < l; j ++) {
            int e;
            scanf("%d", &e);
            s.insert(e);
            v.push_back(e);
        }
        if (l != n + 1 || v[0] != v[l - 1] || s.size() != n) {
            isHam = false;
        }
        for (int j = 0; j < v.size() - 1; j ++) {
            if (edge[v[j]][v[j + 1]] == false) {
                isHam = false;
                break;
            }
        }
        if (isHam) {
            printf("YES\n");
        }
        else {
            printf("NO\n");
        }
    }
    return 0;
}

```

