# [1036 Boys vs Girls (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805453203030016)

- 简单的结构体排序

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Student {
    string name;
    string id;
    int grade;
};

int n;
vector<Student> boy, girl;

bool dsc (Student a, Student b) {
    return a.grade > b.grade;
}

bool asc (Student a, Student b) {
    return a.grade < b.grade;
}

int main () {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        char ch1[20], ch2[20], ch;
        int a;
        scanf("%s %c %s %d", ch1, &ch, ch2, &a);
        Student stu;
        stu.name = ch1;
        stu.id = ch2;
        stu.grade = a;
        if (ch == 'F') {
            girl.push_back(stu);
        }
        else if (ch == 'M') {
            boy.push_back(stu);
        }
        else {
            printf("error!");
        }
    }
    sort(girl.begin(), girl.end(), dsc);
    sort(boy.begin(), boy.end(), asc);
    if (girl.empty()) {
        printf("Absent\n");
    }
    else {
        printf("%s %s\n", girl[0].name.c_str(), girl[0].id.c_str());
    }
    if (boy.empty()) {
        printf("Absent\n");
    }
    else {
        printf("%s %s\n", boy[0].name.c_str(), boy[0].id.c_str());
    }
    if (girl.empty() || boy.empty()) {
        printf("NA\n");
    }
    else {
        printf("%d\n", girl[0].grade - boy[0].grade);
    }
    return 0;
}

```

