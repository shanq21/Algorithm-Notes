# [1028 List Sorting (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805468327690240)

- 简单的结构体排序，一开始数组开小了段错误= =

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Student {
    int id;
    string name;
    int score;
};

int n, c;
Student stu[100001];

bool cmp1 (Student a, Student b) {
    return a.id < b.id;
}

bool cmp2 (Student a, Student b) {
    if (a.name < b.name) {
        return true;
    }
    else if (a.name == b.name && a.id < b.id) {
        return true;
    }
    else {
        return false;
    }
}

bool cmp3 (Student a, Student b) {
    if (a.score < b.score) {
        return true;
    }
    else if (a.score == b.score && a.id < b.id) {
        return true;
    }
    else {
        return false;
    }
}

int main () {
    scanf("%d %d", &n, &c);
    for (int i = 0; i < n; i ++) {
        char tmps[10];
        scanf("%d %s %d", &stu[i].id, tmps, &stu[i].score);
        stu[i].name = tmps;
    }
    if (c == 1) {
        sort(stu, stu + n, cmp1);
    }
    else if (c == 2) {
        sort(stu, stu + n, cmp2);
    }
    else if (c == 3) {
        sort(stu, stu + n, cmp3);
    }
    for (int i = 0; i < n; i ++) {
        printf("%06d %s %d\n", stu[i].id, stu[i].name.c_str(), stu[i].score);
    }
    return 0;
}

```

