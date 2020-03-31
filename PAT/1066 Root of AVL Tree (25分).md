# [1066 Root of AVL Tree (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805404939173888)

- 暂时跳过，树好难啊= =

### 非AC代码


```c++
#include <iostream>
using namespace std;

struct TN {
    int value;
    int left;
    int right;
};

int n;
TN list[25];
int root;

int getH(int id) {
    if (id == -1) {
        return 0;
    }
    return max(getH(list[id].left), getH(list[id].right)) + 1;
}

void insert(int id) {
    int cur = root;
    while (cur != -1) { // 找到插入的节点
        if (list[id].value < list[cur].value) {
            if (list[cur].left == -1) {
                list[cur].left = id;
                if (abs(getH(list[root].left) - getH(list[root].right)) > 1) { // 当不平衡时
                    
                }
                break;
            }
            else {
                cur = list[cur].left;
            }
        }
        else {
            if (list[cur].right == -1) {
                list[cur].right = id;
                break;
            }
            else {
                cur = list[cur].right;
            }
        }
    }
    
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &list[i].value);
        list[i].left = -1;
        list[i].right = -1;
    }
    root = 0;
}

```

