# [1068 Find More Coins (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805402305150976)

- 直接`DFS`测试点6会超时
  - 加了个和判断，过了= =（测试点6是`No Solution`）

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n, m;
int list[10005];
bool mark[10005];
bool found = false;

void dfs(int idx, int sum) {
    if(found) {
        return;
    }
    mark[idx] = true;
    sum += list[idx];
    if (sum == m) {
        found = true;
        bool first = true;
        for (int i = 0; i < n; i ++) {
            if (mark[i] == true) {
                if (!first) {
                    printf(" ");
                }
                printf("%d", list[i]);
                first = false;
            }
        }
    }
    else if (sum < m) {
        for (int i = 0; i < n; i ++) {
            if (mark[i] == false) {
                dfs(i, sum);
            }
        }
    }
    mark[idx] = false;
}

int main() {
    int sum = 0;
    scanf("%d %d", &n, &m);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
        sum += list[i];
    }
    if (sum < m) {
        printf("No Solution");
        return 0;
    }
    sort(list, list + n);
    for (int i = 0; i < n; i ++) {
        mark[i] = false;
    }
    for (int i = 0; i < n; i ++) {
        if (found) {
            break;
        }
        dfs(i, 0);
    }
    if (!found) {
        printf("No Solution");
    }
    return 0;
}


```



