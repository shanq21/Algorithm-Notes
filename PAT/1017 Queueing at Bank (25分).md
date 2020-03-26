# [1017 Queueing at Bank (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805491530579968)

- 模拟银行排队过程，计算平均等待时间
- 重点是记录每个窗口下一次空闲的时刻（可以用优先队列优化），研究对象应该是窗口而不是顾客（研究顾客会要命的= =）

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Customer {
    int time; // 总处理时间，单位s
    int arr; // 到达时刻，单位s
};

int n, k;
int size; // 在关门前到达的人数
Customer list[10001];
int win[101]; // 记录每个窗口的下一次空闲时间
int total = 0; // 总等待时间
const int inf = 123123123;

bool cmp (Customer a, Customer b) {
    return a.arr < b.arr;
}

int main () {
    scanf("%d %d", &n, &k); // 处理输入数据
    size = 0;
    for (int i = 0; i < n; i ++) {
        int hh, mm, ss, t;
        scanf("%d:%d:%d %d", &hh, &mm, &ss, &t);
        int arr = hh * 3600 + mm * 60 + ss;
        if (arr > 17 * 3600) { // 超过关门时间则舍去
            continue;
        }
        list[size].arr = arr;
        list[size].time = t * 60; // 把分转成秒
        size ++;
    }
    sort(list, list + size, cmp); // 按到达时间排序
    for (int i = 0; i < k; i ++) {
        win[i] = 8 * 3600;
    }
    for (int i = 0; i < size; i ++) { // 找到最早空闲的窗口，把当前等待队列的头头放进去，计算下一次空闲的时间
        int tmin = inf, u = -1;
        for (int j = 0; j < k; j ++) { // 找到最早空闲的窗口
            if (win[j] < tmin) {
                tmin = win[j];
                u = j;
            }
        }
        if (list[i].arr <= tmin) { // 如果空闲时已经到达
            total += tmin - list[i].arr; // 计算等待时间
            win[u] = win[u] + list[i].time; // 下一次空闲时间即当前时间加上处理时间
        }
        else { // 如果空闲时还未到达
            win[u] = list[i].arr + list[i].time; // 下一次空闲时间即到达时间加上处理时间，此时没有等待时间
        }
    }
    if (size == 0) {
        printf("0.0");
    }
    else {
        double ans = total / 60.0 / size;
        printf("%.1f", ans);
    }
    return 0;
}

```

