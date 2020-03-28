# [1022 Digital Library (30分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805480801550336)

- 很简单的一道题，不需要查找优化，建立`book`结构体，注意处理字符输入即可

### AC代码

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

struct Book {
    int id;
    string title;
    string author;
    string keywords[5];
    int kwn; // 关键字的个数
    string publisher;
    int year;
};

int n, m;
Book book[10001];

bool cmp (Book a, Book b) {
    return a.id < b.id;
}

int main () {
    cin >> n;
    for (int i = 0; i < n; i ++) { // 输入book数据
        cin >> book[i].id;
        getchar(); // cin缓冲区有一个'\n'
        getline(cin, book[i].title);
        getline(cin, book[i].author);
        string s;
        getline(cin, s); // 处理keywords
        int cnt = 0;
        for (int j = 0; j < 5; j ++) {
            int pos = s.find(" "); // 寻找空格的位置
            book[i].keywords[j] = s.substr(0, pos);
            cnt ++;
            if (pos == string::npos) {
                break;
            }
            s.erase(0, pos + 1);
        }
        book[i].kwn = cnt;
        getline(cin, book[i].publisher);
        cin >> book[i].year;
    }
    sort(book, book + n, cmp);
    cin >> m;
    getchar(); // cin缓冲区有一个'\n'
    for (int i = 0; i < m; i ++) { // 输入query数据
        string s;
        getline(cin ,s);
        printf("%s\n", s.c_str());
        int pos = s.find(":");
        int num = stoi(s.substr(0, pos));
        string str = s.substr(pos + 2);
        bool found = false;
        if (num == 1) { // 如果查找title
            for (int i = 0; i < n; i ++) {
                if (str == book[i].title) {
                    found = true;
                    printf("%07d\n", book[i].id);
                }
            }
        }
        else if (num == 2) { // 如果查找author
            for (int i = 0; i < n; i ++) {
                if (str == book[i].author) {
                    found = true;
                    printf("%07d\n", book[i].id);
                }
            }
        }
        else if (num == 3) { // 如果查找keywords
            for (int i = 0; i < n; i ++) {
                for (int j = 0; j < book[i].kwn; j ++) {
                    if (str == book[i].keywords[j]) {
                        found = true;
                        printf("%07d\n", book[i].id);
                        break;
                    }
                }
            }
        }
        else if (num == 4) { // 如果查找publisher
            for (int i = 0; i < n; i ++) {
                if (str == book[i].publisher) {
                    found = true;
                    printf("%07d\n", book[i].id);
                }
            }
        }
        else if (num == 5) { // 如果查找year
            int year = stoi(str);
            for (int i = 0; i < n; i ++) {
                if (year == book[i].year) {
                    found = true;
                    printf("%07d\n", book[i].id);
                }
            }
        }
        if (!found) {
            printf("Not Found\n");
        }
    }
    return 0;
}

```

