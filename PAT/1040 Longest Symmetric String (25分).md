# [1040 Longest Symmetric String (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805446102073344)

- 本来想用正序和逆序的最长公共子列做DP，后来意识到这样得出的子列是不连续的= =
- 遂采用最简单粗暴的遍历方法，耗时也很少
- 也可以做DP，参考柳婼的代码：https://www.liuchuo.net/archives/2104

### AC代码

```c++
#include <iostream>
using namespace std;

string s;

bool isPalindromic (int head, int tail) {
    for (int i = 0; head + i <= tail - i; i ++) {
        if (s[head + i] != s[tail - i]) {
            return false;
        }
    }
    return true;
}

int main () {
    getline(cin, s);
    int maxlen = 0;
    for (int i = 0; i < s.size(); i ++) {
        for (int j = s.size() - 1; j > i; j --) {
            if (j - i > maxlen && s[i] == s[j]) {
                if (isPalindromic(i, j)) {
                    maxlen = j - i;
                }
            }
        }
    }
    cout << maxlen + 1 << endl;
    return 0;
}

```

