# [1142 Maximal Clique (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343979159552)

- 判断给定集合是否为maximal clique：
  - 如果集合中的任意两点都相邻，则是clique
  - 如果集合中不能再加入其他点成为新的clique，则为maximal clique
- 判断逻辑写在代码注释中

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n, m;
bool edge[201][201];
bool mark[201];

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i ++) {
        for (int j = 1; j <= n; j ++) {
            edge[i][j] = false;
        }
    }
    while (m --) {
        int a, b;
        cin >> a >> b;
        edge[a][b] = edge[b][a] = true;
    }
    cin >> m;
    while (m --) {
        int k;
        cin >> k;
        bool isClique = true;
        vector<int> v;
        for (int i = 1; i <= n; i ++) {
            mark[i] = false;
        }
        for (int i = 0; i < k; i ++) {
            int a;
            cin >> a;
            v.push_back(a);
            mark[a] = true;
        }
        for (int i = 0; i < k; i ++) { // 如果集合中任一两点不相邻，则不是clique
            for (int j = i + 1; j < k; j ++) {
                if (edge[v[i]][v[j]] == false) {
                    isClique = false;
                    break;
                }
            }
            if (!isClique) {
                break;
            }
        }
        if (!isClique) {
            printf("Not a Clique\n");
            continue;
        }
        bool isMax = true;
        for (int i = 1; i <= n; i ++) { // 尝试加入图中的其他点
            if (mark[i] == true) { // 已经在集合中的跳过
                continue;
            }
            bool found = true;
            for (int j = 0; j < k; j ++) {
                if (edge[i][v[j]] == false) { // 如果加入的点与任一点不相邻，则该点无效
                    found = false;
                    break;
                }
            }
            if (found) { // 如果加入的点与集合中的所有点都相邻，则该点有效，说明原集合不是maximal
                isMax = false;
            }
        }
        if (isMax) {
            printf("Yes\n");
        }
        else {
            printf("Not Maximal\n");
        }
    }
    return 0;
}

```

