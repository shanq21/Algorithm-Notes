# [1085 Perfect Sequence (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805381845336064)

- 二分查找，找出小于等于的最大值
- 数据要用`long long`，否则测试点5无法通过

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n, p;
long long list[100005];

int main() {
    scanf("%d %d", &n, &p);
    for (int i = 0; i < n; i ++) {
        scanf("%lld", &list[i]);
    }
    sort(list, list + n);
    int maxn = 1;
    for (int i = 0; i < n; i ++) {
        long long target = list[i] * p;
        int base = i, top = n - 1;
        while (base < top) {
            int mid = (base + top + 1) / 2;
            if (list[mid] <= target) {
                base = mid;
            }
            else {
                top = mid - 1;
            }
        }
        maxn = max(maxn, base - i + 1);
        if (base == n - 1) {
            break;
        }
    }
    printf("%d", maxn);
    return 0;
}

```

