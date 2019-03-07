# Homework1-color-transfer
* Dataset: maps
* Original model: cycleGAN
* Other method: MUNIT

## Training cycleGAN
我們選擇的dataset為maps
下圖為cycleGAN在training中所存的.pth檔截圖

![](https://i.imgur.com/0fKLGSP.jpg)

## Inference cycleGAN
我們從google map上擷取清大校園作為我們的input (中央建築為台達館)

![](https://i.imgur.com/12iMqwo.jpg)

而以下為cycleGAN的生成結果(右：input，左：output)

![](https://i.imgur.com/ur61etu.png)

![](https://i.imgur.com/eqfHqdV.png)


## Compare with other method: MUNIT
- MUNIT架構：
![](https://i.imgur.com/y9MkvFx.png)

- MUNIT特色：
cycleGAN的限制在於只能對兩個domain之間進行轉換，同時也需要個別的generator以及discriminator。
而在MUNIT中，一張圖像被假設是由兩個部分所合成而來—content code和style code。其中，content code來自於content space，描述的是圖像本質上的結構資訊，而content space是被source domain和target domain所共享的；至於style code則是來自於style space，描述的是如何表現content code所記錄的結構資訊，而source domain和target domain分別擁有各自的style space。
其中，就同一個content code而言，我們可以藉由搭配不同的style codes來產生同一個domain不同型態的outputs，如此便克服了前述的困境—缺乏輸出圖像的多樣性。

但由於maps的style較為單一(普通/衛星地圖)，因此清大校園的生成部分，我們以model隨機生成的style中取最接近真實的圖像為主。

## Training MUNIT

MUNIT的training過程如下，到生成之前共跑了約70萬筆iterations

![](https://i.imgur.com/LqLl9CH.jpg)

![](https://i.imgur.com/HHpqZMZ.jpg)

下圖為過程中用70萬iterations的model將test image轉換的截圖：
(上兩圖為：input, reconstructed input, 
  下兩圖則為model隨機style生成的output*2)
  
![](https://i.imgur.com/O58T0el.jpg)


## Inference MUNIT
我們使用跑了70萬iterations的model來生成清大校園的相對應圖片，如下:

此圖為input

![](https://i.imgur.com/Mcz6YUo.jpg)

此圖為轉換之後所生成的對應圖片(上圖對應下圖)

![](https://i.imgur.com/KIY65nQ.jpg) ![](https://i.imgur.com/f7tSwfU.jpg)


## Results
- 以整體而言，我們發現MUNIT所生成的結果不如cycleGAN所生成的理想，例如：
    1. cycleGAN所生成的圖片中，道路細緻度以及建築物還原度皆較MUNIT完整。
    2. 在虛擬圖(普通地圖)轉真實圖(衛星地圖)的結果中，cycleGAN可以將藍色部分轉換成湖，而MUNIT則否。
    3. 以真實地圖轉虛擬地圖來看，MUNIT產生的道路劃分出的街區較cycleGAN方正，但也較不貼近原本的虛擬圖；綠地的部分，cycleGAN生成的位置已經相當類似，MUNIT則是在原本沒有綠地的地方額外多生成了兩塊，因此綠地也是cycleGAN效果更好；湖的部分，cycleGAN雖然生成了一塊，但位置卻完全不正確

- 若以生成後的圖片色調來看，MUNIT在虛擬轉真實地圖的生成結果比較相似於真實的地圖模樣，而cycleGAN的虛擬轉真實成果則較原先的input有色彩上的落差，看起來有點接近於素描的筆觸。

- 另外，過程中我們也有拿小於70萬的model來嘗試生成，發現在50萬iterations前所訓練出的model產生的圖像中容易有模糊或波紋等不清楚的線條，而後期model的結果則與70萬的model類似，僅有細微之差。

- 以test image的截圖以及校園圖相比，道路筆直且建物的劃地很方正，所以結果十分完整，猜測MUNIT對於線條規則的圖片可能擁有比較高的辨識度；而校園的地形就複雜許多，難度也較市區或公路來的高，因此可能使Model容易產生混淆的狀況而導致效果不佳。
