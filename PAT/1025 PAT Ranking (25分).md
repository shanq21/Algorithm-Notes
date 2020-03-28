# [1025 PAT Ranking (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805474338127872)

- 简单的结构体排序，注意同分数`rank`的处理
- 一个地区的数据录入完毕时，先处理这部分数据，获取地区排名

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct P {
    long long id;
    int score;
    int frank;
    int location;
    int lrank;
};

int n;
P people[30001];
int size; // 参赛者总人数

bool cmp (P a, P b) {
    if ((a.score > b.score) || (a.score == b.score && a.id < b.id)) {
        return true;
    }
    else {
        return false;
    }
}

int main () {
    scanf("%d", &n);
    size = 0;
    for (int i = 0; i < n; i ++) {
        int k;
        scanf("%d", &k);
        int first = size; // 记录该地区第一个参赛者的编号
        for (int j = 0; j < k; j ++) {
            scanf("%lld %d", &people[size].id, &people[size].score);
            people[size].location = i + 1;
            size ++;
        }
        sort(people + first, people + size, cmp);
        int rank = 0;
        int pre = -1;
        for (int j = first; j < size; j ++) {
            if (people[j].score != pre) {
                pre = people[j].score;
                rank = j - first + 1;
            }
            people[j].lrank = rank;
        }
    }
    sort(people, people + size, cmp);
    int rank = 0;
    int pre = -1;
    for (int i = 0; i < size; i ++) {
        if (people[i].score != pre) {
            pre = people[i].score;
            rank = i + 1;
        }
        people[i].frank = rank;
    }
    printf("%d\n", size);
    for (int i = 0; i < size; i ++) {
        printf("%013lld %d %d %d\n", people[i].id, people[i].frank, people[i].location, people[i].lrank);
    }
    return 0;
}

```

