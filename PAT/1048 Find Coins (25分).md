# [1048 Find Coins (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805432256675840)

- 很简单的`hash`题，但是不知道为什么一开始的写法总是导致测试点3答案错误……很诡异
- 遂采用[柳婼](https://www.liuchuo.net/archives/2142)的写法，过了

### AC代码

```c++
#include <iostream>
using namespace std;

int n, money;
int coin[1005];

int main () {
    scanf("%d %d", &n, &money);
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        coin[a] ++;
    }
    for (int i = 0; i < money; i ++) {
        if (coin[i] > 0) {
            coin[i] --;
            if (coin[money - i] > 0) {
                printf("%d %d", i, money - i);
                return 0;
            }
        }
    }
    printf("No Solution");
    return 0;
}

```

