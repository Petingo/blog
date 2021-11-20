---
title: const 放在 member function 的後面
date: 2018-06-01 10:36:31
tags: C++
---
### const 放前面與後面的意義
* 放前面：代表回傳值是 const 的。
``` c++
const int foo() { }
```
* 放後面：僅出現在成員函數，代表<b>函數不會改變呼叫者的值</b>；目的是確保 const 的物件不會因為呼叫了某個 function 而變化。
``` c++
int foo() const { }
```
光文字敘述可能不太清楚，下面來舉栗子。

<!--more-->

### 例子
我宣告了一個 `class Dick` ，裡面只有 `int length` 代表的長度。
因為ㄐㄐ會喵喵叫跟啪啪啪，所以另外有兩個 function `meow(const Dick& b)` 和 `fuck()`，其中 `meow` 裡面的 `const Dick& b` 會呼叫 `fuck()`。

先來試試看如果讓 const 的 b 呼叫非 const 的 fuck 會怎樣：

``` c++
#include <bits/stdc++.h>
using namespace std;
class Dick {
    int length;

  public:
    Dick(int length = 30) {
        this->length = length;
    }
    int meow(const Dick& b) {
        return fuck() + b.fuck(); // const 的 b call 了非 const 的 fuck()
    }
    // 非 const 的 fuck()
    int fuck() {
        return length + 5;
    }
};

int main() {
    Dick myDick, hisDick;
    cout << myDick.meow(hisDick) << endl;
}
```
編譯器馬上噴錯
```
error: passing 'const Dick' as 'this' argument discards qualifiers [-fpermissive]
   int meow(const Dick& b) { return fuck() + b.fuck(); }
                                                    ^
```
把 const 加上去：
``` c++
#include <bits/stdc++.h>
using namespace std;
class Dick {
    int length;

  public:
    Dick(int length = 30) {
        this->length = length;
    }
    int meow(const Dick& b) {
        return fuck() + b.fuck(); // const 的 b call 了 const 的 fuck()
    }
    // const 的 fuck()
    int fuck() const {
        return length + 5;
    }
};

int main() {
    Dick myDick, hisDick;
    cout << myDick.meow(hisDick) << endl;
}
```
恩，成功印出 70！

* * *

再來試試看如果在一般的函數後面加上 const 會怎樣：
``` c++
int woof() const { return 55555; }
```
編譯器噴錯：
```
error: non-member function 'int woof()' cannot have cv-qualifier
int woof() const { return 55555; }
           ^~~~~
```

### 寫在後面
記得第一次遇到這個問題是為了要用 priority queue 而重載 `operator<()`，必須讓它 const 否則會報錯。
至於後來幾次應該都是為了傳 const reference 的參數進 function（也就是範例的形式）。
說來慚愧，朋友今天問了我這個問題，雖然也查過、寫過一兩次，但一不小心還是忘了 Orz
stackoverflow 上的[這篇](https://stackoverflow.com/questions/4059932/what-is-the-meaning-of-a-const-at-end-of-a-member-function)講的還蠻清楚的，但相關的中文資源到底在供三小我也是看沒有，於是寫了這篇，希望我講的夠直白明瞭。
