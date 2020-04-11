# [1149 Dangerous Goods Packaging (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/1038429908921778176)

- 查找题，建立`vector`数组，由于`m`远远小于`n`，故暴力查找即可（搞花里胡哨的反而超时了= =）

### AC代码

```c++
#include <iostream>
#include <unordered_set>
#include <vector>
using namespace std;

int n, m;
vector<int> list[100000];

int main() {
    scanf("%d %d", &n, &m);
    while (n --) {
        int a, b;
        scanf("%d %d", &a, &b);
        list[a].push_back(b);
        list[b].push_back(a);
    }
    while (m --) {
        int k;
        scanf("%d", &k);
        vector<int> tmpv;
        while (k --) {
            int a;
            scanf("%d", &a);
            tmpv.push_back(a);
        }
        bool isSafe = true;
        for (int i = 0; i < tmpv.size() - 1; i ++) {
            int a = tmpv[i];
            for (int j = i + 1; j < tmpv.size(); j ++) {
                int b = tmpv[j];
                for (auto it = list[a].begin(); it != list[a].end(); it ++) {
                    if (b == *it) {
                        isSafe = false;
                        break;
                    }
                }
                if (isSafe == false) {
                    break;
                }
            }
            if (isSafe == false) {
                break;
            }
        }
        printf("%s\n", isSafe ? "Yes" : "No");
    }
    return 0;
}

```

