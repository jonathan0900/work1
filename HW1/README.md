# 41141128

作業一
## Problem 1:
## 解題說明

Ackermann 函數是一個著名的遞迴例子，它不屬於原始遞迴範疇，其特點是增長速度非常快，因此適合用來測試遞迴與堆疊的概念。

$$
A(m, n) = \begin{cases}
n + 1 & \text{if } m = 0 \\
A(m - 1, 1) & \text{if } n = 0 \\
A(m - 1, A(m, n - 1)) & \text{otherwise}
\end{cases}
$$


## 解題策略
1.遞迴思維建立遇到 m=0就回傳 n+1，是最底層，遇到 n=0，則往左邊減一層 m，從 n=1 重啟，其他情況會展開成「雙層遞迴」，這是最困難處。  

2.非遞迴策略用stack（堆疊）模擬系統 call stack，每次記錄 (m,n) 當前狀態，根據條件把下一步推入 stack，特別是 A(m-1, A(m,n-1))，需要等到內部 A(m,n-1) 得出值後，才能處理外層 A(m-1,x)。\

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
## 效能分析
時間複雜度:  

遞迴：非常高，非多項式，屬於超指數級，O(A(m,n))onential）

非遞迴：模擬堆疊行為，但仍接近上述時間複雜度，O(A(m,n))

空間複雜度:

遞迴：最壞可能為，O(A(m,n))

非遞迴：控制在手動堆疊，O(A(m,n))

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
1.遞迴版本

g++ -o ackermann ackermann.cpp

./ackermann

2.非遞迴版本

g++ -o ackermann.exe ackermann.cpp

## Problem 2:
## 解題說明
給定一個集合S，請遞迴計算它的冪集合，也就是所有可能子集合的集合。

例如：

若 S = (a, b, c)

則(S) = { (), (a), (b), (c), (a,b), (a,c), (b,c), (a,b,c) }

若 S = {a,b}

則(S) = {}, {a}, {b}, {a,b}
## 解題策略
1.每個元素都有兩種選擇：包含進子集合 或 不包含進子集合

2.使用遞迴，每層決定是否加入當前元素。
## 程式實作
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
子集合數量為 2^n，因為每個元素都有包含/不包含兩種狀態。

時間複雜度：O(2^n)

空間複雜度：O(n)（每次遞迴需要儲存目前的子集合）

## 測試與驗證
測試案例 s：{a,b}

{ }
{ b }
{ a }
{ a b }
## 編譯與執行指令
g++ -o subsets.exe subsets.cpp

subsets.exe















