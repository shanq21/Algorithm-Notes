# [1039 Course List for Student (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805447855292416)

- 输入数据的处理和查找，镜面题（简单）：[1047 Student List for Course (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805433955368960)
- 一开始使用`map`和`vector`数组模拟哈希表，但最后一个测试点段错误，遂改为`vector<int> stu[27][27][27][10]`

### AC代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int n, k;
vector<int> stu[27][27][27][10];
vector<string> query;

int main () {
    scanf("%d %d", &n, &k);
    for (int i = 0; i < k; i ++) {
        int cid, m;
        scanf("%d %d", &cid, &m);
        for (int j = 0; j < m; j ++) {
            char name[5];
            scanf("%s", name);
            stu[name[0] - 'A'][name[1] - 'A'][name[2] - 'A'][name[3] - '0'].push_back(cid);
        }
    }
    for (int i = 0; i < n; i ++) {
        char name[5];
        scanf("%s", name);
        auto tmp = stu[name[0] - 'A'][name[1] - 'A'][name[2] - 'A'][name[3] - '0'];
        printf("%s %i", name, tmp.size());
        sort(tmp.begin(), tmp.end());
        for (int j = 0; j < tmp.size(); j ++) {
            printf(" %d", tmp[j]);
        }
        printf("\n");
    }
    return 0;
}

```

