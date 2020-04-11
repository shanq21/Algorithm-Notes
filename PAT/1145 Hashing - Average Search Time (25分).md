# [1145 Hashing - Average Search Time (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343236767744)

- 哈希表的平均查找长度，二次探测
- `idx = (key + i * i) % size`，`i`的范围是`0`到`size`

### AC代码

```c++
#include <iostream>
#include <vector>
using namespace std;

int size, n, m;
bool prime[10010];
int list[10010];

int main() {
    prime[0] = prime[1] = false; // 初始化prime
    for (int i = 2; i < 10010; i ++) {
        prime[i] = true;
    }
    for (int i = 2; i * i < 10010; i ++) {
        if (prime[i] == true) {
            for (int j = i * i; j < 10010; j += i) {
                prime[j] = false;
            }
        }
    }
    cin >> size >> n >> m;
    while (prime[size] == false) { // 找到合适的size
        size ++;
    }
    for (int i = 0; i < size; i ++) { // 初始化哈希表
        list[i] = -1;
    }
    while (n --) {
        int a;
        cin >> a;
        int idx = 0;
        bool found = false;
        for (int i = 0; i <= size; i ++) {
            idx = (a + i * i) % size;
            if (list[idx] == -1) {
                list[idx] = a;
                found = true;
                break;
            }
        }
        if (!found) {
            printf("%d cannot be inserted.\n", a);
        }
    }
    int sum = 0, copy = m;
    while (copy --) {
        int a;
        cin >> a;
        int idx = 0, cnt = 0;
        for (int i = 0; i <= size; i ++) { // i的范围是0~size
            cnt ++;
            idx = (a + i * i) % size;
            if (list[idx] == -1 || list[idx] == a) {
                break;
            }
            
        }
        sum += cnt;
    }
    printf("%.1f", double(sum) / m);
    return 0;
}

```

