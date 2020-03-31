# [1074 Reversing Linked List (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805394512134144)

- 链表，用栈实现逆序

### AC代码

```c++
#include <iostream>
#include <stack>
using namespace std;

struct N {
    int addr;
    int data;
    int next;
};

int n, k, head;
N list[100005];
N* ans[100005];

int main() {
    scanf("%d %d %d", &head, &n, &k);
    for (int i = 0; i < n; i ++) {
        int a, d, ne;
        scanf("%d %d %d", &a, &d, &ne);
        list[a].addr = a;
        list[a].data = d;
        list[a].next = ne;
    }
    int idx = head;
    int cnt = 0, size = 0;
    stack<N*> sta;
    while (idx != -1) {
        cnt ++;
        sta.push(&list[idx]);
        idx = list[idx].next;
        if (cnt == k) {
            while (!sta.empty()) {
                ans[size ++] = sta.top();
                sta.pop();
            }
            cnt = 0;
        }
    }
    if (cnt > 0) {
        stack<N*> tmp;
        while (!sta.empty()) {
            tmp.push(sta.top());
            sta.pop();
        }
        while (!tmp.empty()) {
            ans[size ++] = tmp.top();
            tmp.pop();
        }
    }
    for (int i = 0; i < size; i ++) {
        if (i == size - 1) {
            printf("%05d %d -1\n", ans[i]->addr, ans[i]->data);
        }
        else {
            printf("%05d %d %05d\n", ans[i]->addr, ans[i]->data, ans[i + 1]->addr);
        }
    }
    return 0;
}

```

