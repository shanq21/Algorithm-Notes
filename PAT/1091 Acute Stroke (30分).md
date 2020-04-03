# [1091 Acute Stroke (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805375457411072)

- 用`DFS`最后两个测试点4、5会段错误（堆栈溢出），因此要用`BFS`

### AC代码

```c++
#include <iostream>
#include <queue>
using namespace std;

struct N {
    int x, y, z;
};

int m, n, l, t;
int cube[65][1300][130];
bool mark[65][1300][130];
int ans = 0;

int go[6][3] = {1, 0, 0, 0, 1, 0, 0, 0, 1, -1, 0, 0, 0, -1, 0, 0, 0, -1};

//void dfs(int x, int y, int z, int &cnt) {
//    for (int i = 0; i < 6; i ++) {
//        int nx = x + go[i][0];
//        int ny = y + go[i][1];
//        int nz = z + go[i][2];
//        if (nx >= 0 && nx < l && ny >= 0 && ny < m && nz >= 0 && nz < n) {
//            if (cube[nx][ny][nz] == 1 && mark[nx][ny][nz] == false) {
//                mark[nx][ny][nz] = true;
//                cnt ++;
//                dfs(nx, ny, nz, cnt);
//            }
//        }
//    }
//}

void bfs(N node) {
    int cnt = 1;
    queue<N> q;
    q.push(node);
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            N tn = q.front();
            q.pop();
            for (int j = 0; j < 6; j ++) {
                int nx = tn.x + go[j][0];
                int ny = tn.y + go[j][1];
                int nz = tn.z + go[j][2];
                if (nx >= 0 && nx < l && ny >= 0 && ny < m && nz >= 0 && nz < n) {
                    if (cube[nx][ny][nz] == 1 && mark[nx][ny][nz] == false) {
                        mark[nx][ny][nz] = true;
                        cnt ++;
                        N ttn = {nx, ny, nz};
                        q.push(ttn);
                    }
                }
            }
        }
    }
    if (cnt >= t) {
        ans += cnt;
    }
}

int main() {
    scanf("%d %d %d %d", &m, &n, &l, &t);
    for (int i = 0; i < l; i ++) {
        for (int j = 0; j < m; j ++) {
            for (int k = 0; k < n;k ++) {
                scanf("%d", &cube[i][j][k]);
                mark[i][j][k] = false; // 顺便初始化标记数组
            }
        }
    }
    for (int i = 0; i < l; i ++) {
        for (int j = 0; j < m; j ++) {
            for (int k = 0; k < n;k ++) {
                if (cube[i][j][k] == 1 && mark[i][j][k] == false) {
                    mark[i][j][k] = true;
                    N node = {i, j, k};
                    bfs(node);
                }
            }
        }
    }
    printf("%d", ans);
    return 0;
}

```

