# [1056 Mice and Rice (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805419468242944)

- 做了好一会儿才搞懂题目= = 主要是`initial order`那里迷惑了很久，以为每个数字代表的是出场顺序，原来代表的是选手序号……
- 用了指针`vector`来做，感觉有一点点难看懂（不过看了看别人的代码也差不多23333

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Person {
    int w;
    int rank;
};

int n, ng;
Person person[1005];

bool cmp(Person* a, Person* b) {
    return a->w > b->w;
}

int main() {
    scanf("%d %d", &n, &ng);
    for (int i = 0; i < n; i ++) {
        int w;
        scanf("%d", &w);
        person[i].w = w;
    }
    vector<Person*> contest; // 每轮比赛的人
    for (int i = 0; i < n; i ++) {
        int id;
        scanf("%d", &id);
        contest.push_back(&person[id]);
    }
    vector<Person*> group; // 每组比赛的人
    while (contest.size() > 1) { // 当contest中只有一个人时，即胜者已决出
        vector<Person*> tmpv; // 临时存放下一轮的人
        int gnum = contest.size() / ng + (contest.size() % ng == 0 ? 0 : 1); // 组数
        for (int i = 0; i < contest.size(); i ++) {
            group.push_back(contest[i]);
            if (group.size() == ng || i == contest.size() - 1) { // 满人或只剩最后几人不满一组时结算
                sort(group.begin(), group.end(), cmp);
                tmpv.push_back(group[0]); // 胜者进入下一轮
                for (int i = 1; i < group.size(); i ++) { // 记录败者名次，即组数+1
                    group[i]->rank = gnum + 1;
                }
                group.clear(); // 组清零
            }
        }
        contest.clear(); // 更新下一轮名单
        for (int i = 0; i < tmpv.size(); i ++) {
            contest.push_back(tmpv[i]);
        }
    }
    contest[0]->rank = 1; // 胜者排名为1
    for (int i = 0; i < n; i ++) {
        if (i != 0) {
            printf(" ");
        }
        printf("%d", person[i].rank);
    }
    return 0;
}

```

