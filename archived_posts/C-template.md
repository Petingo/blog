---
title: C++ template
date: 2018-12-04 14:29:14
tags: C++
---

假設我們需要一個求兩變數相加的函示；而變數可能是整數或浮點數；我們可以：

```c++
int sum(int a, int b) {
    return a + b;
}

float sum(float a, float b) {
    return a + b;
}

int main() {
    float a = 3.3, b = 4.3;
    cout << sum(a, b) << endl;

    int c = 3, d = 4;
    cout << sum(c, d) << endl;
}
```
<!--more--->
```
> 7.6
> 7
```

可是這樣要寫兩次很麻煩，這時候就可以使用 template
```c++
template <typename T> //或 template <class T>
T sum(T a, T b) {
    return a + b;
}

int main() {
    float a = 3.3, b = 4.3;
    cout << sum(a, b) << endl;

    int c = 3, d = 4;
    cout << sum(c, d) << endl;
}
```

```
> 7.6
> 7
```

至於 ```<typename T> ``` 跟 ```<class T>``` 是沒有差別的（但在某些 compiler 會報錯）；據說原本設計得時候是想說，使用 class 可以少一個關鍵字的使用，但後來覺得不太直觀，才又加上 typename 這個關鍵字。