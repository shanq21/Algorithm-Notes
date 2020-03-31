# [1064 Complete Binary Search Tree (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805407749357568)

- 将输入数据升序排序，根据总节点个数计算左子树个数，从而得到根节点下标，不断递归即可建立树

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <queue>
using namespace std;

struct TN {
    int value;
    int left;
    int right;
};

int n;
TN list[1005];

int build(int minn, int maxx) { // 在有序序列中寻找root节点
    if (minn > maxx) {
        return -1;
    }
    int m = maxx - minn + 1; // 从minn到maxx的节点个数
    int sum = 1, x;
    int ln = 0; // 左子树的节点个数
    for (x = 2; sum + x < m; x *= 2) {
        ln += x / 2;
        sum += x;
    }
    ln += min(m - sum, x / 2);
    int root = minn + ln; // 根节点下标
    list[root].left = build(minn, root - 1);
    list[root].right = build(root + 1, maxx);
    return root;
}

bool cmp(TN a, TN b) {
    return a.value < b.value;
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++) {
        scanf("%d", &list[i].value);
    }
    sort(list + 1, list + 1 + n, cmp);
    int root = build(1, n);
    queue<int> q;
    q.push(root);
    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i ++) {
            int cur = q.front();
            q.pop();
            if (cur != root) {
                printf(" ");
            }
            printf("%d", list[cur].value);
            if (list[cur].left != -1) {
                q.push(list[cur].left);
            }
            if (list[cur].right != -1) {
                q.push(list[cur].right);
            }
        }
    }
    return 0;
}

```

