# [1101 Quick Sort (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805366343188480)

- 判断一个序列中能作为快排pivot的元素
- 记录到`i`为止，从左数的最大值，以及从右数的最小值，当`lmax[i]==rmin[i]`，则说明这个元素符合条件
- 第一次提交测试点2格式错误，原因是当符合条件的元素个数为`0`时，第二行没有输出空行

### AC代码

```c++
#include <iostream>
using namespace std;

int n;
int list[100005];
int lmax[100005], rmin[100005];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
        if (i == 0) {
            lmax[i] = list[i];
        }
        else {
            lmax[i] = max(list[i], lmax[i - 1]);
        }
    }
    int cnt = 0;
    for (int i = n - 1; i >= 0; i --) {
        if (i == n - 1) {
            rmin[i] = list[i];
        }
        else {
            rmin[i] = min(list[i], rmin[i + 1]);
        }
        if (lmax[i] == rmin[i]) {
            cnt ++;
        }
    }
    printf("%d\n", cnt);
    bool first = true;
    for (int i = 0; i < n; i ++) {
        if (lmax[i] == rmin[i]) {
            if (!first) {
                printf(" ");
            }
            printf("%d", list[i]);
            first = false;
        }
    }
    printf("\n");
    return 0;
}

```

