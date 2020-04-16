# [1141 PAT Ranking of Institutions (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805344222429184)

- 结构体排序，用`map`把字符串映射为数组下标，输出时计算`rank`

### AC代码

```c++
#include <iostream>
#include <map>
#include <algorithm>
using namespace std;

struct School {
    string name;
    int sb, sa, st;
    int cnt;
    int tws;
};

int n;
School school[100000];
map<string, int> s_idx;

bool cmp(School &a, School &b) {
    return a.tws > b.tws || (a.tws == b.tws && a.cnt < b.cnt) || (a.tws == b.tws && a.cnt == b.cnt && a.name < b.name);
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++) {
        string s1, s2;
        int a;
        cin >> s1 >> a >> s2;
        for (int j = 0; j < s2.size(); j ++) { // 把大写字母转化为小写
            if (s2[j] >= 'A' && s2[j] <= 'Z') {
                s2[j] += 'a' - 'A';
            }
        }
        int idx;
        if (s_idx.find(s2) == s_idx.end()) {
            idx = s_idx.size();
            s_idx[s2] = idx;
            school[idx] = {s2, 0, 0, 0, 0, 0};
        }
        else {
            idx = s_idx[s2];
        }
        school[idx].cnt ++;
        if (s1[0] == 'B') {
            school[idx].sb += a;
        }
        else if (s1[0] == 'A') {
            school[idx].sa += a;
        }
        else if (s1[0] == 'T') {
            school[idx].st += a;
        }
    }
    int size = s_idx.size();
    for (int i = 0; i < size; i ++) {
        school[i].tws = school[i].sb / 1.5 + school[i].sa + school[i].st * 1.5;
    }
    sort(school, school + size, cmp);
    printf("%d\n", size);
    int rank = 0, pre = -1;
    for (int i = 0; i < size; i ++) {
        if (school[i].tws != pre) {
            rank = i + 1;
            pre = school[i].tws;
        }
        printf("%d %s %d %d\n", rank, school[i].name.c_str(), school[i].tws, school[i].cnt);
    }
    return 0;
}

```

