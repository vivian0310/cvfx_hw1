# Homework1-color-transfer
* Dataset: maps
* Original model: cycleGAN
* Other method: MUNIT

## Training cycleGAN
我們選擇的dataset為maps  
下圖為cycleGAN在training中所存的.pth檔截圖

![](https://i.imgur.com/0fKLGSP.jpg)

## Inference cycleGAN
生成的部分，我們從google map上擷取3張地圖作為inputs

1. 清大校園 (中央建築為台達館)

![](https://i.imgur.com/btnsadg.jpg) ![](https://i.imgur.com/qcQEBwc.jpg)

- cycleGAN的生成結果(右：input，左：output)

![](https://i.imgur.com/ur61etu.png)

![](https://i.imgur.com/eqfHqdV.png)

2. 隨機的台灣市區

![](https://i.imgur.com/e3LO26P.jpg) ![](https://i.imgur.com/2eROpKA.jpg)

- cycleGAN的生成結果(右：input，左：output)

![](https://i.imgur.com/5zVFxX9.png)

![](https://i.imgur.com/sL6pZvH.png)

3. 隨機的台灣市區及河流

![](https://i.imgur.com/kO4Rt3h.jpg) ![](https://i.imgur.com/nsDNYGq.jpg)

- cycleGAN的生成結果(右：input，左：output)

![](https://i.imgur.com/sRJhEn6.png)

![](https://i.imgur.com/H7DThIR.png)

## Compare with other method: MUNIT
- MUNIT架構：
![](https://i.imgur.com/y9MkvFx.png)

- MUNIT特色：  
cycleGAN的限制在於只能對兩個domain之間進行轉換，同時也需要個別的generator以及discriminator。
而在MUNIT中，一張圖像被假設是由content code與style code所合成而來。其中，content code描述的是圖像本質上的結構資訊；而style code描述的是如何表現content code所記錄的結構資訊。
其中，就同一個content code而言，我們可以藉由搭配不同的style codes來產生同一個domain不同型態的outputs，如此便克服了前述的困境─缺乏輸出圖像的多樣性。

但由於maps的style較為單一(虛擬/真實地圖)，因此清大校園的生成部分，我們以model隨機生成的style中取最接近真實的圖像為主。

## Training MUNIT

MUNIT的training過程如下，到生成之前共跑了約70萬筆iterations

![](https://i.imgur.com/LqLl9CH.jpg)
![](https://i.imgur.com/HHpqZMZ.jpg)

下圖為過程中用70萬iterations的model將test image轉換的截圖：  
(上兩圖為：input, reconstruction output,下兩圖則為model隨機style生成的output*2)
  
![](https://i.imgur.com/O58T0el.jpg)
   
## Inference MUNIT
生成的部分，我們皆使用跑70萬筆iterations的model來產生結果

1. 清大校園

![](https://i.imgur.com/btnsadg.jpg) ![](https://i.imgur.com/qcQEBwc.jpg)

- MUNIT的生成結果

![](https://i.imgur.com/d9JZoUv.jpg) ![](https://i.imgur.com/uWgYb6r.jpg)
 
2. 台灣某縣市市區

![](https://i.imgur.com/e3LO26P.jpg) ![](https://i.imgur.com/2eROpKA.jpg)

- MUNIT的生成結果

![](https://i.imgur.com/MPI2Kvo.jpg) ![](https://i.imgur.com/nVBh4px.jpg)
 
3. 台灣某縣市市區及河流

![](https://i.imgur.com/kO4Rt3h.jpg) ![](https://i.imgur.com/nsDNYGq.jpg)

- MUNIT的生成結果

![](https://i.imgur.com/md7aD8v.jpg) ![](https://i.imgur.com/Gxasuix.jpg)
 
## Analysis
- 以整體而言，我們發現MUNIT所生成的結果不如cycleGAN所生成的理想，例如：
    1. 在真實地圖轉虛擬地圖的圖片中，cycleGAN所生成的圖片其道路細緻度較MUNIT高，而在虛擬地圖轉真實地圖的結果發現，cycleGAN的建築還原度也比MUNIT完整。
    2. 在虛擬地圖轉真實地圖的結果中，cycleGAN可以將藍色部分轉換成湖，而MUNIT則否。而虛擬地圖中的黃色道路部分，clycleGAN會將其辨識為綠地而生成樹林或草地的樣子，MUNIT則是將其轉為一般的道路，這部分反而是MUNIT的效果較佳。但真正應該將綠地轉為樹林或草地的部分，MUNIT的生成效果不如cycleGAN，會將部分綠地轉為一般的道路。
    3. 此外，以真實地圖轉虛擬地圖來看，MUNIT產生的道路劃分出的街區較cycleGAN方正，但也較不貼近原本的虛擬地圖；綠地的部分，cycleGAN生成的位置較類似原本的地圖，MUNIT則在原本沒有綠地的地方額外多生成，因此綠地也是cycleGAN效果更好；湖的部分，cycleGAN雖然有生成，但位置卻不完全正確，MUNIT則是完全無法生成出來。
 
- 若以生成後的圖片色調來看，MUNIT在虛擬轉真實地圖的生成結果比較相似於真實的地圖模樣，而cycleGAN的虛擬轉真實成果則較原先的input有色彩上的落差，看起來有點接近於素描的筆觸。

- 另外，過程中我們也有拿小於70萬的model來嘗試生成，發現在50萬iterations前所訓練出的model產生的圖像中容易有模糊或波紋等不清楚的線條，而後期model的結果則與70萬的model類似，僅有細微之差。

- 以test image的截圖來看，其道路筆直且建物的劃地很方正，所以轉換的結果十分完整，猜測MUNIT對於線條規則的圖片可能擁有比較高的辨識度；若是偏不規則的地形就會複雜許多，其難度也會相對提高，因此可能使Model容易產生混淆的狀況而導致效果不佳。
 
- 真實與虛擬地圖之間的轉換，已經不只有顏色上的轉換，還包括形狀、材質等等，因此對於cycleGAN與MUNIT來說學習轉換的難度較高，這也是我們認為可能是結果不如預想的原因。
