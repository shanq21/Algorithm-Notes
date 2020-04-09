# [1133 Splitting A Linked List (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805346776760320)

- 把一条链表按数据的性质分成三个部分，要求每个部分内部数据的顺序不变
- 设置三个`vector`分别保存三个部分的数据后再合并输出即可

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

struct N {
    int addr;
    int data;
    int next;
};

int head, n, k;
N list[100000];
vector<int> p1, p2, p3;

int main() {
    cin >> head >> n >> k;
    while (n --) {
        int a, d, ne;
        cin >> a >> d >> ne;
        list[a].addr = a;
        list[a].data = d;
        list[a].next = ne;
    }
    int idx = head;
    while (idx != -1) {
        if (list[idx].data < 0) {
            p1.push_back(idx);
        }
        else if (list[idx].data <= k) {
            p2.push_back(idx);
        }
        else {
            p3.push_back(idx);
        }
        idx = list[idx].next;
    }
    p1.insert(p1.end(), p2.begin(), p2.end());
    p1.insert(p1.end(), p3.begin(), p3.end());
    for (int i = 0; i < p1.size(); i ++) {
        if (i == p1.size() - 1) {
            printf("%05d %d -1\n", list[p1[i]].addr, list[p1[i]].data);
        }
        else {
            printf("%05d %d %05d\n", list[p1[i]].addr, list[p1[i]].data, list[p1[i + 1]].addr);
        }
    }
    return 0;
}

```

