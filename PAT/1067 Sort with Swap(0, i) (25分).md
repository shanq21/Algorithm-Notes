# [1067 Sort with Swap(0, i) (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805403651522560)

- 直接暴力查找交换会超时
- 使用`hash`后测试点1仍然会超时，问题在于`hash[0]==0`时的策略
  - 方法：记录上一次找到的尚未对应的下标，从该下标开始遍历，而不是从头开始，可将该部分的时间复杂度降为$O(n)$

### AC代码

```c++
#include <iostream>

int n;
int list[100005];
int hash[100005]; // 保存数组下标

int main() {
    scanf("%d", &n);
    for(int i = 0; i < n; i ++) {
        scanf("%d", &list[i]);
        hash[list[i]] = i;
    }
    int cnt = 0;
    int pos = 0;
    while(1) {
        cnt ++;
        if (hash[0] == 0) { // 如果0就在0，跟下一个不同的交换
            for (int i = pos; i < n; i ++) {
                if (list[i] != i) {
                    list[0] = list[i];
                    hash[list[i]] = 0;
                    list[i] = 0;
                    hash[0] = i;
                    pos = i;
                    break;
                }
            }
            if (list[0] == 0) {
                break;
            }
        }
        else { // 如果0不在0，跟其下标交换
            int tmp = hash[0];
            list[hash[tmp]] = 0;
            hash[0] = hash[tmp];
            list[tmp] = tmp;
            hash[tmp] = tmp;
        }
    }
    printf("%d", cnt - 1);
    return 0;
}

```

