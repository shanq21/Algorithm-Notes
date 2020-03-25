# [1024 Palindromic Number (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805476473028608)

- 高精度整数，构造`bigInt`类（只是为了练手，其实写字符串加法更快= =）

### AC代码

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

class bigInt {
    int digit[1000];
    int size;
public:
    bigInt () {
        size = 0;
        for (int i = 0; i < 1000; i ++) {
            digit[i] = 0;
        }
    }
    void set(string s) {
        while (s.size() >= 4) {
            int pos = s.size() - 4;
            digit[size ++] = stoi(s.substr(pos));
            s.erase(pos);
        }
        if (!s.empty()) {
            digit[size ++] = stoi(s);
        }
    }
    bigInt operator + (const bigInt &A) const {
        bigInt ret;
        int carry = 0;
        for (int i = 0; i < A.size || i < size; i ++) {
            int tmp = A.digit[i] + digit[i] + carry;
            ret.digit[ret.size ++] = tmp % 10000;
            carry = tmp / 10000;
        }
        if (carry != 0) {
            ret.digit[ret.size ++] = carry;
        }
        return ret;
    }
    string str () {
        string ret = "";
        for (int i = size - 1; i >= 0; i --) {
            if (i != size - 1) {
                string tmp = to_string(digit[i]);
                while (tmp.size() < 4) {
                    tmp = "0" + tmp;
                }
                ret += tmp;
            }
            else {
                ret += to_string(digit[i]);
            }
        }
        return ret;
    }
};

bool isPanlindromic (bigInt num) {
    string s = num.str();
    string copy = s;
    reverse(s.begin(), s.end());
    return copy == s;
}

int main () {
    string s;
    int k;
    cin >> s >> k;
    bigInt num;
    num.set(s);
    int step = 0;
    for (int i = 0; i < k; i ++) {
        if (isPanlindromic(num)) {
            break;
        }
        else {
            string tmps = num.str();
            reverse(tmps.begin(), tmps.end());
            bigInt tmpn;
            tmpn.set(tmps);
            num = num + tmpn;
            step ++;
//            cout << num.str() << endl;
        }
    }
    cout << num.str() << endl << step << endl;
    return 0;
}

```

