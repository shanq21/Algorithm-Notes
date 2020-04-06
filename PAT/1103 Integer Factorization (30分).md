# [1103 Integer Factorization (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805364711604224)

- `DFS`+剪枝
- 测试点6一直答案错误，太tm诡异了……
  - 啊啊啊啊啊气死了，琢磨了三天才发现是自己写的`pow`函数有问题，只在算二次方的时候有效……跪了orz

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int n, k, p;
vector<int> tpath, ans;

int powp(int a) {
    int x = p, ret = 1;
    while (x -- > 0) {
        ret *= a;
        if (ret > n) {
            break;
        }
    }
    return ret;
}

void dfs(int sum, int cnt) {
    if (sum == n && cnt == 0) {
        if (ans.empty()) {
            ans = tpath;
        }
        else {
            int sumt = 0, suma = 0;
            for (int i = 0; i < k; i ++) {
                sumt += tpath[i];
                suma += ans[i];
            }
            if (sumt > suma) {
                ans = tpath;
            }
            else if (sumt == suma) {
                bool isAns = true;
                for (int i = 0; i < k; i ++) {
                    if (tpath[i] < ans[i]) {
                        isAns = false;
                        break;
                    }
                }
                if (isAns) {
                    ans = tpath;
                }
            }
        }
    }
    else if (sum < n - cnt + 1 && cnt > 0) {
        int bound = n;
        if (!tpath.empty()) {
            bound = tpath[tpath.size() - 1];
        }
        for (int i = bound; i >= 1; i --) {
            int tmp = sum + powp(i);
            if (tmp <= n - cnt + 1) {
                tpath.push_back(i);
                dfs(tmp, cnt - 1);
                tpath.pop_back();
            }
        }
    }
}

int main() {
    scanf("%d %d %d", &n, &k, &p);
    dfs(0, k);
    if (ans.empty()) {
        printf("Impossible");
    }
    else {
        printf("%d = ", n);
        for (int i = 0; i < k; i ++) {
            if (i != 0) {
                printf(" + ");
            }
            printf("%d^%d", ans[i], p);
        }
    }
    return 0;
}


```

