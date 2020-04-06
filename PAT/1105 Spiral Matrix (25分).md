# [1105 Spiral Matrix (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805363117768704)

- 给定若干整数，要求从大到小顺时针输出
- 不要直接输出，先把结果保存在`ans`中，最后再输出
- 用一个`flag`变量控制下次输出的方向，当都不符合条件时改变方向（自认为`flag`和`while`那块写得很漂亮！23333）

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n, rn, cn;
int list[10005];
int ans[10005][10005]; // 按输出格式保存数据
bool mark[10005][10005]; // 标记该位置是否已有数据

bool cmp(int a, int b) {
    return a > b;
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
    }
    for (int i = 1; i <= n / i; i ++) { // 找到符合条件的行数和列数
        if (n % i == 0) {
            cn = i;
            rn = n / i;
        }
    }
    sort(list, list + n, cmp); // 按从大到小排列
    for (int i = 0; i < rn; i ++) { // 初始化标记数组
        for (int j = 0; j < cn; j ++) {
            mark[i][j] = false;
        }
    }
    int r = 0, c = 0; // 位置下标
    int flag = 0; // 表示移动的方向，0: right, 1: down, 2: left, 3: up，初始方向是右
    for (int i = 0; i < n; i ++) { // 按从大到小依次赋值
        ans[r][c] = list[i];
        mark[r][c] = true;
        if (i == n - 1) { // 提前退出，否则会陷入死循环
            break;
        }
        while (1) {
            if (flag == 0 && c + 1 < cn && !mark[r][c + 1]) { // 如果能走右，就走右
                c ++;
                break;
            }
            else if (flag == 1 && r + 1 < rn && !mark[r + 1][c]) { // 如果能走下，就走下
                r ++;
                break;
            }
            else if (flag == 2 && c - 1 >= 0 && !mark[r][c - 1]) { // 如果能走左，就走左
                c --;
                break;
            }
            else if (flag == 3 && r - 1 >= 0 && !mark[r - 1][c]) { // 如果能走上，就走上
                r --;
                break;
            }
            flag = (flag + 1) % 4; // 如果都不能，说明该换方向了
        }
    }
    for (int i = 0; i < rn; i ++) {
        for (int j = 0; j < cn; j ++) {
            if (j != 0) {
                printf(" ");
            }
            printf("%d", ans[i][j]);
        }
        printf("\n");
    }
    return 0;
}

```

