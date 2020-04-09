# [1126 Eulerian Path (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805349851185152)

- 根据度的奇偶性个数判断是否欧拉回路，注意要判断连通图

### AC代码

```c++
#include <iostream>

int n, m;
int degree[501], ufind[501];
int flag = 0;

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
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= n; i ++) {
        degree[i] = 0;
        ufind[i] = -1;
    }
    while (m --) {
        int a, b;
        scanf("%d %d", &a, &b);
        degree[a] ++;
        degree[b] ++;
        a = findRoot(a);
        b = findRoot(b);
        if (a != b) {
            ufind[a] = b;
        }
    }
    int cnt = 0;
    for (int i = 1; i <= n; i ++) {
        if (i != 1) {
            printf(" ");
        }
        printf("%d", degree[i]);
        if (degree[i] % 2 == 1) {
            cnt ++;
            flag = 1;
        }
    }
    printf("\n");
    if (flag == 1 && cnt != 2) {
        flag = 2;
    }
    cnt = 0;
    for (int i = 1; i <= n; i ++) {
        if (ufind[i] == -1) {
            cnt ++;
        }
    }
    if (cnt != 1) {
        flag = 2;
    }
    if (flag == 0) {
        printf("Eulerian");
    }
    else if (flag == 1) {
        printf("Semi-Eulerian");
    }
    else {
        printf("Non-Eulerian");
    }
    return 0;
}

```

