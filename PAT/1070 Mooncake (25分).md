# [1070 Mooncake (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805399578853376)

- 最简单的贪心算法，但是要注意所有数据都可能是浮点数

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Mooncake {
    double num;
    double rate;
};

int n;
double d;
Mooncake mc[1005];

bool cmp(Mooncake a, Mooncake b) {
    return a.rate > b.rate;
}

int main() {
    scanf("%d %lf", &n, &d);
    for (int i = 0; i < n; i ++) {
        scanf("%lf", &mc[i].num);
    }
    for (int i = 0; i < n; i ++) {
        double p;
        scanf("%lf", &p);
        if (mc[i].num == 0) {
            mc[i].rate = 123123123.0;
        }
        else {
            mc[i].rate = p / mc[i].num;
        }
    }
    sort(mc, mc + n, cmp);
    double profit = 0.0;
    for (int i = 0; i < n; i ++) {
        if (d > mc[i].num) {
            profit += mc[i].num * mc[i].rate;
            d -= mc[i].num;
        }
        else if (d <= mc[i].num) {
            profit += d * mc[i].rate;
            d = 0;
            break;
        }
    }
    printf("%.2f", profit);
    return 0;
}

```

