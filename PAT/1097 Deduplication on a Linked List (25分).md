# [1097 Deduplication on a Linked List (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805369774129152)

- 剔除链表中的重复元素，简单

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

struct N {
    int key;
    int next;
};

int head, n;
N list[100005];
bool mark[10005];

int main() {
    scanf("%d %d", &head, &n);
    for (int i = 0; i < n; i ++) {
        int a, k, ne;
        scanf("%d %d %d", &a, &k, &ne);
        list[a].key = k;
        list[a].next = ne;
    }
    int idx = head;
    vector<int> ans, removed;
    for (int i = 0; i < 10005; i ++) {
        mark[i] = false;
    }
    while (idx != -1) {
        int k = abs(list[idx].key);
        if (mark[k] == false) {
            mark[k] = true;
            ans.push_back(idx);
        }
        else {
            removed.push_back(idx);
        }
        idx = list[idx].next;
    }
    for (int i = 0; i < ans.size(); i ++) {
        if (i == ans.size() - 1) {
            printf("%05d %d -1\n", ans[i], list[ans[i]].key);
        }
        else {
            printf("%05d %d %05d\n", ans[i], list[ans[i]].key, ans[i + 1]);
        }
    }
    for (int i = 0; i < removed.size(); i ++) {
        if (i == removed.size() - 1) {
            printf("%05d %d -1\n", removed[i], list[removed[i]].key);
        }
        else {
            printf("%05d %d %05d\n", removed[i], list[removed[i]].key, removed[i + 1]);
        }
    }
    return 0;
}

```

