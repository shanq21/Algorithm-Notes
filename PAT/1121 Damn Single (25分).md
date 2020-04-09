# [1121 Damn Single (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805352359378944)

- 找到派对中的落单人士，包括单身狗和伴侣不在场的人
- `map`记录情侣，标记数组标记在场的人，遍历再排序即可

### AC代码

```c++
#include <iostream>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

int n, list[10000];
map<int, int> couple;
bool mark[100000];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        int a, b;
        scanf("%d %d", &a, &b);
        couple[a] = b;
        couple[b] = a;
    }
    for (int i = 0; i < 100000; i ++) {
        mark[i] = false;
    }
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        mark[a] = true;
        list[i] = a;
    }
    vector<int> ans;
    for (int i = 0; i < n; i ++) {
        if (mark[couple[list[i]]] == false) {
            ans.push_back(list[i]);
        }
    }
    sort(ans.begin(), ans.end());
    printf("%d\n", ans.size());
    for (int i = 0; i < ans.size(); i ++) {
        if (i != 0) {
            printf(" ");
        }
        printf("%05d", ans[i]);
    }
    return 0;
}

```

