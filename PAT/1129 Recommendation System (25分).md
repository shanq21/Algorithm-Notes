# [1129 Recommendation System (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805348471259136)

- 按出现的频率高低输出元素，元素序列持续添加
- 每次只需比较更新的元素与`output`的最后一个元素，输出前`sort`即可

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int n, k;
int cnt[50001];
vector<int> output;

bool cmp(int a, int b) {
    if (cnt[a] > cnt[b]) {
        return true;
    }
    else if (cnt[a] == cnt[b] && a < b) {
        return true;
    }
    else {
        return false;
    }
}

int main() {
    scanf("%d %d", &n, &k);
    for (int i = 1; i <= n; i ++) {
        cnt[i] = 0;
    }
    for (int i = 0; i < n; i ++) {
        int a;
        scanf("%d", &a);
        cnt[a] ++;
        if (output.empty()) {
            output.push_back(a);
            continue;
        }
        printf("%d:", a);
        for (int j = 0; j < output.size(); j ++) {
            printf(" %d", output[j]);
        }
        printf("\n");
        if (i == n - 1) {
            break;
        }
        int pos = -1;
        for (int j = 0; j < output.size(); j ++) {
            if (output[j] == a) {
                pos = j;
                break;
            }
        }
        if (pos == -1) { // 如果output中没有a
            if (output.size() < k) { // 如果output还没满，就往里加a
                output.push_back(a);
            }
            else { // 如果output已经满了，比较a和output[k-1]哪个大
                if (cnt[a] > cnt[output[k - 1]] || (cnt[a] == cnt[output[k - 1]] && a < output[k - 1])) {
                    output[k - 1] = a;
                }
            }
        }
        sort(output.begin(), output.end(), cmp);
    }
    return 0;
}

```

