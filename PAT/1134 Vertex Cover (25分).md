# [1134 Vertex Cover (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805346428633088)

- 判断给定的点集，是否包含每条边的至少一个顶点
- 用`unordered_set`保存点集，遍历边，判断两个顶点是否至少有一个在点集中

### AC代码

```c++
#include <iostream>
#include <unordered_set>
using namespace std;

int n, m;
pair<int, int> edge[10000];

int main() {
    scanf("%d %d", &n, &m);
    for (int i = 0; i < m; i ++) {
        int a, b;
        scanf("%d %d", &a, &b);
        edge[i] = {a, b};
    }
    int k;
    scanf("%d", &k);
    while (k --) {
        int l;
        scanf("%d", &l);
        unordered_set<int> us;
        for (int i = 0; i < l; i ++) {
            int a;
            scanf("%d", &a);
            us.insert(a);
        }
        bool isCover = true;
        for (int i = 0; i < m; i ++) {
            int a = edge[i].first, b = edge[i].second;
            if (us.find(a) == us.end() && us.find(b) == us.end()) {
                isCover = false;
                break;
            }
        }
        printf("%s\n", isCover ? "Yes" : "No");
    }
    return 0;
}

```

