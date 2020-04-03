# [1083 List Grades (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805383929905152)

- 结构体排序，区间输出

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Stu {
    string name;
    string id;
    int grade;
};

int n;
Stu stu[10005];
int base, top;

bool cmp(Stu a, Stu b) {
    return a.grade > b.grade;
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++) {
        cin >> stu[i].name >> stu[i].id >> stu[i].grade;
    }
    cin >> base >> top;
    sort(stu, stu + n, cmp);
    bool output = false, isNone = true;
    for (int i = 0; i < n; i ++) {
        if (stu[i].grade <= top) {
            output = true;
        }
        if (stu[i].grade < base) {
            output = false;
        }
        if (output == true) {
            cout << stu[i].name << " " << stu[i].id << endl;
            isNone = false;
        }
    }
    if (isNone) {
        cout << "NONE" << endl;
    }
    return 0;
}

```

