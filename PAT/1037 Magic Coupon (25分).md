# [1037 Magic Coupon (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805451374313472)

- 简单的贪心算法？
- 一开始段错误以为是判断条件写不对导致越界，最后发现是数组开得不够大= =

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int nc, np;
int c[100010], p[100010];

int main () {
    scanf("%d", &nc);
    for (int i = 0; i < nc; i ++) {
        int a;
        scanf("%d", &a);
        c[i] = a;
    }
    scanf("%d", &np);
    for (int i = 0; i < np; i ++) {
        int a;
        scanf("%d", &a);
        p[i] = a;
    }
    sort(c, c + nc);
    sort(p, p + np);
    int ans = 0;
    int pos = 0;
    for (int i = 0; i < nc && c[i] < 0; i ++) {
        if (pos >= np || p[pos] > 0) {
            break;
        }
        ans += c[i] * p[pos ++];
    }
    int rpos = np - 1;
    for (int i = nc - 1; i >= 0 && c[i] > 0; i --) {
        if (rpos < 0 || p[rpos] < 0) {
            break;
        }
        ans += c[i] * p[rpos --];
    }
    printf("%d", ans);
    return 0;
}

```

