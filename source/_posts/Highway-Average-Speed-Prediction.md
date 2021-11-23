---
title: 高速公路車速預測
date: 2018-12-15 00:33:43
tags: Small Project
---
## 資料集與目標

此為物聯網與資料分析的作業五，本題旨在利用高速公路交通流量資料，來預估 30 分鐘後的平均車速；下表為資料各欄位的詳細說明

| 欄位名稱             |內容介紹 | 資料格式|
| --------           | -------- | -------- |
|Timestamp	|The time時間MM/DD/YYYY HH24:MI:SS. （5 分鐘）|timestamp|
|Station ID	|高速公路站點ID （這次資料全部都是1108687）|	integer|
|Freeway	|高速公路ID （這次資料全部都是5）	|integer|
|Direction of Travel	|站點的道路通往方向（東西南北） （這次資料全部都是N）	|string|
|Station Type	|ML = Main line 主線道 	|string|
|Samples	|全部道路（lanes）5分鐘內有多少sample|	integer|
|% Observed	|全部道路之觀察數據比例	 [（觀察筆數+缺失填入筆數）/全資料數]|percentage
|Total Flow	|全部道路5分鐘內車流量 。 （少於10筆資料的使他乘以（10/好樣本數量））	（-1代表無資料）		|integer|
|Average Occupancy	|五分鐘內所有道路的平均佔有率 （某時段內偵測到車子的頻率）|	percentage|	
|Average Speed	|所有車道在5分鐘内的流量加權平均速度mph（每條道路速度每條道路流量加總 / 每條道路流量加總）	|float|
|Lane “N” Samples	|第N道路之5分鐘內sample數量	|integer|
|Lane “N” Flow	|第N道路之5分鐘內所有流量 。（少於10筆資料的使他乘以（10/樣本數量））		|integer|	
|Lane ”N” Average Occupancy	|第N道路之平均佔有率	|percentage|
|Lane ”N” Average Speed 	|第N道路之平均速度	|float|
|Lane “N” Observed	|第N道路之觀察數據比例 （觀察筆數+缺失填入筆數）/全資料數	|percentage|

<!--more-->		

## SVM
### SVM 介紹
SVM是一種可以將資料做分類的機器學習方法，如下圖所示：

![](https://i.imgur.com/sQMPxnf.png)

有黑點和白點兩群資料集，直線H1切過兩者中間、H2雖然成功分割但是距離差異太近、H3則可以成功分割且距離最大。如果資料呈現類似環狀分布，二維平面中無法直接分割，可以透過一kernel matrix將其轉換到高維平面中即可順利分割。

![](https://i.imgur.com/mMDtcCS.png)
![](https://i.imgur.com/i9LuZ4u.png)

類似的方法也可以用於迴歸分析（我們這次的作業），簡單介紹參數

 - kernel：將資料轉換到高維空間所使用的方法，這裡我們使用預設的RBF
（https://zh.wikipedia.org/wiki/%E5%BE%84%E5%90%91%E5%9F%BA%E5%87%BD%E6%95%B0%E6%A0%B8）
 - gamma：
 ![](https://i.imgur.com/JD43nfN.png)
 影響上圖中藍點和紅點在高維空間中的距離
 - C：SVR會使用一部分的資料訓練，剩下的用作測驗，C 即預測值和實際值落差過大時，要在 loss function 中給予懲罰的係數。
 - epsilon：承上，當誤差小於給定的值時便不予懲罰。

## Result & Discussion
### 結果圖
藍色點為原始資料集；綠色線為模型的預估結果
#### Score = 10.98746
![](https://i.imgur.com/6Svygrz.png)

#### Score = 14.31129
![](https://i.imgur.com/6aK3RMi.png)


### 分析與討論
#### 失敗的模型
一開始，我們直覺地認為車速與一天當中不同的時間有關；在上下班時間因為車多擁擠，導致時速下降、在凌晨時分因為車少，平均速率便會上升。於是我們將不同日子裡相同時間的速度平均，做成下列的表格（僅為示意）：
``` python
average_speed["00:00"] = 68.21
average_speed["00:05"] = 69.53
average_speed["00:10"] = 70.22
...
average_speed["23:55"] = 67.91
```
做出表格以後，再利用普通的線性回歸內插出所有時間的數值；但結果在 kaggle 上僅得到 35 分左右，可以說是奇慘無比。

#### 利用 SVR
後來我們利用時間戳記、當前速率、總流量三者作為輸入，並利用 ```sklearn``` 提供的函示庫實作。

經過幾次的試驗以後，**我們在 kaggle 拿到的兩組最好成績分別是 10.98746 和 14.31129**，兩者唯一的差別在於時間的 normalization；在 10.98746 的部分，時間是直接使用原始的 timestamp，但由於 kaggle 上的測試數據包含在訓練數據中，我們認為結果會大大的受到其影響。也就是說，我們用 1 月 1 日到 2 月 5 日的資料來訓練；假設 1 月 21 日時有突發狀況導致平均車速減少，因為時間戳記是唯一的，會受到其權重的影響，我們訓練出的 model 可能會有 over-fitting 的情況，如果利用此模型預估 5 月 20 日的車流可能會有問題。

在把資料視覺化以後，我們可以輕易的觀察出其平均速率是有週期性的變動，大約是以一個禮拜為單位。於是我們將時間戳轉換為「在一個禮拜裡的某個時間」（即 $\bmod 60*60*24*7$），並正規化（映射到 0~1 的區間）。雖然在 kaggle 上並沒有獲得比較好的分數，但我們認為這是一個比較 general 的模型，比較能夠應付不同的狀況。
