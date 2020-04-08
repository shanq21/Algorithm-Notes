# [1114 Family Property (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805356599820288)

- 并查集，不算难，但写了好久而且好长……再也不想做这种题了orz

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct P {
    int minid;
    int newcnt;
    int sets;
    double area;
};

struct F {
    int minid;
    int people;
    int sets;
    double area, avgs, avga;
};

int n;
int list[1001];
P people[10000];
int ufind[10000];
vector<F> family;
bool mark[1001], mark2[10000];

int findRoot(int x) {
    if (ufind[x] == -1) {
        return x;
    }
    else {
        int tmp = findRoot(ufind[x]);
        ufind[x] = tmp;
        return tmp;
    }
}

bool cmp(F a, F b) {
    if (a.avga > b.avga) {
        return true;
    }
    else if (a.avga == b.avga && a.minid < b.minid) {
        return true;
    }
    else {
        return false;
    }
}

int main() {
    for (int i = 0; i < 10000; i ++) {
        mark2[i] = false;
        ufind[i] = -1;
    }
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        list[i] = a;
        int newcnt = 0, minid = a;
        if (mark2[a] == false) {
            newcnt ++;
            mark2[a] = true;
        }
        for (int j = 0; j < 2; j ++) {
            int b;
            scanf("%d", &b);
            if (b == -1) {
                continue;
            }
            if (mark2[b] == false) {
                newcnt ++;
                mark2[b] = true;
            }
            int u = findRoot(a);
            int v = findRoot(b);
            if (u != v) {
                ufind[v] = u;
            }
            minid = min(minid, b);
        }
        int m;
        scanf("%d", &m);
        for (int j = 0; j < m; j ++) {
            int b;
            scanf("%d", &b);
            if (mark2[b] == false) {
                newcnt ++;
                mark2[b] = true;
            }
            int u = findRoot(a);
            int v = findRoot(b);
            if (u != v) {
                ufind[v] = u;
            }
            minid = min(minid, b);
        }
        people[a].newcnt = newcnt;
        people[a].minid = minid;
        scanf("%d %lf", &people[a].sets, &people[a].area);
    }
    for (int i = 0; i < n; i ++) {
        mark[i] = false;
    }
    for (int i = 0; i < n; i ++) {
        if (mark[i] == false) {
            int root = findRoot(list[i]);
            mark[i] = true;
            F fa;
            fa.minid = people[list[i]].minid;
            fa.people = people[list[i]].newcnt;
            fa.sets = people[list[i]].sets;
            fa.area = people[list[i]].area;
            for (int j = i + 1; j < n; j ++) {
                if (root == findRoot(list[j])) {
                    mark[j] = true;
                    fa.minid = min(fa.minid, people[list[j]].minid);
                    fa.people += people[list[j]].newcnt;
                    fa.sets += people[list[j]].sets;
                    fa.area += people[list[j]].area;
                }
            }
            family.push_back(fa);
        }
    }
    for (int i = 0; i < family.size(); i ++) {
        family[i].avgs = double(family[i].sets) / family[i].people;
        family[i].avga = family[i].area / family[i].people;
    }
    sort(family.begin(), family.end(), cmp);
    printf("%d\n", family.size());
    for (int i = 0; i < family.size(); i ++) {
        printf("%04d %d %.3f %.3f\n", family[i].minid, family[i].people, family[i].avgs, family[i].avga);
    }
    return 0;
}

```

