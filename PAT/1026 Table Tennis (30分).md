# [1026 Table Tennis (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805472333250560)

- 坑点

  - 四舍五入，光用`%.f`不行，要用`round`

- > 逻辑如下
  >
  > - 找到最早空闲且编号最小的房间
  >   - 如果队列中的第一个顾客是VIP
  >     - 如果他到了
  >       - 如果当前房间不是VIP房，查找有没有空闲时间相等的VIP房
  >         - 如果有，把当前房间设为这个VIP房
  >       - 把当前房间给他
  >     - 如果他没到
  >       - **如果他没到，需要判断他到了之后是否有编号更小的VIP房**
  >   - 如果队列中的第一个顾客不是VIP
  >     - 如果他到了，查找队列里有没有已经到了的VIP
  >       - 如果有，就把房给VIP
  >       - 如果没有，就把这个房给他
  >     - 如果他没到，也把这个房给他？

### 非AC代码

- 测试点8未过

```c++
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

struct P {
    int come; // 到达时刻
    int serve; // 被服务的时刻
    int time; // 服务时间
    bool isVIP;
};

struct T {
    int end; // 下一次空闲的时刻
    int cnt; // 累计服务人数
    bool isVIP = false;
};

int n, k, m;
P player[10001];
T table[101];
bool mark[10001];
int size;
const int inf = 123123123;

bool cmp (P a, P b) {
    return a.come < b.come;
}

bool cmp2 (P a, P b) {
    return a.serve < b.serve;
}

void output (int t) {
    int hh, mm, ss;
    hh = t / 3600;
    t %= 3600;
    mm = t / 60;
    t %= 60;
    ss = t;
    printf("%02d:%02d:%02d", hh, mm, ss);
}

int main () {
    scanf("%d", &n);
    size = 0;
    for (int i = 0; i < n; i ++) {
        int hh, mm, ss, t, tag;
        scanf("%d:%d:%d %d %d", &hh, &mm, &ss, &t, &tag);
        int sum = hh * 3600 + mm * 60 + ss;
        if (sum >= 21 * 3600) { // 如果到达时刻晚于21点，则不计入
            continue;
        }
        t = t <= 120 ? t * 60 : 120 * 60;
        player[size].isVIP = tag == 1 ? true : false;
        player[size].come = sum;
        player[size ++].time = t;
    }
    scanf("%d %d", &k, &m);
    for (int i = 1; i <= k; i ++) { // 初始化
        table[i].end = 8 * 3600; // 把下一次空闲时刻设为8点
        table[i].cnt = 0;
        table[i].isVIP = false;
    }
    for (int i = 0; i < m; i ++) {
        int idx;
        scanf("%d", &idx);
        table[idx].isVIP = true; // 设相应的table为VIP
    }
    if (size == 0) { // 如果没有在21点前到达的人
        for (int i = 1; i <= k; i ++) {
            if (i != 1) {
                printf(" ");
            }
            printf("0");
        }
        return 0;
    }
    sort(player, player + size, cmp); // 按到达时刻排序
    fill(mark, mark + size, false);
    for (int i = 0; i < size; i ++) {
        int cur = -1;
        for (int j = 0; j < size; j ++) { // 找到队列里的第一个人
            if (!mark[j]) {
                cur = j;
                break;
            }
        }
        if (cur == -1) {
            break;
        }
        int tmin = inf, idx = -1;
        for (int j = 1; j <= k; j ++) { // 找到空闲最早的table
            if (table[j].end < tmin) {
                tmin = table[j].end;
                idx = j;
            }
        }
        if (table[idx].end >= 21 * 3600) { // 如果已经关门，则退出循环
            size = i;
            break;
        }
        if (player[cur].isVIP) { // 如果cur是VIP，寻找是否有VIP房
            for (int j = 1; j <= k; j ++) {
                if (table[j].end == tmin && table[j].isVIP) {
                    idx = j;
                    break; // 找到就退出循环，以保证编号最小
                }
            }
        }
        if (table[idx].isVIP) { // 如果是VIP房间
            if (!player[cur].isVIP) { // 如果cur不是VIP，查找是否有已经到了的VIP
                int vip = -1;
                for (int j = 0; j < size; j ++) {
                    if (!mark[j] && player[j].isVIP && player[j].come <= tmin) {
                        vip = j;
                        break; // 找到就退出循环
                    }
                }
                if (vip != -1) { // 如果有已经到了的VIP，把cur设为该VIP
                    cur = vip;
                }
            }
            if (player[cur].come <= tmin) { // 如果已经到了
                player[cur].serve = tmin;
                table[idx].end += player[cur].time;
            }
            else { // 如果还没到
                player[cur].serve = player[cur].come;
                table[idx].end = player[cur].come + player[cur].time;
            }
        }
        else { // 如果不是VIP房间
            if (player[cur].come <= tmin) { // 如果已经到了，不管是不是VIP都先服务
                player[cur].serve = tmin;
                table[idx].end += player[cur].time;
            }
            else { // 如果没到，需要判断一下未来第一个到的人是不是VIP
                if (!player[cur].isVIP) { // 如果不是VIP，就不用考虑
                    player[cur].serve = player[cur].come;
                    table[idx].end = player[cur].come + player[cur].time;
                }
                else { // 如果是VIP，就需要考虑VIP到的时候有没有VIP房间
                    int vip = -1;
                    for (int i = 1; i <= k; i ++) {
                        if (table[i].isVIP && table[i].end <= player[cur].come) { // 如果VIP玩家到达时，已经有VIP房空闲
                            vip = i;
                            break; // 找到就退出循环，以保证编号最小
                        }
                    }
                    if (vip != -1) { // 如果有VIP房空闲，则把找到的VIP房分给该VIP玩家
                        idx = vip;
                    }
                    else { // 如果没有，则把当时空闲房间中编号最小的分给该VIP玩家
                        for (int i = 1; i <= k; i ++) {
                            if (table[i].end <= player[cur].come) {
                                idx = i;
                                break; // 找到就退出循环，以保证编号最小
                            }
                        }
                    }
                    player[cur].serve = player[cur].come;
                    table[idx].end = player[cur].come + player[cur].time;
                }
            }
        }
        mark[cur] = true;
        table[idx].cnt ++;
//        printf("%d ", idx);
//        output(player[cur].come);
//        printf("\n");
    }
    sort(player, player + size, cmp2); // 按服务时间排序
    for (int i = 0; i < size; i ++) {
        output(player[i].come);
        printf(" ");
        output(player[i].serve);
        double wt = (player[i].serve - player[i].come) / 60.0;
        printf(" %.f\n", round(wt)); // 秒转换成分，四舍五入
    }
    for (int i = 1; i <= k; i ++) {
        if (i != 1) {
            printf(" ");
        }
        printf("%d", table[i].cnt);
    }
    return 0;
}

```

