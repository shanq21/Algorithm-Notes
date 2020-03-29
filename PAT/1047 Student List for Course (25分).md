# [1047 Student List for Course (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805433955368960)

- 输入数据的处理和组织，镜面题：[1039 Course List for Student (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805447855292416)，这题更简单

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int n, k;
vector<string> course[2505];

int main () {
    scanf("%d %d", &n, &k);
    for (int i = 0; i < n; i ++) {
        char s[5];
        int m;
        scanf("%s %d", s, &m);
        for (int j = 0; j < m; j ++) {
            int cid;
            scanf("%d", &cid);
            course[cid].push_back(s);
        }
    }
    for (int i = 1; i <= k; i ++) {
        sort(course[i].begin(), course[i].end());
        printf("%d %d\n", i, course[i].size());
        for (int j = 0; j < course[i].size(); j ++) {
            printf("%s\n", course[i][j].c_str());
        }
    }
    return 0;
}

```

