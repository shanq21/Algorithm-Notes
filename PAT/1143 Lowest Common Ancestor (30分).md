# [1143 Lowest Common Ancestor (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343727501312)

- 给定两个关键字，求这两个关键字的最近共同祖先
- 根据二叉搜索树的前序遍历还原二叉树，保存所有关键字
  - 若任一关键字不存在，则跳过
  - 若存在，把寻找关键字经历的路径保存下来，比较得到最近共同祖先

### AC代码

```c++
#include <iostream>
#include <set>
#include <vector>
using namespace std;

struct TN {
    int key;
    int left, right;
};

int m, n;
TN list[10000];
set<int> skey;

void build(int i, int j) {
    int l = -1, r = -1;
    for (int k = i + 1; k <= j; k ++) {
        if (list[k].key >= list[i].key) {
            r = k;
            break;
        }
        l = k;
    }
    if (l != -1) {
        list[i].left = i + 1;
        build(i + 1, l);
    }
    else {
        list[i].left = -1;
    }
    list[i].right = r;
    if (r != -1) {
        build(r, j);
    }
}

void findX(vector<int> &v, int x) {
    int idx = 0;
    while (idx != -1) {
        v.push_back(idx);
        if (list[idx].key == x) {
            break;
        }
        else if (list[idx].key < x) {
            idx = list[idx].right;
        }
        else {
            idx = list[idx].left;
        }
    }
}

int main() {
    cin >> m >> n;
    for (int i = 0; i < n; i ++) {
        cin >> list[i].key;
        skey.insert(list[i].key);
    }
    build(0, n - 1);
    vector<int> av, bv;
    while (m --) {
        int a, b;
        cin >> a >> b;
        int flag = 0;
        if (skey.find(a) == skey.end()) {
            flag = 1;
        }
        if (skey.find(b) == skey.end()) {
            if (flag == 0) {
                flag = 2;
            }
            else {
                flag = 3;
            }
        }
        if (flag != 0) {
            if (flag == 3) {
                printf("ERROR: %d and %d are not found.\n", a, b);
            }
            else {
                printf("ERROR: %d is not found.\n", flag == 1 ? a : b);
            }
            continue;
        }
        av.clear();
        bv.clear();
        findX(av, a);
        findX(bv, b);
        int pos = 0;
        while (pos < av.size() && pos < bv.size()) {
            if (av[pos] != bv[pos]) {
                break;
            }
            pos ++;
        }
        if (pos == av.size()) {
            printf("%d is an ancestor of %d.\n", a, b);
        }
        else if (pos == bv.size()) {
            printf("%d is an ancestor of %d.\n", b, a);
        }
        else {
            printf("LCA of %d and %d is %d.\n", a, b, list[av[pos - 1]].key);
        }
    }
    return 0;
}


```

