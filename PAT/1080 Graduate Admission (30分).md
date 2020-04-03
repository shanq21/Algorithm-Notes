# [1080 Graduate Admission (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805387268571136)

- 典型的结构体排序
- 可以用`fin = ge + gi`代替`fin = (ge + gi) / 2`，以及要注意保存的是`id`还是`i`……

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Student {
    int id;
    int rank;
    int ge;
    int gi;
    int fin;
    vector<int> apply;
};

int n, m, k;
int quota[105];
vector<int> school[105];
Student stu[40005];

bool cmp(Student a, Student b) {
    if (a.fin > b.fin) {
        return true;
    }
    else if (a.fin == b.fin && a.ge > b.ge) {
        return true;
    }
    else {
        return false;
    }
}

bool cmp2(int a, int b) {
    return stu[a].id < stu[b].id;
}

int main() {
    scanf("%d %d %d", &n, &m, &k);
    for (int i = 0; i < m; i ++) {
        scanf("%d", &quota[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d %d", &stu[i].ge, &stu[i].gi);
        stu[i].id = i;
        stu[i].fin = stu[i].ge + stu[i].gi;
        for (int j = 0; j < k; j ++) {
            int sid;
            scanf("%d", &sid);
            stu[i].apply.push_back(sid);
        }
    }
    sort(stu, stu + n, cmp);
    int preFin = -1, preGe = -1, rank = 0;
    for (int i = 0; i < n; i ++) {
        if (stu[i].fin != preFin || (stu[i].fin == preFin && stu[i].ge != preGe)) {
            rank ++;
            preFin = stu[i].fin;
            preGe = stu[i].ge;
        }
        stu[i].rank = rank;
        for (int j = 0; j < k; j ++) {
            int sid = stu[i].apply[j];
            int size = school[sid].size();
            if (size < quota[sid] || (size > 0 && stu[school[sid][size - 1]].rank == stu[i].rank)) {
                school[sid].push_back(i);
                break;
            }
        }
    }
    for (int i = 0; i < m; i ++) {
        sort(school[i].begin(), school[i].end(), cmp2);
        for (int j = 0; j < school[i].size(); j ++) {
            if (j != 0) {
                printf(" ");
            }
            printf("%d", stu[school[i][j]].id);
        }
        printf("\n");
    }
    return 0;
}

```

