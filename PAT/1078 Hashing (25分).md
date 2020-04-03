# [1078 Hashing (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805389634158592)

- 哈希表的二次探测法

- `MSize = 10000`后的第一个质数是`10007`，因此数组要开到`10007`以上，否则会段错误
- 注意二次探测法是`pos = (key + k *k) % size`，而不是`pos = key % size + k * k`，这里的`k`的范围是`0 ~ size - 1`

### AC代码

```c++
#include <iostream>

const int maxn = 10020;
int size, n;
int hash[maxn];
int prime[maxn];

void init() {
    for (int i = 2; i < maxn; i ++) {
        prime[i] = true;
    }
    for (int i = 2; i * i < maxn; i ++) {
        if (prime[i] == true) {
            for (int j = i * i; j < maxn; j += i) {
                prime[j] = false;
            }
        }
    }
}

int main() {
    init();
    scanf("%d %d", &size, &n);
    while (prime[size] == false) {
        size ++;
    }
    for (int i = 0; i < size; i ++) {
        hash[i] = -1;
    }
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        int pos = 0;
        bool found = false;
        for (int j = 0; j <= size - 1; j ++) {
            pos = (a + j * j) % size;
            if (hash[pos] == -1) {
                found = true;
                break;
            }
        }
        if (i != 0) {
            printf(" ");
        }
        if (found) {
            hash[pos] = a;
            printf("%d", pos);
        }
        else {
            printf("-");
        }
    }
    return 0;
}

```

