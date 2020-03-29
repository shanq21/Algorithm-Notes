# [1055 The World's Richest (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805421066272768)

- 比较典型的结构体排序题，按要求做即可

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Person {
    string name;
    int age;
    int worth;
};

int n, m;
Person person[100005];

bool cmp(Person a, Person b) {
    if (a.worth > b.worth) {
        return true;
    }
    else if (a.worth == b.worth) {
        if (a.age < b.age) {
            return true;
        }
        else if (a.age == b.age && a.name < b.name) {
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
    scanf("%d %d", &n, &m);
    for (int i = 0; i < n; i ++) {
        char s[10];
        int a, w;
        scanf("%s %d %d", s, &a, &w);
        person[i].name = s;
        person[i].age = a;
        person[i].worth = w;
    }
    sort(person, person + n, cmp);
    for (int i = 0; i < m; i ++) {
        printf("Case #%d:\n", i + 1);
        int cnt, amin, amax;
        scanf("%d %d %d", &cnt, &amin, &amax);
        bool found = false;
        for (int j = 0; j < n; j ++) {
            if (cnt == 0) {
                break;
            }
            if (person[j].age >= amin && person[j].age <= amax) {
                found = true;
                printf("%s %d %d\n", person[j].name.c_str(), person[j].age, person[j].worth);
                cnt --;
            }
        }
        if (!found) {
            printf("None\n");
        }
    }
    return 0;
}

```

