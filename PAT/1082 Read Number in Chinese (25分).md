# [1082 Read Number in Chinese (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805385053978624)

- 整数的中文念法
- 拆解数位，如果是`0`则跳过，根据位置决定后缀
- 记录前一个非零数位的位置，若与当前位置不相邻，要根据情况考虑补`wan`或`ling`，遍历结束后还要再判断一次
- `n==0`的情况单独判断（测试点3）

### AC代码

```c++
#include <iostream>
using namespace std;

string ch[] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
int n;
int digit[10];
string ans = "";

int main() {
    cin >> n;
    if (n < 0) {
        ans += "Fu";
        n = -n;
    }
    if (n == 0) {
        cout << "ling" << endl;
        return 0;
    }
    int size = 0;
    while (n != 0) {
        digit[size ++] = n % 10;
        n /= 10;
    }
    int preIdx = size - 1;
    for (int i = size - 1; i >= 0; i --) {
        if (digit[i] == 0) {
            continue;
        }
        if (preIdx - i > 1) { // 如果前一个非零数位与当前数位不相邻，则需要考虑补零和万
            if (preIdx > 4 && preIdx != 8 && i < 4) {
                ans += " Wan";
                if (i < 3) {
                    ans += " ling";
                }
            }
            else {
                ans += " ling";
            }
        }
        preIdx = i;
        if (ans != "") {
            ans += " ";
        }
        ans += ch[digit[i]];
        if (i == 8) {
            ans += " Yi";
        }
        else if (i == 7 || i == 3) {
            if (digit[i] != 0) {
                ans += " Qian";
            }
        }
        else if (i == 6 || i == 2) {
            if (digit[i] != 0) {
                ans += + " Bai";
            }
        }
        else if (i == 5 || i == 1) {
            ans += " Shi";
        }
        else if (i == 4) {
            ans += " Wan";
        }
    }
    if (preIdx > 4 && preIdx != 8) {
        ans += " Wan";
    }
    cout << ans << endl;
    return 0;
}

```

