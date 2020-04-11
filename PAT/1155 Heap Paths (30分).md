# [1155 Heap Paths (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/1071785408849047552)

- 判断大根堆、小根堆、非堆
- `DFS`，把从根节点到叶子节点的每条路保存在临时`vector`中，最后再判断
- 相似题：[1147 Heaps (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805342821531648)

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n;
int list[1000];
int flag = 0;

vector<int> tpath;
void dfs(int idx) {
    tpath.push_back(idx);
    if (2 * idx + 1 >= n) { // 如果没有孩子节点
        int pre = list[0];
        for (int i = 0; i < tpath.size(); i ++) {
            if (i != 0) {
                printf(" ");
            }
            int x = list[tpath[i]];
            printf("%d", x);
            if (i != 0) {
                if (x < pre) {
                    if (flag == 0) {
                        flag = 1;
                    }
                    else if (flag != 1) {
                        flag = 3;
                    }
                }
                else if (x > pre) {
                    if (flag == 0) {
                        flag = 2;
                    }
                    else if (flag != 2) {
                        flag = 3;
                    }
                }
            }
            pre = x;
        }
        printf("\n");
    }
    if (2 * idx + 2 < n) {
        dfs(2 * idx + 2);
    }
    if (2 * idx + 1 < n) {
        dfs(2 * idx + 1);
    }
    tpath.pop_back();
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
    }
    dfs(0);
    if (flag == 1) {
        printf("Max Heap");
    }
    else if (flag == 2) {
        printf("Min Heap");
    }
    else if (flag == 3) {
        printf("Not Heap");
    }
    else {
        while(1);
    }
    return 0;
}

```

