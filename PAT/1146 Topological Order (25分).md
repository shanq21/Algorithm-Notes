# [1146 Topological Order (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343043829760)

- 判断是否拓扑排序
- 在输入时记录弧和点的入度，依次遍历查询序列，当入度为`0`时有效，与该点对应的弧头入度减一

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n, m, k;
vector<int> edge[1001];
int in[1001], tin[1001];
int list[1001];

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i ++) {
        in[i] = 0;
    }
    while (m --) {
        int a, b;
        cin >> a >> b;
        edge[a].push_back(b);
        in[b] ++;
    }
    cin >> k;
    bool first = true;
    for (int i = 0; i < k; i ++) {
        bool isTopo = true;
        for (int j = 1; j <= n; j ++) {
            tin[j] = in[j];
        }
        for (int j = 0; j < n; j ++) {
            cin >> list[j];
        }
        for (int j = 0; j < n; j ++) {
            int a = list[j];
            if (tin[a] != 0) {
                isTopo = false;
                break;
            }
            for (int l = 0; l < edge[a].size(); l ++) {
                int b = edge[a][l];
                tin[b] --;
            }
        }
        if (!isTopo) {
            if (!first) {
                printf(" ");
            }
            printf("%d", i);
            first = false;
        }
    }
    return 0;
}

```

