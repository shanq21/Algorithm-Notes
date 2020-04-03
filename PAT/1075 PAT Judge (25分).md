# [1075 PAT Judge (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805393241260032)

- 典型的结构体排序，要注意区分没提交和没通过两种状态，如果是提交过但没通过编译器的，在最后输出时分数为`0`

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct User {
    int id;
    int score[10];
    int perfect;
    int total;
};

int n, k, m;
int p[10];
User user[10005];

bool cmp(User a, User b) {
    if (a.total > b.total) {
        return true;
    }
    else if (a.total == b.total) {
        if (a.perfect > b.perfect || (a.perfect == b.perfect && a.id < b.id)) {
            return true;
        }
        else {
            return false;
        }
    }
    else {
        return false;
    }
}

int main() {
    scanf("%d %d %d", &n, &k, &m);
    for (int i = 1; i <= k; i ++) {
        scanf("%d", &p[i]);
    }
    for (int i = 1; i <= n; i ++) {
        for (int j = 1; j <= k; j ++) {
            user[i].score[j] = -2;
        }
        user[i].perfect = 0;
        user[i].total = -1;
    }
    for (int i = 0; i < m; i ++) {
        int uid, pid, score;
        scanf("%d %d %d", &uid, &pid, &score);
        if (score > user[uid].score[pid]) {
            user[uid].id = uid;
            if (score >= 0) {
                if (user[uid].total == -1) {
                    user[uid].total = 0;
                }
                if (user[uid].score[pid] < 0) {
                    user[uid].total += score;
                }
                else {
                    user[uid].total += score - user[uid].score[pid];
                }
            }
            user[uid].score[pid] = score;
            if (score == p[pid]) {
                user[uid].perfect ++;
            }
        }
    }
    sort(user + 1, user + 1 + n, cmp);
    int pre = -1, rank = 0;
    for (int i = 1; i <= n; i ++) {
        if (user[i].total == -1) {
            break;
        }
        if (user[i].total != pre) {
            pre = user[i].total;
            rank = i;
        }
        printf("%d %05d %d", rank, user[i].id, user[i].total);
        for (int j = 1; j <= k; j ++) {
            if (user[i].score[j] == -2) {
                printf(" -");
            }
            else if (user[i].score[j] == -1) {
                printf(" 0");
            }
            else {
                printf(" %d", user[i].score[j]);
            }
        }
        printf("\n");
    }
    return 0;
}

```

