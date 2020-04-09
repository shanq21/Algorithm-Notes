# [1118 Birds in Forest (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805354108403712)

- 并查集，判断两只鸟是不是在同一棵树上

### AC代码

```c++
#include <iostream>

int n, ufind[10001];
int size = 0;
bool mark[10001];

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

int main() {
    for (int i = 1; i <= 10000; i ++) {
        ufind[i] = -1;
        mark[i] = false;
    }
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        int m, a;
        scanf("%d %d", &m, &a);
        if (mark[a] == false) {
            mark[a] = true;
            size ++;
        }
        a = findRoot(a);
        for (int i = 1; i < m; i ++) {
            int b;
            scanf("%d", &b);
            if (mark[b] == false) {
                mark[b] = true;
                size ++;
            }
            b = findRoot(b);
            if (a != b) {
                ufind[b] = a;
            }
        }
    }
    int cnt = 0;
    for (int i = 1; i <= size; i ++) {
        if (ufind[i] == -1) {
            cnt ++;
        }
    }
    printf("%d %d\n", cnt, size);
    int k;
    scanf("%d", &k);
    for (int i = 0; i < k; i ++) {
        int a, b;
        scanf("%d %d", &a, &b);
        a = findRoot(a);
        b = findRoot(b);
        if (a == b) {
            printf("Yes\n");
        }
        else {
            printf("No\n");
        }
    }
    return 0;
}

```

