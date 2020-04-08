# [1113 Integer Set Partition (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805357258326016)

- 把一堆整数分成两部分，使它们的数量之差最小，和之差最大
- 很简单，排序后加加加就行，好久没写过这么短的代码了……

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n;
int list[100005];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
    }
    sort(list, list + n);
    int n1 = n / 2;
    int s1 = 0, s2 = 0;
    for (int i = 0; i < n1; i ++) {
        s1 += list[i];
    }
    for (int i = n1; i < n; i ++) {
        s2 += list[i];
    }
    printf("%d %d", n % 2, s2 - s1);
    return 0;
}

```

