# [1095 Cars on Campus (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805371602845696)

- 模拟停车系统查询，做起来有点麻烦
- 筛选出配对的出入记录：将记录先按名字再按时间排序，使用一个栈筛选配对
- 分别记录一辆车的出入记录（车辆可重复）和一辆车的停放总时间（车辆不重复）

### AC代码

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <stack>
#include <map>
using namespace std;

struct Car {
    string id;
    int in, out;
    int total;
};

struct Record {
    string id;
    int time;
    bool isIn;
};

struct Total {
    string id;
    int time;
};

int n, k;
Record rec[10005];
vector<Car> car;
map<string, int> stoid;
Total total[10005];

bool cmp1(Record a, Record b) {
    if (a.id < b.id) {
        return true;
    }
    else if (a.id == b.id && a.time < b.time) {
        return true;
    }
    else {
        return false;
    }
}

bool cmp2(Car a, Car b) {
    return a.in < b.in;
}

bool cmp3(Total a, Total b) {
    if (a.time > b.time || (a.time == b.time && a.id < b.id)) {
        return true;
    }
    else {
        return false;
    }
}

int main() {
    scanf("%d %d", &n, &k);
    for (int i = 0; i < n; i ++) { // 处理记录数据
        char s1[10], s2[5];
        int hh, mm, ss;
        scanf("%s %d:%d:%d %s", s1, &hh, &mm, &ss, s2);
        string s = s2;
        rec[i].id = s1;
        rec[i].time = hh * 3600 + mm * 60 + ss;
        rec[i].isIn = s == "in" ? true : false;
    }
    sort(rec, rec + n, cmp1); // 先按id，再按time排序
    stack<Record> sta;
    for (int i = 0; i < n; i ++) { // 寻找配对，把配对成功的加入car
        if (rec[i].isIn) {
            if (!sta.empty()) {
                sta.pop();
            }
            sta.push(rec[i]);
        }
        else if (!sta.empty() && rec[i].id == sta.top().id) {
            Car tmp;
            tmp.id = rec[i].id;
            tmp.in = sta.top().time;
            tmp.out = rec[i].time;
            tmp.total = tmp.out - tmp.in;
            car.push_back(tmp);
            if (stoid.find(tmp.id) == stoid.end()) { // 计算每辆车的总时间，计入total数组，使用map实现hash查找
                int id = stoid.size();
                stoid[tmp.id] = id;
                total[id].id = tmp.id;
                total[id].time = tmp.total;
            }
            else {
                total[stoid[tmp.id]].time += tmp.total;
            }
            sta.pop();
        }
    }
    sort(car.begin(), car.end(), cmp2); // 按进入时间排序
    for (int i = 0; i < k; i ++) {
        int hh, mm, ss;
        scanf("%d:%d:%d", &hh, &mm, &ss);
        int sum = hh * 3600 + mm * 60 + ss;
        int cnt = 0;
        for (int j = 0; j < car.size(); j ++) {
            if (car[j].in > sum) { // 如果已经超过当前时间，查找结束
                break;
            }
            if (car[j].out > sum) { // 如果离开时间晚于当前时间，数量加一
                cnt ++;
            }
        }
        printf("%d\n", cnt);
    }
    sort(total, total + stoid.size(), cmp3); // 按总计时间和id排序
    int maxt = total[0].time;
    for (int i = 0; i < stoid.size(); i ++) {
        if (total[i].time < maxt) {
            break;
        }
        printf("%s ", total[i].id.c_str());
    }
    printf("%02d:%02d:%02d\n", maxt / 3600, (maxt % 3600) / 60, (maxt % 3600) % 60);
    return 0;
}

```

