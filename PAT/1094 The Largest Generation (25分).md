# [1094 The Largest Generation (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805372601090048)

- `BFS`，求家谱树中人数最多的一代，简单

### AC代码

```c++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int n, m;
vector<int> node[100];

int main() {
    scanf("%d %d", &n, &m);
    for (int i = 0; i < m; i ++) {
        int id, k;
        scanf("%d %d", &id, &k);
        for (int i = 0; i < k; i ++) {
            int cid;
            scanf("%d", &cid);
            node[id].push_back(cid);
        }
    }
    queue<int> q;
    q.push(1);
    int step = 0;
    int maxn = 0, gen = 1;
    while (!q.empty()) {
        step ++;
        int size = q.size();
        if (size > maxn) {
            maxn = size;
            gen = step;
        }
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            for (int j = 0; j < node[cur].size(); j ++) {
                q.push(node[cur][j]);
            }
        }
    }
    printf("%d %d", maxn, gen);
    return 0;
}

```

