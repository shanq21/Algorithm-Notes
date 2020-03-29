# [1045 Favorite Color Stripe (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805437411475456)

- 不算难的DP，自己写出来了（叉个腰！）
- 状态转移方程
  - $dp[i][j]=\max\{dp[i][j-1]+1,dp[i-1][j]\};list[j]==eva[i]$
  - $dp[i][j]=\max\{dp[i][j-1],dp[i-1][j]\};list[j]!=eva[i]$

### AC代码

```c++
#include <iostream>
using namespace std;

int n, m, l;
int eva[205];
int list[10005];
int dp[205][10005]; // dp[i][j]，进行到i顺序颜色、j长度时的结果

int main () {
    scanf("%d %d", &n, &m);
    for (int i = 1; i <= m; i ++) {
        scanf("%d", &eva[i]);
    }
    scanf("%d", &l);
    for (int i = 1; i <= l; i ++) {
        scanf("%d", &list[i]);
    }
    for (int i = 0; i <= m; i ++) {
        dp[i][0] = 0;
    }
    for (int i = 0; i <= l; i ++) {
        dp[0][i] = 0;
    }
    for (int i = 1; i <= m; i ++) {
        for (int j = 1; j <= l; j ++) {
            if (list[j] == eva[i]) {
                dp[i][j] = max(dp[i][j - 1] + 1, dp[i - 1][j]);
            }
            else {
                dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
            }
//            printf("dp[%d][%d] = %d\n", i, j, dp[i][j]);
        }
    }
    printf("%d", dp[m][l]);
    return 0;
}

```

