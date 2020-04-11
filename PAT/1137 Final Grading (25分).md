# [1137 Final Grading (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805345401028608)

- 排序题，信息筛选
- 注意计算分数时要四舍五入

### AC代码

```c++
#include <iostream>
#include <map>
#include <vector>
#include <cmath>
#include <algorithm>
using namespace std;

struct Stu {
    string id;
    int hw, mid, fin, g;
};

int p, m, n;
Stu stu[10000];
map<string, int> s_idx;

bool cmp(Stu &a, Stu &b) {
    return a.g > b.g || (a.g == b.g && a.id < b.id);
}

int main() {
    scanf("%d %d %d", &p, &m, &n);
    while (p --) {
        string s;
        int a;
        cin >> s >> a;
        if (a < 200) {
            continue;
        }
        int idx;
        if (s_idx.find(s) == s_idx.end()) {
            idx = s_idx.size();
            s_idx[s] = idx;
        }
        else {
            idx = s_idx[s];
        }
        stu[idx].id = s;
        stu[idx].hw = a;
        stu[idx].mid = -1;
        stu[idx].fin = -1;
    }
    while (m --) {
        string s;
        int a;
        cin >> s >> a;
        if (s_idx.find(s) == s_idx.end()) {
            continue;
        }
        int idx = s_idx[s];
        stu[idx].mid = a;
    }
    while (n --) {
        string s;
        int a;
        cin >> s >> a;
        if (s_idx.find(s) == s_idx.end()) {
            continue;
        }
        int idx = s_idx[s];
        stu[idx].fin = a;
    }
    vector<Stu> ans;
    for (int i = 0; i < s_idx.size(); i ++) {
        if (stu[i].fin == -1) {
            continue;
        }
        int g = stu[i].mid > stu[i].fin ? round(0.4 * stu[i].mid + 0.6 * stu[i].fin) : stu[i].fin;
        if (g >= 60) {
            stu[i].g = g;
            ans.push_back(stu[i]);
        }
    }
    sort(ans.begin(), ans.end(), cmp);
    for (int i = 0; i < ans.size(); i ++) {
        printf("%s %d %d %d %d\n", ans[i].id.c_str(), ans[i].hw, ans[i].mid, ans[i].fin, ans[i].g);
    }
    return 0;
}

```

