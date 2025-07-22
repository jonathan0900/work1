# 41141128

作業一
## 解題說明
**Problem 1:**

Ackermann 函數是一個著名的遞迴例子，它不屬於原始遞迴範疇，其特點是增長速度非常快，因此適合用來測試遞迴與堆疊的概念。

A(m, n) = 
    if m = 0:           n + 1
    else if n = 0:      A(m - 1, 1)
    else:               A(m - 1, A(m, n - 1))


## 解題策略
遞迴思維建立
遇到 m=0 就回傳 n+1，是最底層，遇到 n=0，則往左邊減一層 m，從 n=1 重啟，其他情況會展開成「雙層遞迴」，這是最困難處。
非遞迴策略
用 stack（堆疊）模擬系統 call stack，每次記錄 (m,n) 當前狀態，根據條件把下一步推入 stack，特別是 A(m-1, A(m,n-1))，需要等到內部 A(m,n-1) 得出值後，才能處理外層 A(m-1,x)。\
**Problem 2:**
遞迴遍歷所有選擇的可能性，每個元素都有兩種選擇：取或不取。

## 程式實作
遞迴版本:
```cpp
#include <iostream>
using namespace std;

int ackermann(int a, int b) {
    if (a == 0) return b + 1;
    else if (b == 0) return ackermann(a - 1, 1);
    else return ackermann(a - 1, ackermann(a, b - 1));
}

int main() {
    int a = 0, b = 1;
    cout << "A(" << a << "," << b << ") = " << ackermann(a, b) << endl;
    return 0;
}
```
非遞迴版本:
```cpp
#include <iostream>
#include <stack>
using namespace std;

int P1(int a, int b) {
    stack<int> stk;

    stk.push(a);
    while (!stk.empty()) {
        a = stk.top();
        stk.pop();

        if (a == 0) {
            b = b + 1;
        } else if (b == 0) {
            stk.push(a - 1);
            b = 1;
        } else {
            stk.push(a - 1);
            stk.push(a);
            b = b - 1;
        }
    }

    return b;
}

int main() {
    int a , b ;
    cin>>a>>b;
    int result = P1(a, b);
    cout << "P2(" << a << "," << b << ") = " << result << endl;

    return 0;
}
```
Problem 2
```cpp
#include <iostream>
#include <string>
using namespace std;

const int MAX = 10;  

void generate_powerset(char set[], char current[], int index, int curr_size, int n) {
    if (index == n) {
        
        cout << "{ ";
        for (int i = 0; i < curr_size; ++i) {
            cout << current[i] << " ";
        }
        cout << "}" << endl;
        return;
    }

    generate_powerset(set, current, index + 1, curr_size, n);

    current[curr_size] = set[index];
    generate_powerset(set, current, index + 1, curr_size + 1, n);
}

int main() {
    char set[] = {'a', 'b', 'c'};
    int n = 3;

    char current[MAX]; 
    generate_powerset(set, current, 0, 0, n);

    return 0;
}
```

## 效能分析
Time complexity:
遞迴：非常高，非多項式，屬於超指數級（hyper-exponential）
非遞迴：模擬堆疊行為，但仍接近上述時間複雜度。
Space complexity:
遞迴：O(stack depth)，最壞可能為 O(n^(m+1))
非遞迴：O(stack size)，控制在手動堆疊。

## 測試與驗證
**Problem 1:**
| 測試案例       | 參數(a,b)     | 預期輸出       | 實際輸出      |
| ------------- | ------------- | ------------- | ------------- |
| 測試1         | (0,1)         | 2             |2              |
| 測試2         | (1,2)         | 4             | 4             |
| 測試3         | (0,4)         | 5             | 5             |
| 測試4         | (2,3)         | 9             | 9             |
| 測試5         | (3,4)         | 125           | 125           |


**Problem 2:**
| 測試案例       | 輸入           | 預期輸出       | 實際輸出      |
| ------------- | ------------- | ------------- | ------------- |
| 測試1         | { }           | { }          |{ }         |
| 測試2         | {a}        | { }, {a}             | { }, {a}             |
| 測試3         | {a,b}         | { }, {a}, {b}, {a,b}             | { }, {a}, {b}, {a,b}             |
| 測試4         | {a,b,c}        | { }, {c}, {b}, {b,c}, {a}, {a,c}, {a,b}, {a,b,c}             | { }, {c}, {b}, {b,c}, {a}, {a,c}, {a,b}, {a,b,c}             |

## 編譯與執行指令





## 解題說明
**Problem 2:**
冪集合指的是所有子集合的集合，包含空集合與本身。可以使用「包含 / 不包含」的方式用遞迴解。
ex：
S = {a,b}
powerset = {}, {a}, {b}, {a,b}
## 解題策略
## 程式實作
## 效能分析
Time complexity: 每個元素有兩種選擇。
Space complexity: 所有子集合的儲存空間。
## 測試與驗證
## 編譯與執行指令














