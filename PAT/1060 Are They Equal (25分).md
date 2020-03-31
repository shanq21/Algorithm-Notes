# [1060 Are They Equal (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805413520719872)

- 给定最大有效字符长度，判断两个数在该条件下是否相等
- 将两个数分别转换好后再比较
- 容易错的地方：
  - 注意`0.00 0`和`0012 12`等异常数据
  - 如果`ans`长度比`n`小，后面要补`0`（测试点6）

### AC代码

```c++
#include <iostream>
using namespace std;

int n;
string s1, s2;

void func(string s, string &ans, int &k) {
    string inum = "", dnum = "";
    bool dot = false, firstZero = true;
    for (int i = 0; i < s.size(); i ++) { // 把s分成整数部分和小数部分
        if (s[i] == '.') {
            dot = true;
            firstZero = false;
            continue;
        }
        if (firstZero == true && s[i] != '0') { // 跳过前面无效的0
            firstZero = false;
        }
        if (firstZero == false) {
            if (dot == false) {
                inum += s[i];
            }
            else {
                dnum += s[i];
            }
        }
    }
    if (inum != "") { // 如果整数部分不为0
        k = inum.size();
        inum += dnum;
        for (int i = 0; i < inum.size() && i < n; i ++) {
            ans += inum[i];
        }
    }
    else { // 如果整数部分为0
        int pos = -1;
        for (int i = 0; i < dnum.size(); i ++) { // 找到第一个有效数字
            if (dnum[i] != '0') {
                pos = i;
                break;
            }
        }
        if (pos == -1) { // 如果小数部分也是0
            k = 0;
            ans = "0";
        }
        else {
            k = -pos;
            for (int i = pos; i < dnum.size() && ans.size() < n; i ++) {
                ans += dnum[i];
            }
        }
    }
  	while (ans.size() < n) { // ans长度不够的，后面补0
      ans += '0';
    }
}

int main() {
    cin >> n >> s1 >> s2;
    int k1, k2;
    string ans1 = "", ans2 = "";
    func(s1, ans1, k1);
    func(s2, ans2, k2);
    if (ans1 == ans2 && k1 == k2) {
        printf("YES ");
        printf("0.%s*10^%d", ans1.c_str(), k1);
    }
    else {
        printf("NO ");
        printf("0.%s*10^%d ", ans1.c_str(), k1);
        printf("0.%s*10^%d", ans2.c_str(), k2);
    }
    return 0;
}

```

