---
title: 數位相機理論與實務期末考
date: 2019-11-04 10:09:36
tags: 筆記
---

### 一、名詞解釋
#### 1. Uniform Color Space
將色彩用 hue, lightness, chroma 三個數值來表示。該色彩空間的好處在於兩顏色的色差會與其座標的距離相等，實務上會比較好操作。

![](https://i.imgur.com/oJMXFr5.png)

#### 2. 3A
- Auto Focus：自動對焦——將焦距調整到正確的位置，使目標物清晰。
- Auto White Balance：自動白平衡——去除環境光的影響，還原物體的真實色彩。
- Auto Exposure：自動曝光——藉由調控光圈、快門、ISO 值還原環境的光強度。

<!--more-->

#### 3. Black Body Radiation
處於熱力學平衡狀態的黑體發出的輻射。所謂黑體是一個理想化的模型，可以吸收所有照射於表面的電磁波，在不同溫度下會釋放出不同的光譜，呈現不同的色彩，而此溫度（K）－色彩的對應關係即是色溫。

#### 4. Independent color space
利用色彩模型和色域將顏色定義，使得不同設備之間可以藉由某種轉換而呈現相同的顏色，不會因為機器的不同而產生色差。

### 二、簡述 DSC 動作原理，請以方塊圖說明之。
![](https://i.imgur.com/eTJVl5g.png)
![](https://i.imgur.com/QqEb5b4.png)

- 光線經過鏡頭的透鏡、反射鏡後，攝影師可以從觀景窗看到取景的內容。另外會有一部分的光線被反射到 Autofocus module，讓相機進行自動對焦之類的操作。
- 快門擊發時，反射鏡會縮下去、快門會打開，光線經過透鏡以後會先經過 low pass filter、micro lens array、color filter array，最後成像在感光元件上面，經過信號調整、數位轉換、色彩處理（解馬賽克、白平衡等等）、影像增益（和壓縮），最後儲存成照片。

### 三、簡述 DSC 如何進行色彩校正及其原理。
在標準光源下先用 DSC 拍攝 Macbeth Color Checker 之類的色校卡取得色塊值，再利用色彩檢測儀拍攝獲得該色彩的參考值（或直接利用廠商提供的）；將其轉換到標準空間以後，再利用兩者計算色校參數，未來拍攝的時候就可以直接將此參數套用在拍攝的影像上校正色彩。其原理是利用轉換矩陣使 DSC 拍攝下來的數值可以映射到對應的參考值上面，還原正確的色彩。

![](https://i.imgur.com/m6S8L48.png)
![](https://i.imgur.com/u6uLbfc.png)


### 四、簡述 White Balance
一個物體呈現的顏色可以視作「物體本身的顏色＋物體反射環境光的顏色」，白平衡目的在於去除環境光對物體顏色造成的影響，還原物體原始的顏色，常用的做法有 Gray World 、Retinex 兩種。
- Gray World：
Gary World 的理論認為色彩是均勻分布的，因此將 RGB 平均以後應該會接近灰色。實作上先定義參考灰(G)，計算原圖 RGB 各自的平均(G')，計算 G/G' 的比值，然後根據此值調整 RGB 的數值。

- Retinex：
Retinex 的理論認為原始影像(S)、物體(B)、環境光(L) 的關係為 log(S) = log(B) + log(L)，其中環境光被認為是原始影像中低頻資訊；實務上藉由高斯模糊得到低頻影像(D)，再利用 log(S) - log(D) 得到高頻影像(G)，對 G 取反對數 exp(G) 即可得到正確顏色的影像 B。

### 五、如何評估數位相機影像品質？說明如何實踐
影像品質可由以下 8 點來評估，旨在還原真實場景的顏色、細節、光線對比強度等等。
1. Color Reproduce (CR) 色彩再現性
2. Color Rendering Index 演色性指數
3. Auto White Balance 自動白平衡
4. MTF (解析度）
5. SNR （雜訊比）
6. Uniformity （均勻度）
7. Dynamic range （動態範圍）
8. Veiling Flare （炫光）
#### 色彩再現性
還原色彩的程度，可以藉由計算色校卡照片與實驗室測得的色彩之間的差異 ΔEab 來評估其色彩再現性。

#### 解析度
![](https://i.imgur.com/4eI3ubx.png)
解析度為相機對於細節的還原能力，實務作法為拍攝 ISO 12233 chart 以後，再藉由 MTF 值分析，若在細節處黑線和白線糊在一起，表示解析度不夠，MTF 值會降低。
![](https://i.imgur.com/VArMBdN.png)

而解析度又可再細分為：
1. 垂直解像力
2. 水平解像力
3. 中央對焦區
4. 對角線解像力
5. 對比指示

#### Uniformity
在均勻的光線下拍攝單色的內容，根據此公式即可算出均勻度，PSNR 越高表示品質越佳：
![](https://i.imgur.com/IgBs4R5.png)
