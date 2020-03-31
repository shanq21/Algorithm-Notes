# [1062 Talent and Virtue (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805410555346944)

- 结构体排序
- 算法简单，但是要求很烦，好讨厌这种题= =

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Person {
    int id;
    int v;
    int t;
    int total;
};

int n, l, h;
vector<Person> sage, nobleman, fool, small;

bool cmp(Person a, Person b) {
    if (a.total > b.total) {
        return true;
    }
    else if (a.total == b.total) {
        if (a.v > b.v || (a.v == b.v && a.id < b.id)) {
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

void func(vector<Person> vec) {
    sort(vec.begin(), vec.end(), cmp);
    for (auto it = vec.begin(); it != vec.end(); it ++) {
        printf("%08d %d %d\n", it->id, it->v, it->t);
    }
}

int main() {
    scanf("%d %d %d", &n, &l, &h);
    int cnt = 0;
    for (int i = 0; i < n; i ++) {
        int id, v, t;
        scanf("%d %d %d", &id, &v, &t);
        if (v < l || t < l) {
            continue;
        }
        cnt ++;
        Person p;
        p.id = id;
        p.v = v;
        p.t = t;
        p.total = v + t;
        if (v >= h && t >= h) {
            sage.push_back(p);
        }
        else if (v >= h && t < h) {
            nobleman.push_back(p);
        }
        else if (v < h && t < h && v >= t) {
            fool.push_back(p);
        }
        else {
            small.push_back(p);
        }
    }
    printf("%d\n", cnt);
    func(sage);
    func(nobleman);
    func(fool);
    func(small);
    return 0;
}

```

