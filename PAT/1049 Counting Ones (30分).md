# [1049 Counting Ones (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805430595731456)

- 简单粗暴遍历会超时，待优化

### 非AC代码

- 测试点4、6超时

```c++
#include <iostream>

int n;

int cntone (int a) {
    int cnt = 0;
    while (a != 0) {
        if (a % 10 == 1) {
            cnt ++;
        }
        a /= 10;
    }
    return cnt;
}

int main () {
    scanf("%d", &n);
    int ans = 0;
    for (int i = 1; i <= n; i ++) {
        ans += cntone(i);
    }
    printf("%d", ans);
    return 0;
}

```

