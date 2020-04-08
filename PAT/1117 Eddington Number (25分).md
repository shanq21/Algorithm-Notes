# [1117 Eddington Number (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805354762715136)

- 求有`X`天跑步超过`X`公里的`X`的最大值
- 直接遍历计数也没超时，但总觉得不够优雅，遂参考了[柳婼的思路](https://www.liuchuo.net/archives/2478)

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n, list[100005];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
    }
    sort(list, list + n, greater<int>());
    int ans = n;
    for (int i = 0; i < n; i ++) {
        while (list[i] <= ans) {
            ans --;
        }
        if (i + 1 >= ans) {
            break;
        }
    }
    printf("%d", ans);
    return 0;
}

```

