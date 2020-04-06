# [1098 Insertion or Heap Sort (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805368847187968)

- 判断是插入排序还是堆排序
- 复习了一下堆排序，自己写了一个，debug好久=。=

### AC代码

```c++
#include <iostream>

int n;
int one[105], two[105];
int heap[105], size = 0;

void swap(int &a, int &b) {
    int tmp = a;
    a = b;
    b = tmp;
}

int hs(int idx) { // 将当前元素与较大的孩子节点交换，返回交换的孩子节点，如果没有交换则返回-1
    if (idx * 2 + 1 < size && heap[idx] < heap[idx * 2 + 1] && ((idx * 2 + 2 >= size) || heap[idx * 2 + 2] < heap[idx * 2 + 1]) ) {
        swap(heap[idx], heap[idx * 2 + 1]);
        return idx * 2 + 1;
    }
    else if (idx * 2 + 2 < size && heap[idx] < heap[idx * 2 + 2] && heap[idx * 2 + 1] < heap[idx * 2 + 2]) {
        swap(heap[idx], heap[idx * 2 + 2]);
        return idx * 2 + 2;
    }
    return -1;
}

void HeapSort() {
    for (int i = 0; i < n; i ++) { // 建立堆
        heap[size ++] = one[i]; // 插入节点
    }
    for (int i = (size - 2) / 2; i >= 0; i --) {
        int idx = i;
        while (idx != -1) { // 向下调整交换
            idx = hs(idx);
        }
    }
    while (1) {
        bool found = true;
        for (int i = 0; i < n; i ++) { // 判断当前堆排序与two是否相等
            if (heap[i] != two[i]) {
                found = false;
                break;
            }
        }
        swap(heap[0], heap[size - 1]); // 堆排序，交换第一个节点与最后一个节点
        size --; // 堆元素个数减一
        int idx = 0; // 向下调整交换
        while (idx != -1) {
            idx = hs(idx);
        }
        if (found) {
            break;
        }
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) {
        scanf("%d", &one[i]);
    }
    for (int i = 0; i < n; i ++) {
        scanf("%d", &two[i]);
    }
    bool isI = true;
    for (int i = 0; i < n - 1; i ++) {
        if (two[i] > two[i + 1]) {
            for (int j = i + 1; j < n; j ++) {
                if (one[j] != two[j]) {
                    isI = false;
                    break;
                }
            }
            if (isI) {
                int tmp = two[i + 1];
                for (int j = 0; j < i; j ++) {
                    if (two[j] > two[i + 1]) {
                        for (int k = i + 1; k > j; k --) {
                            two[k] = two[k - 1];
                        }
                        two[j] = tmp;
                        break;
                    }
                }
            }
            break;
        }
    }
    if (isI) {
        printf("Insertion Sort\n");
        for (int i = 0; i < n; i ++) {
            if (i != 0) {
                printf(" ");
            }
            printf("%d", two[i]);
        }
        printf("\n");
    }
    else {
        printf("Heap Sort\n");
        HeapSort();
        for (int i = 0; i < n; i ++) {
            if (i != 0) {
                printf(" ");
            }
            printf("%d", heap[i]);
        }
        printf("\n");
    }
    
    return 0;
}

```

