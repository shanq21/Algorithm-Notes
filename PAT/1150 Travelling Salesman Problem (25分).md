# [1150 Travelling Salesman Problem (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/1038430013544464384)

- 判断给定路径是否是经过所有点的回路、简单回路、非回路或非经过所有点，记录总距离最短的回路

### AC代码

```c++
#include <iostream>
#include <vector>
#include <set>
using namespace std;

const int inf = 123123123;
int n, m, k;
int edge[201][201];

int main() {
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= n; i ++) {
        for (int j = 1; j <= n; j ++) {
            edge[i][j] = inf;
        }
    }
    int mind = inf, mini = -1;
    while (m --) {
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        edge[a][b] = edge[b][a] = c;
    }
    scanf("%d", &k);
    for (int i = 1; i <= k; i ++) {
        printf("Path %d: ", i);
        int l;
        scanf("%d", &l);
        vector<int> tv;
        set<int> s;
        for (int j = 0; j < l; j ++) {
            int a;
            scanf("%d", &a);
            tv.push_back(a);
            s.insert(a);
        }
        int sum = 0;
        for (int j = 0; j < l - 1; j ++) {
            sum += edge[tv[j]][tv[j + 1]];
        }
        if (sum > inf) {
            printf("NA (Not a TS cycle)\n");
        }
        else {
            printf("%d ", sum);
            if (s.size() == n && tv[0] == tv[l - 1]) {
                if (l == n + 1) {
                    printf("(TS simple cycle)\n");
                }
                else {
                    printf("(TS cycle)\n");
                }
                if (sum < mind) {
                    mind = sum;
                    mini = i;
                }
            }
            else {
                printf("(Not a TS cycle)\n");
            }
        }
    }
    printf("Shortest Dist(%d) = %d\n", mini, mind);
    return 0;
}

```

