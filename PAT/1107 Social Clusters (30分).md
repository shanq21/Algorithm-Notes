# [1107 Social Clusters (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805361586847744)

- 设置爱好的并查集，检查每个人是否属于同一并查集

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int n;
int people[1005], ufind[1005];
bool mark[1005];
vector<int> ans;

int findRoot(int x) {
    if (ufind[x] == -1) {
        return x;
    }
    else {
        int tmp = findRoot(ufind[x]);
        ufind[x] = tmp;
        return tmp;
    }
}

bool cmp(int a, int b) {
    return a > b;
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= 1000; i ++) {
        ufind[i] = -1;
    }
    for (int i = 1; i <= n; i ++) {
        int m, h1;
        scanf("%d: %d", &m, &h1);
        people[i] = h1; // 保存第一个爱好
        for (int i = 2; i <= m; i ++) {
            int h2;
            scanf("%d", &h2);
            int a = findRoot(h1);
            int b = findRoot(h2);
            if (a != b) {
                ufind[a] = b;
            }
        }
    }
    for (int i = 1; i <= n; i ++) {
        mark[i] = false;
    }
    for (int i = 1; i <= n; i ++) {
        if (!mark[i]) {
            int root = findRoot(people[i]);
            int cnt = 0;
            for (int j = i; j <= n; j ++) {
                if (root == findRoot(people[j])) {
                    cnt ++;
                    mark[j] = true;
                }
            }
            ans.push_back(cnt);
        }
    }
    sort(ans.begin(), ans.end(), cmp);
    printf("%d\n", ans.size());
    for (int i = 0; i < ans.size(); i ++) {
        if (i != 0) {
            printf(" ");
        }
        printf("%d", ans[i]);
    }
    return 0;
}

```

