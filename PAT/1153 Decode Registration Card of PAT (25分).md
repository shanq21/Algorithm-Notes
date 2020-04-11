# [1153 Decode Registration Card of PAT (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/1071785190929788928)

- 排序题，根据编码上的信息建立信息库，根据查询类型输出结果，主要考查数据结构的设计
- 测试点1和4死活过不去……
  - 过了，原因是没有判断`map`中是否存在相应的`date`就获取了`idx`，导致不存在`date`时`idx`始终为`0`，以后用`map`一定要记得判断是否存在

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <map>
using namespace std;

struct Stu {
    char level;
    int site;
    int date;
    int score;
    string s;
};

int n, m;
Stu stu[10000];
int nt[1000];
bool mark[1000];
vector<int> date_site[10000];
map<int, int> date_idx;

bool cmp(Stu &a, Stu &b) {
    if (a.score > b.score || (a.score == b.score && a.s < b.s)) {
        return true;
    }
    else {
        return false;
    }
}

bool cmp2(int &a, int &b) {
    if (nt[a] > nt[b] || (nt[a] == nt[b] && a < b)) {
        return true;
    }
    else {
        return false;
    }
}

int main() {
    cin >> n >> m;
    for (int i = 0; i < n; i ++) {
        string s;
        int a;
        cin >> s >> a;
        char l = s[0];
        int site = stoi(s.substr(1, 3));
        int date = stoi(s.substr(4, 6));
        stu[i].level = l;
        stu[i].site = site;
        stu[i].date = date;
        stu[i].score = a;
        stu[i].s = s;
        int idx;
        if (date_idx.find(date) != date_idx.end()) {
            idx = date_idx[date];
        }
        else {
            idx = date_idx.size();
            date_idx[date] = idx;
        }
        date_site[idx].push_back(site);
    }
    sort(stu, stu + n, cmp);
    for (int i = 1; i <= m; i ++) {
        int type;
        string s;
        cin >> type >> s;
        printf("Case %d: %d %s\n", i, type, s.c_str());
        bool found = false;
        if (type == 1) {
            char l = s[0];
            for (int j = 0; j < n; j ++) {
                if (stu[j].level == l) {
                    printf("%s %d\n", stu[j].s.c_str(), stu[j].score);
                    found = true;
                }
            }
        }
        else if (type == 2) {
            int site = stoi(s);
            int cnt = 0, sum = 0;
            for (int j = 0; j < n; j ++) {
                if (stu[j].site == site) {
                    cnt ++;
                    sum += stu[j].score;
                }
            }
            if (cnt > 0) {
                printf("%d %d\n", cnt, sum);
                found = true;
            }
        }
        else if (type == 3) {
            int date = stoi(s);
            if (date_idx.find(date) != date_idx.end()) {
                int idx = date_idx[date];
                for (int j = 0; j < date_site[idx].size(); j ++) { // 同一个site可能在不同的date，只统计当天的site数量
                    int site = date_site[idx][j];
                    mark[site] = false;
                    nt[site] = 0; // 计数器清零
                }
                for (int j = 0; j < date_site[idx].size(); j ++) {
                    int site = date_site[idx][j];
                    nt[site] ++;
                }
                sort(date_site[idx].begin(), date_site[idx].end(), cmp2);
                for (int j = 0; j < date_site[idx].size(); j ++) {
                    int site = date_site[idx][j];
                    if (mark[site] == false) {
                        printf("%d %d\n", site, nt[site]);
                        mark[site] = true;
                    }
                }
                found = true;
            }
        }
        if (!found) {
            printf("NA\n");
        }
    }
    return 0;
}

```

