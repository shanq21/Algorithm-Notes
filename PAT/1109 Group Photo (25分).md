# [1109 Group Photo (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805360043343872)

- 拍合照时，后排要比前排高，中间的人要比两边高，按此要求排队
- 把所有人的身高从高到低排序，设置一个`flag`变量表示当前应该在左边还是在右边，依次排列

### AC代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct P {
    string name;
    int h;
};

int n, k;
P list[10005];
int ans[10005][10005];

bool cmp(P a, P b) {
    if (a.h > b.h) {
        return true;
    }
    else if (a.h == b.h && a.name < b.name) {
        return true;
    }
    else {
        return false;
    }
}

int main() {
    cin >> n >> k;
    for (int i = 0; i < n; i ++) {
        cin >> list[i].name >> list[i].h;
    }
    sort(list, list + n, cmp);
    int l, r, row, level = 1, cnt = 0, flag = 0;
    for (int i = 0; i < n; i ++) {
        cnt ++;
        if (cnt == 1) {
            row = n / k + (level == 1 ? n % k : 0);
            l = r = row / 2 + 1;
            ans[level][l] = i;
            continue;
        }
        if (flag == 0 && l - 1 >= 1) {
            l --;
            ans[level][l] = i;
            flag = 1;
        }
        else if (flag == 1 && r + 1 <= row) {
            r ++;
            ans[level][r] = i;
            flag = 0;
        }
        if (cnt == row) {
            level ++;
            cnt = 0;
            flag = 0;
            continue;
        }
    }
    for (int i = 1; i <= k; i ++) {
        row = n / k + (i == 1 ? n % k : 0);
        for (int j = 1; j <= row; j ++) {
            if (j != 1) {
                cout << " ";
            }
            cout << list[ans[i][j]].name;
        }
        cout << endl;
    }
    return 0;
}

```

