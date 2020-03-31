# [1063 Set Similarity (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805409175420928)

- 计算两个集合的重合率，在输入时就把集合处理为有序不重复，`nt = set1.size + set2.size - nc;`

### AC代码

```c++
#include <iostream>
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

int n, k;
vector<int> sets[55];

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++) {
        int m;
        scanf("%d", &m);
        unordered_set<int> hash;
        for (int j = 0; j < m; j ++) {
            int a;
            scanf("%d", &a);
            if (hash.find(a) == hash.end()) {
                hash.insert(a);
                sets[i].push_back(a);
            }
        }
        sort(sets[i].begin(), sets[i].end());
    }
    scanf("%d", &k);
    for (int i = 0; i < k; i ++) {
        int u, v;
        scanf("%d %d", &u, &v);
        int nc = 0, nt = 0;
        int pos = 0;
        for (int i = 0; i < sets[u].size(); i ++) { // 计算nc
            for (pos = pos; pos < sets[v].size() && sets[v][pos] <= sets[u][i]; pos ++) {
                if (sets[v][pos] == sets[u][i]) {
                    nc ++;
                    pos ++;
                    break;
                }
            }
        }
        nt = sets[u].size() + sets[v].size() - nc;
        printf("%.1f%%\n", double(nc) / double(nt) * 100.0);
    }
    return 0;
}

```

