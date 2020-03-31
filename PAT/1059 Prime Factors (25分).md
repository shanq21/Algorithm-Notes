# [1059 Prime Factors (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805415005503488)

- 找出一个正整数的所有素数因子，使用素数筛法即可
- 注意输出格式，尤其是`n==1`（测试点3）和`n`为质数的情况（测试点4）

### AC代码

```c++
#include <iostream>
#include <cmath>

long int n;
bool prime[50000];
int p[50000];

int main() {
    scanf("%ld", &n);
    if (n == 1) {
        printf("1=1");
        return 0;
    }
    long int copy = n;
    int m = sqrt(n) + 1;
    for (int i = 2; i <= m; i ++) { // 初始化prime[i]
        prime[i] = true;
    }
    for (int i = 2; i <= m; i ++) {
        if (prime[i] == true) {
            for (int j = i * i; j <= m; j += i) {
                prime[j] = false;
            }
        }
    }
    for (int i = 2; i <= m; i ++) { // 初始化p[i]
        p[i] = 0;
    }
    for (int i = 2; i <= m; i ++) {
        if (prime[i] == true) {
            while (n % i == 0) {
                p[i] ++;
                n /= i;
            }
        }
        if (n == 1) {
            break;
        }
    }
    bool isGreater = false;
    if (n != 1) {
        isGreater = true;
    }
    printf("%ld=", copy);
    bool first = true;
    for (int i = 2; i <= m; i ++) {
        if (prime[i] == true && p[i] > 0) {
            if (!first) {
                printf("*");
            }
            first = false;
            printf("%d", i);
            if (p[i] > 1) {
                printf("^%d", p[i]);
            }
        }
    }
    if (isGreater) {
        if (!first) {
            printf("*");
        }
        printf("%ld", n);
    }
    return 0;
}

```

