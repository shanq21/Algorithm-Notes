# [1130 Infix Expression (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805347921805312)

- 根据符号树生成表达式
- `DFS`遍历，用`pair`返回子式字符串和是否为叶子节点
  - 如果不是叶子节点，则需要给返回的字符串加括号

### AC代码

```c++
#include <iostream>
using namespace std;

struct TN {
    int flag;
    string key;
    int left, right;
};

int n, root;
TN list[21];
bool mark[21];

pair<string, int> dfs(int x) {
    string l = "", r = "";
    int fl = 0, fr = 0;
    if (list[x].left != -1) {
        auto tmp = dfs(list[x].left);
        l = tmp.first;
        fl = tmp.second;
    }
    if (list[x].right != -1) {
        auto tmp = dfs(list[x].right);
        r = tmp.first;
        fr = tmp.second;
    }
    if (fl != 0) {
        l = "(" + l + ")";
    }
    if (fr != 0) {
        r = "(" + r + ")";
    }
    return make_pair(l + list[x].key + r, list[x].flag);
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i ++) { // 初始化
        mark[i] = false;
    }
    for (int i = 1; i <= n; i ++) {
        string s;
        int l, r;
        cin >> s >> l >> r;
        list[i].key = s;
        list[i].left = l;
        list[i].right = r;
        if (l != -1) {
            mark[l] = true;
        }
        if (r != -1) {
            mark[r] = true;
        }
        if (l == -1 && r == -1) {
            list[i].flag = 0;
        }
        else {
            list[i].flag = 1;
        }
    }
    for (int i = 1; i <= n; i ++) { // 寻找根节点
        if (mark[i] == false) {
            root = i;
            break;
        }
    }
    string ans = dfs(root).first;
    cout << ans << endl;
    return 0;
}

```

