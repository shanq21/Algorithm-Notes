# [1053 Path of Equal Weight (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805424153280512)

- 查找从根节点到叶子节点权重之和等于给定数值的路径，按大小输出路径上各节点的权重
- 用`DFS`遍历即可，使用一个`vector`保存临时路径

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct TN {
    int w;
    vector<int> child;
};

int n, m, tg;
TN node[101];

vector<int> tpath; // 存放临时路径
void dfs(int id, int sum) {
    tpath.push_back(id);
    sum += node[id].w;
    if (node[id].child.empty() && sum == tg) {
        for (int i = 0; i < tpath.size(); i ++) {
            if (i != 0) {
                printf(" ");
            }
            printf("%d", node[tpath[i]].w);
        }
        printf("\n");
    }
    else if (!node[id].child.empty() && sum < tg) {
        for (int i = 0; i < node[id].child.size(); i ++) {
            dfs(node[id].child[i], sum);
            tpath.pop_back();
        }
    }
}

bool cmp(int a, int b) {
    return node[a].w > node[b].w;
}

int main() {
    scanf("%d %d %d", &n, &m, &tg);
    for (int i = 0; i < n; i ++) { // 输入权重
        int w;
        scanf("%d", &w);
        node[i].w = w;
    }
    for (int i = 0; i < m; i ++) { // 输入非叶子节点
        int id, k;
        scanf("%d %d", &id, &k);
        for (int j = 0; j < k; j ++) {
            int cid;
            scanf("%d", &cid);
            node[id].child.push_back(cid);
        }
        sort(node[id].child.begin(), node[id].child.end(), cmp); // 按权重大小排序孩子节点
    }
    dfs(0, 0);
    return 0;
}

```

