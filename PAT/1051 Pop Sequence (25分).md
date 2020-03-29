# [1051 Pop Sequence (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805427332562944)

- 判断一个序列是不是可能的出栈序列
  - 在一个数之后，比这个数小的数必须降序排列，且个数不能超过栈的大小

### AC代码

```c++
#include <iostream>

int size, n, k;
int seq[1005];

int main () {
    scanf("%d %d %d", &size, &n, &k);
    for (int i = 0; i < k; i ++) {
        for (int j = 0; j < n; j ++) { // 输入序列数据
            scanf("%d", &seq[j]);
        }
        bool isStack = true;
        for (int j = 0; j < n; j ++) {
            if (!isStack) {
                break;
            }
            int maxn = seq[j], cnt = 1;
            for (int u = j + 1; u < n; u ++) { // 从下一个数开始判断
                if (seq[u] < seq[j]) { // 如果小于j，则必须降序排列
                    if (seq[u] > maxn) {
                        isStack = false;
                        break;
                    }
                    maxn = seq[u];
                    cnt ++;
                }
            }
            if (cnt > size) {
                isStack = false;
            }
        }
        if (isStack) {
            printf("YES\n");
        }
        else {
            printf("NO\n");
        }
    }
    return 0;
}

```

