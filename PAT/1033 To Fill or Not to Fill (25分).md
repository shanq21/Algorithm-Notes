# [1033 To Fill or Not to Fill (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805458722734080)

- 一条路上有好几个加油站，每个加油站的油价不同，给出小车的油箱容量、每单位汽油能走的距离、到目的地的距离、各个加油站到起点的距离，求最便宜的加油方式，油箱初始为空
- 用贪心算法即可，具体逻辑写在代码注释中（一开始想用动态规划，傻了= =）
- 刚写完的时候结果老是差一部分，最后把数据类型全部改成`double`就过了……

```c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Station {
    double price;
    double dis;
};

double cap, total, avg;
int n;
Station sta[510];

bool cmp (Station a, Station b) {
    return a.dis < b.dis;
}

int main () {
    scanf("%lf %lf %lf %d", &cap, &total, &avg, &n);
    for (int i = 0; i < n; i ++) {
        double p; int d;
        scanf("%lf %d", &p, &d);
        sta[i].price = p;
        sta[i].dis = d;
    }
    sort(sta, sta + n, cmp);
    if (sta[0].dis != 0) { // 如果起点没有加油站
        printf("The maximum travel distance = 0.00");
        return 0;
    }
    sta[n] = {123123123.0, total};
    double money = 0, curGas = 0;
    for (int i = 0; i < n; i ++) {
        double minp = sta[i].price; // 找到价格更便宜的最近那一站
        int idx = -1;
        for (int j = i + 1; j <= n; j ++) {
            if (sta[i].dis + cap * avg < sta[j].dis) { // 如果开不到某一站，就退出查找
                if (j == i + 1) { // 如果连下一站都开不到，程序结束
                    printf("The maximum travel distance = %.2f", double(sta[i].dis) + cap * avg);
                    return 0;
                }
                break;
            }
            if (sta[j].price <= minp) {
                minp = sta[j].price;
                idx = j;
                break; // 找到就退出
            }
        }
        if (idx == -1) { // 如果没有比当前价格低的
            if (sta[i].dis + cap * avg >= total) { // 如果能直接开到终点
                money += ((total - sta[i].dis) / avg - curGas) * sta[i].price; // 把油加到恰好到终点
                break;
            }
            else { // 如果不能直接开到终点
                money += (cap - curGas) * sta[i].price; // 把油加满
                curGas = cap - (sta[i + 1].dis - sta[i].dis) / avg; // 计算到下一站时剩余的油量
            }
        }
        else { // 如果有比当前价格低的，直接开到那个站
            if (sta[i].dis + curGas * avg < sta[idx].dis) { // 如果当前的油不能支撑到那个站，要加到适量的油
                money += ((sta[idx].dis - sta[i].dis) / avg - curGas) * sta[i].price;
                curGas = 0; // 到站时油箱为空
            }
            else { // 如果油有剩余
                curGas = curGas - (sta[idx].dis - sta[i].dis) / avg; // 计算到达时的剩余量
            }
            i = idx - 1;
        }
    }
    printf("%.2f", money);
    return 0;
}

```

