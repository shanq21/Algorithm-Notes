# [1154 Vertex Coloring (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/1071785301894295552)

- 给出图中各点的颜色，判断是否有同一条边上的两点颜色相同
- 这题的`N`可以达到`10000`，遍历点的复杂度为$O(N^2)$，会超时，应遍历边（复杂度为$O(E)$）

### AC代码

```c++
#include <iostream>
#include <set>
#include <vector>
using namespace std;

int n, m, k;
vector<int> edge[10000];
int list[10000];

int main() {
    scanf("%d %d", &n, &m);
    while (m --) {
        int a, b;
        scanf("%d %d", &a, &b);
        edge[a].push_back(b);
        edge[b].push_back(a);
    }
    scanf("%d", &k);
    while (k --) {
        set<int> color_s;
        bool isColoring = true;
        for (int i = 0; i < n; i ++) {
            scanf("%d", &list[i]);
            color_s.insert(list[i]);
        }
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < edge[i].size(); j ++) {
                if (list[i] == list[edge[i][j]]) {
                    isColoring = false;
                    break;
                }
            }
            if (!isColoring) {
                break;
            }
        }
        if (isColoring) {
            printf("%d-coloring\n", color_s.size());
        }
        else {
            printf("No\n");
        }
    }
    return 0;
}

```

