# [1093 Count PAT's (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805373582557184)

- 直接暴力遍历的话，最后三个测试点2、3、4会超时
- 用递推的方法，设置`p`、`pa`、`pat`三个变量，分别记录到`s[i]`为止有多少个`P`、多少个`PA`、多少个`PAT`
  - 如果`s[i]`是`P`，`p = p + 1`
  - 如果`s[i]`是`A`，`pa = pa + p`
  - 如果`s[i]`是`T`，`pat = pat + pa`
- 遍历完毕后，`pat`即为所求

### AC代码

```c++
#include <iostream>
using namespace std;

const int mod = 1000000007;
int p, pa, pat; // 到s[i]为止有多少个P、多少个PA、多少个PAT

int main() {
    string s;
    cin >> s;
    int size = s.size();
    p = pa = pat = 0;
    for (int i = 0; i < size; i ++) {
        if (s[i] == 'P') {
            p ++;
            p %= mod;
        }
        else if (s[i] == 'A') {
            pa += p;
            pa %= mod;
        }
        else if (s[i] == 'T') {
            pat += pa;
            pat %= mod;
        }
    }
    cout << pat << endl;
    return 0;
}

```

