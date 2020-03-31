# [1071 Speech Patterns (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805398257647616)

- 找到出现频率最高的单词，对大小写不敏感
- 用`map`来保存单词的出现频率即可（还以为会超时，结果一遍过2333）

### AC代码

```c++
#include <iostream>
#include <map>
using namespace std;

map<string, int> mymap;

void add(string s) {
    if (mymap.find(s) == mymap.end()) {
        mymap[s] = 0;
    }
    mymap[s] ++;
}

int main() {
    string s;
    while (cin >> s) {
        string tmp = "";
        for (int i = 0; i < s.size(); i ++) {
            if ((s[i] >= '0' && s[i] <= '9') || (s[i] >= 'a' && s[i] <= 'z')) {
                tmp += s[i];
            }
            else if (s[i] >= 'A' && s[i] <= 'Z') {
                tmp += s[i] + 'a' - 'A';
            }
            else {
                add(tmp);
                tmp = "";
            }
        }
        if (tmp != "") {
            add(tmp);
        }
    }
    int maxn = 0;
    string ans = "";
    for (auto it = mymap.begin(); it != mymap.end(); it ++) {
        if (it->second > maxn) {
            maxn = it->second;
            ans = it->first;
        }
    }
    cout << ans << " " << maxn << endl;
    return 0;
}

```

