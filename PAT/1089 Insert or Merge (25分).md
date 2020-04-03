# [1089 Insert or Merge (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805377432928256)

- 给定序列1和序列2，判断序列2是序列1的插入排序还是归并排序
- 插入排序的特征：前n个元素有序，后m个元素与原序列相同
- 执行插入操作时要注意元素交换的顺序
- 注意边界

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n;
int one[105], two[105];

void merge() {
    int len = 1;
    bool found = false;
    while (1) { // 找到现在是归并排序的第几步
        for (int i = 0; i < n; i += len) {
            for (int j = i; j < min(n - 2, i + len - 1); j ++) {
                if (two[j] > two[j + 1]) {
                    found = true;
                    break;
                }
            }
            if (found) {
                break;
            }
        }
        if (found) {
            break;
        }
        len *= 2;
    }
    int pos = 0;
    while (pos <= n) { // 归并
        sort(two + pos, two + min(n, pos + len));
        pos += len;
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &one[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &two[i]);
    }
    int pos = 0;
    while (pos < n - 1 && two[pos] <= two[pos + 1]) {
        pos ++;
    }
    bool isI = true;
    for (int i = pos + 1; i < n; i ++) { // 此时pos+1的位置即下一个
        if (one[i] != two[i]) {
            isI = false;
            break;
        }
    }
    if (isI) {
        printf("Insertion Sort\n");
        int tmp = two[pos + 1];
        for (int i = 0; i < n; i ++) {
            if (tmp < two[i]) {
                for (int j = pos + 1; j > i; j --) {
                    two[j] = two[j - 1];
                }
                two[i] = tmp;
                break;
            }
        }
    }
    else {
        printf("Merge Sort\n");
        merge();
    }
    for (int i = 0; i < n; i ++) {
        if (i != 0) {
            printf(" ");
        }
        printf("%d", two[i]);
    }
    printf("\n");
    return 0;
}

```



