# [1007 Maximum Subsequence Sum (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805514284679168)

- 简单的DP
- $dp[i] = dp[i - 1] + list[i];$

### AC代码

```c++
#include <iostream>
using namespace std;

int n;
int list[10010];
int dp[10010];
int head[10010];
int ansi, ansj;
const int inf = 123123123;

int main () {
    scanf("%d", &n);
    bool isNegative = true;
    for (int i = 1; i <= n; i ++) {
        scanf("%d", &list[i]);
        if (list[i] >= 0) {
            isNegative = false;
        }
    }
    if (isNegative) {
        cout << 0 << " " << list[1] << " " << list[n] << endl;
        return 0;
    }
    for (int i = 1; i <= n; i ++) {
        dp[i] = list[i];
        head[i] = i;
    }
    dp[0] = -inf;
    ansj = 0;
    for (int i = 1; i <= n; i ++) {
        if (dp[i] < dp[i - 1] + list[i]) {
            dp[i] = dp[i - 1] + list[i];
            head[i] = head[i - 1];
        }
        if (dp[i] > dp[ansj]) {
            ansi = head[i];
            ansj = i;
        }
    }
    cout << dp[ansj] << " " << list[ansi] << " " << list[ansj] << endl;
    return 0;
}

```

