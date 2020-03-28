# [1038 Recover the Smallest Number (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805449625288704)

- 排序题
- 我晕，写了长长一串`cmp`，看了下别人的代码，其实只要`a + b < b + a`就行……

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;

int n;
string seg[10010];

bool cmp (string a, string b) { // 使用cmp或cmp2均可
    return a + b < b + a;
}

bool cmp2 (string a, string b) {
    if (a.size() == b.size()) {
        return a < b;
    }
    else if (a.size() < b.size()) { // 如果a的长度小于b
        int i = 0, j = 0;
        for ( ; i < a.size(); i ++) { // 循环比较
            if (j == b.size()) {
                break;
            }
            if (a[i] == b[j]) {
                j ++;
                if (i == a.size() - 1) {
                    i = -1;
                }
                continue;
            }
            return a[i] < b[j];
        }
        return a[i] < a[0]; // 比较剩余的数字和a[0]的大小
    }
    else { // 如果a的长度大于b，与上面反过来
        int i = 0, j = 0;
        for ( ; i < b.size(); i ++) {
            if (j == a.size()) {
                break;
            }
            if (b[i] == a[j]) {
                j ++;
                if (i == b.size() - 1) {
                    i = -1;
                }
                continue;
            }
            return b[i] > a[j];
        }
        return b[i] > b[0];
    }
}

int main () {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        char s[10];
        scanf("%s", s);
        seg[i] = s;
    }
    sort(seg, seg + n, cmp2);
    string ans = "";
    for (int i = 0; i < n; i ++) {
        ans += seg[i];
    }
    int pos; // 消去开头的零
    for (pos = 0; pos < ans.size(); pos ++) {
        if (ans[pos] != '0') {
            break;
        }
    }
    if (pos == ans.size()) {
        printf("0");
    }
    else {
        ans = ans.substr(pos);
        printf("%s", ans.c_str());
    }
    return 0;
}

```

