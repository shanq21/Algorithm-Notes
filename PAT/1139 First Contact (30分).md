# [1139 First Contact (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805344776077312)

- 如果A要追求D，必须通过A的朋友B、B的朋友C、C的朋友D这样一条路径
- 使用邻接矩阵存储边，把A的同性朋友、B的同性朋友分别找出来，判断他们之间是否是朋友
- 注意排除A即是D的朋友的情况，以及序号为0000和-0000的人

### AC代码

```c++
#include <iostream>
#include <map>
#include <vector>
#include <algorithm>
using namespace std;

int n, m, k;
bool edge[300][300];
map<int, int> midx;
int mnum[300];

int getidx(int x) {
    if (midx.find(x) == midx.end()) {
        int idx = midx.size();
        midx[x] = idx;
        mnum[idx] = x;
        return idx;
    }
    else {
        return midx[x];
    }
}

bool cmp(pair<int, int> a, pair<int, int> b) {
    return a.first < b.first || (a.first == b.first && a.second < b.second);
}

int main() {
    scanf("%d %d", &n, &m);
    for (int i = 0; i < n; i ++) {
        for (int j = 0; j < n; j ++) {
            edge[i][j] = false;
        }
    }
    while (m --) {
        string sa, sb;
        cin >> sa >> sb;
        int a = stoi(sa), b = stoi(sb);
        if (a == 0) {
            a = sa[0] == '-' ? -10000 : 10000;
        }
        if (b == 0) {
            b = sb[0] == '-' ? -10000 : 10000;
        }
        a = getidx(a);
        b = getidx(b);
        edge[a][b] = edge[b][a] = true;
    }
    scanf("%d", &k);
    while (k --) {
        int a, b;
        scanf("%d %d", &a, &b);
        a = getidx(a);
        b = getidx(b);
        vector<int> va, vb;
        for (int i = 0; i < n; i ++) {
            if (edge[a][i] == true && i != b && mnum[a] * mnum[i] > 0) {
                va.push_back(i);
            }
        }
        for (int i = 0; i < n; i ++) {
            if (edge[b][i] == true && i != a && mnum[b] * mnum[i] > 0) {
                vb.push_back(i);
            }
        }
        vector<pair<int, int>> ans;
        for (int i = 0; i < va.size(); i ++) {
            for (int j = 0; j < vb.size(); j ++) {
                if (edge[va[i]][vb[j]] == true) {
                    int x = abs(mnum[va[i]]), y = abs(mnum[vb[j]]);
                    if (x == 10000) {
                        x = 0;
                    }
                    if (y == 10000) {
                        y = 0;
                    }
                    ans.push_back({x, y});
                }
            }
        }
        sort(ans.begin(), ans.end(), cmp);
        printf("%d\n", ans.size());
        for (int i = 0; i < ans.size(); i ++) {
            printf("%04d %04d\n", ans[i].first, ans[i].second);
        }
    }
    return 0;
}

```

