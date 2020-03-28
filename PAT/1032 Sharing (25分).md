# [1032 Sharing (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805460652113920)

- 考查链表结点的遍历
- 处理输入的时候卡了好久，发现不能直接赋值数组元素，猜想应该是数组下标比较大的缘故，以后还是要尽量先把数据用临时变量取出再赋值

```c++
#include <iostream>

struct N {
    char c;
    int next;
    bool mark;
};

int st1, st2, n;
N node[100000];
int ans;

int main () {
    scanf("%d %d %d", &st1, &st2, &n);
    for (int i = 0; i < n; i ++) {
        int a, c;
        char b;
        scanf("%d %c %d", &a, &b, &c); // 这里不能直接赋值node[idx].c，为什么？
        node[a].c = b;
        node[a].next = c;
        node[a].mark = false;
    }
    int idx = st1;
    while (idx != -1) {
        node[idx].mark = true;
        idx = node[idx].next;
    }
    idx = st2;
    while (idx != -1) {
        if (node[idx].mark == true) {
            printf("%05d", idx);
            return 0;
        }
        idx = node[idx].next;
    }
    printf("-1");
    return 0;
}

```

