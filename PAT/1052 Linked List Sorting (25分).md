# [1052 Linked List Sorting (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805425780670464)

- 根据链表找到所有节点，再对它们按`key`排序后输出，输出时要把`next`修改为下一个节点的`address`，注意输出格式
- 第一次提交时段错误，参考网上代码时发现是一开始给的`head`有可能是`-1`，需要特殊处理一下

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Node {
    int addr;
    int key;
    int next;
};

int n, head;
Node node[100005];

bool cmp (Node a, Node b) {
    return a.key < b.key;
}

int main () {
    scanf("%d %d", &n, &head);
    for (int i = 0; i < n; i ++) {
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        node[a].addr = a;
        node[a].key = b;
        node[a].next = c;
    }
    vector<Node> list;
    int idx = head;
    while (idx != -1) {
        list.push_back(node[idx]);
        idx = node[idx].next;
    }
    sort(list.begin(), list.end(), cmp);
    if (!list.empty()) {
        printf("%d %05d\n", list.size(), list[0].addr);
    }
    else {
        printf("0 -1\n");
    }
    for (int i = 0; i < list.size(); i ++) {
        if (i == list.size() - 1) {
            printf("%05d %d -1\n", list[i].addr, list[i].key);
        }
        else {
            printf("%05d %d %05d\n", list[i].addr, list[i].key, list[i + 1].addr);
        }
    }
    return 0;
}

```

