---
title: UVa-136 Ugly Numbers
date: 2018-12-09 17:51:33
tags:
- C++
- STL
- 題解
---
尋找第 1500「醜數」；所謂「醜數」為質因數只有 2 或 3 或 5 的數。
利用 `priority_queue` 每次把最當前最小的值拿出來，乘 2、3、5 再丟回去，並利用 `set` 紀錄是否已經丟過。
<!--more-->
```c++
#include <bits/stdc++.h>
using namespace std;
int main() {
  set<long long> s;
  priority_queue<long long, vector<long long>, greater<long long>> pq;

  pq.push(1);
  s.insert(1);

  int cnt = 0;
  long long tmp;
  while (cnt < 1500) {
    tmp = pq.top();
    pq.pop();

    int mul[] = {2, 3, 5};
    for (int i = 0; i < 3; i++) {
      long long meow = tmp * mul[i];
      if (s.find(meow) == s.end()) {
        pq.push(meow);
        s.insert(meow);
      }
    }
    cnt++;
  }

  cout << "The 1500'th ugly number is " << tmp << "." << endl;
  return 0;
}
```