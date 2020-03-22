# [1034 Head of a Gang (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805456881434624)

- 并查集处理连通分量
- 使用一个`map`和一个`string`数组来转换`name`字符串和`ID`
- 使用`map`存储二元结果
- 使用`iterator`遍历时可以直接写`auto`
- 错误
  - 测试点3，段错误
    - 数组开得不够大：1000条通话记录可能有2000个人
  - 测试点3，答案错误
    - 数组开大了，但初始化的范围还是`n`（害我debug了一个下午……气死了！）

### AC代码

```c++
#include <iostream>
#include <vector>
#include <map>
using namespace std;

int n, k, size;
int ufind[2010]; // 并查集
int weight[2010]; // 点的权重
map<string, int> dict; // 使字符串name与整数id对应
map<string, int> res; // 记录结果
string id2name[2010]; // 根据id查找name

int getId (string s) { // 获取dict中name对应的id
    int id;
    if (dict.find(s) != dict.end()) {
        id = dict[s];
    }
    else { // 如果dict中没有
        id = dict.size();
        dict[s] = id; // add new to dict
        id2name[id] = s; // add new to id2name
    }
    return id;
}

int findRoot (int id) { // 查找集合的根，含压缩路径
    if (ufind[id] == -1) {
        return id;
    }
    else {
        int tmp = findRoot(ufind[id]);
        ufind[id] = tmp;
        return tmp;
    }
}

int main () {
    scanf("%d %d", &n, &k); // 处理输入数据
    for (int i = 0; i < 2 * n; i ++) { // 初始化并查集
        ufind[i] = -1;
    }
    for (int i = 0; i < 2 * n; i ++) { // 初始化weight
        weight[i] = 0;
    }
    for (int i = 0; i < n; i ++) {
        string s1, s2;
        int w;
        cin >> s1 >> s2 >> w;
        int id1 = getId(s1);
        int id2 = getId(s2);
        weight[id1] += w; // 处理weight
        weight[id2] += w;
        id1 = findRoot(id1); // 处理并查集数据
        id2 = findRoot(id2);
        if (id1 != id2) {
            ufind[id1] = id2;
        }
    }
    size = dict.size(); // 人数
    for (int i = 0; i < size; i ++) {
        if (ufind[i] == -1) { // 当i为集合的根，查找该集合的所有点
            int cnt = 0; // 点个数
            int we = 0; // 点的总权重
            int head = i, maxw = 0;
            for (int j = 0; j < size; j ++) {
                if (findRoot(j) == i) { // 同根则同集合
                    cnt ++;
                    we += weight[j];
                    if (weight[j] > maxw) { // 寻找weight最大的head
                        maxw = weight[j];
                        head = j;
                    }
                }
            }
            if (cnt > 2 && we > k * 2) { // we是两倍总边权重
                res[id2name[head]] = cnt;
            }
        }
    }
    cout << res.size() << endl;
    for (auto it = res.begin(); it != res.end(); it ++) {
        cout << it->first << " " << it->second << endl;
    }
    return 0;
}
```

