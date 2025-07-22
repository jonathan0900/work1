# 41141128

作業一
## 解題說明
Problem 1
Ackermann 函數是一個著名的遞迴例子，它不屬於原始遞迴範疇，其特點是增長速度非常快，因此適合用來測試遞迴與堆疊的概念。
Problem 2
冪集合指的是所有子集合的集合，包含空集合與本身。可以使用「包含 / 不包含」的方式用遞迴解。
ex：
S = {a,b}
powerset = {}, {a}, {b}, {a,b}

## 解題策略
Problem 1
1.遞迴思維建立
遇到 m=0 就回傳 n+1，是最底層，遇到 n=0，則往左邊減一層 m，從 n=1 重啟，其他情況會展開成「雙層遞迴」，這是最困難處。
2：非遞迴策略
用 stack（堆疊）模擬系統 call stack，每次記錄 (m,n) 當前狀態，根據條件把下一步推入 stack，特別是 A(m-1, A(m,n-1))，需要等到內部 A(m,n-1) 得出值後，才能處理外層 A(m-1,x)。
Problem 2
遞迴遍歷所有選擇的可能性，每個元素都有兩種選擇：取或不取。

## 程式實作
Problem 1
遞迴版本
```cpp
#include <iostream>
using namespace std;

int ackermann(int m, int n) {
    if (m == 0) return n + 1;
    else if (n == 0) return ackermann(m - 1, 1);
    else return ackermann(m - 1, ackermann(m, n - 1));
}
```
非遞迴版本
```cpp
#include <stack>
int ackermann_iterative(int m, int n) {
    stack<int> stk;
    stk.push(m);
    while (!stk.empty()) {
        m = stk.top(); stk.pop();
        if (m == 0) {
            n += 1;
        } else if (n == 0) {
            stk.push(m - 1);
            n = 1;
        } else {
            stk.push(m - 1);
            stk.push(m);
            n = n - 1;
        }
    }
    return n;
}
```
Problem 2
```cpp
#include <vector>
#include <string>
#include <iostream>
using namespace std;

void generate_powerset(vector<string>& set, vector<string>& current, int index, vector<vector<string>>& result) {
    if (index == set.size()) {
        result.push_back(current);
        return;
    }

    // 不選擇當前元素
    generate_powerset(set, current, index + 1, result);

    // 選擇當前元素
    current.push_back(set[index]);
    generate_powerset(set, current, index + 1, result);
    current.pop_back();
}
```

## 效能分析
Problem 1
Time complexity:
遞迴：非常高，非多項式，屬於超指數級（hyper-exponential）。
非遞迴：模擬堆疊行為，但仍接近上述時間複雜度。
Space complexity:
遞迴：O(stack depth)，最壞可能為 O(n^(m+1))
非遞迴：O(stack size)，控制在手動堆疊。

Problem 2
Time complexity: 每個元素有兩種選擇。
Space complexity: 所有子集合的儲存空間。


## 測試與驗證
Problem 1
```cpp
int main() {
    cout << "A(1,2) = " << ackermann(1,2) << endl;
    cout << "A(3,2) = " << ackermann_iterative(3,2) << endl;
    return 0;
}
```

A(1, 2) = 4
A(3, 2) = 29

Problem 2

## 效能量測
Problem 1

```cpp
#include <chrono>
auto start = chrono::high_resolution_clock::now();
int result = ackermann_iterative(3,4);
auto end = chrono::high_resolution_clock::now();
cout << "Result: " << result << endl;
cout << "Time: " << chrono::duration<double>(end - start).count() << " sec" << endl;
```
Problem 2

## 心得討論
Ackermann 函數展示了遞迴的極限，許多現代語言都可能 Stack Overflow。我學到模擬遞迴與堆疊可以讓演算法具備更強的控制與效能彈性。這對未來解決複雜遞迴問題是一大收穫。






















