# [1044 Shopping in Mars (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805439202443264)

- 直接暴力遍历超时了……嗯
- 参考[柳婼](https://www.liuchuo.net/archives/2939)的思路使用二分查找过了，这里学习到使用二分查找大于等于的最小值的方法

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n, target;
int list[100005];
int sum[100005];
int res[100005], tail[100005];
int minn;
const int inf = 999999999;

int main () {
    scanf("%d %d", &n, &target);
    sum[0] = 0;
    for (int i = 1; i <= n; i ++) {
        scanf("%d", &list[i]);
        sum[i] = sum[i - 1] + list[i]; // 输入时就把sum计算好，i-j的和即sum[j]-sum[i-1]
    }
    minn = inf;
    for (int i = 1; i <= n; i ++) {
        int base = i, top = n;
        while (base < top) {
            int mid = (base + top) / 2;
            if (sum[mid] - sum[i - 1] >= target) { // 最后的top是大于等于target的最小值
                top = mid;
            }
            else {
                base = mid + 1;
            }
        }
        if (sum[top] - sum[i - 1] >= target) { // 由于list最后几个值可能小于target，需要再判断一下
            res[i] = sum[top] - sum[i - 1];
            tail[i] = top;
            if (res[i] < minn) { // 更新大于target的最小值
                minn = res[i];
            }
        }
        else {
            res[i] = inf;
        }
    }
    for (int i = 1; i <= n; i ++) {
        if (res[i] == minn) {
            printf("%d-%d\n", i, tail[i]);
        }
    }
    return 0;
}

```

