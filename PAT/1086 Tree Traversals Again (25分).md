# [1086 Tree Traversals Again (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805380754817024)

- 根据栈实现中序遍历的过程可以得到唯一的一棵二叉树，输出这棵二叉树的后序遍历
- 用`cur`指针指向下一个`push`节点的根节点
  - 当实现`push`操作时，`cur`为`push`的节点
  - 当实现`pop`操作时，`cur`为`pop`前的栈顶节点

### AC代码

```c++
#include <iostream>
#include <stack>
using namespace std;

struct TN {
    int left;
    int right;
};

int n;
TN list[35];

bool first = true;
void postorder(int node) {
    if (list[node].left != -1) {
        postorder(list[node].left);
    }
    if (list[node].right != -1) {
        postorder(list[node].right);
    }
    if (!first) {
        printf(" ");
    }
    printf("%d", node);
    first = false;
}

int main() {
    cin >> n;
    stack<TN*> sta;
    int root = -1;
    TN* cur = NULL;
    for (int i = 0; i < n * 2; i ++) {
        string s;
        cin >> s;
        if (s == "Push") {
            int v;
            cin >> v;
            if (root == -1) { // 根节点
                root = v;
            }
            TN tn; // 创建新节点
            tn.left = -1;
            tn.right = -1;
            list[v] = tn;
            if (cur != NULL) { 
                if (cur->left == -1) {
                    cur->left = v;
                }
                else if (cur->right == -1) {
                    cur->right = v;
                }
            }
            cur = &list[v];
            sta.push(cur);
        }
        else if (s == "Pop") {
            cur = sta.top();
            sta.pop();
        }
    }
    postorder(root);
    return 0;
}

```

