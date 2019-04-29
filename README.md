# Homework4
## Sequence of moving-forward images in NTHU campus
下圖為昆明湖旁邊的小徑
![](https://i.imgur.com/Sx2EG95.jpg)

## Feature extraction using ORB
![](https://i.imgur.com/hY38e3J.jpg)


## Image alignment 
我們使用ORB feature來計算descriptors，再針對matches的好壞進行sorting，並保留了10%的good matches來進行alignment。

執行的過程中，每align一次其結果會產生或多或少的黑邊，導致圖像整體看起來稍加扭曲或扁平。我們試過將align後的結果圖切除黑邊再與其他張input進行align，執行到最後所產生的圖像視角會更加歪斜，且黑邊範圍會更大使得圖像更加不完整，因此我們是將一連串連續的圖片，兩兩原圖配對來執行alignment。

結果如下圖所示：

![](https://i.imgur.com/TQ1yrfI.jpg)

## Different feature extrators
我們使用的其他特徵檢測為SIFT及SURF，兩者皆為尺度不變的特徵檢測法，其共同的步驟主要為先建立尺度空間，並提取圖像特徵點(keypoints)，接著利用特徵點的周圍領域來生成相應的描述子(descriptors)，最後再進行所有特徵點的匹配。

以下為各方法的介紹與執行結果：
### 1. SIFT(Scale-Invariant Feature Transform)
- SIFT此演算法用來偵測與描述影像中的局部性特徵，它在空間尺度中尋找極值點，並提取出其位置、尺度、旋轉不變數。算法為在不同的尺度下用Gaussian filters進行convolved，以連續的高斯模糊化影像差異找出關鍵點。再根據關鍵點附近像素的資訊、關鍵點的尺寸和主曲率來消除部分關鍵點。關鍵點以相鄰像素的梯度方向分布作為指定方向參數，使關鍵點描述子能以此方向來表示並具備旋轉不變性。再建立描述子向量，使描述子在不同光線與視角下皆能保持其不變性。

- 配對結果如下圖
![](https://i.imgur.com/rKd2pxP.jpg)



### 2. SURF(Speeded Up Robust Features)
- SURF同時具有尺度與旋轉的不變性，其算法主要利用了Hessian Matrix來提取圖中的特徵點，而在生成尺度空間中，SURF能讓圖像維持原本大小而只改變filter的尺度大小，用以提升運算速度與精確度。接著利用Non-maximum suppression來檢測出初步特徵點與特徵最強點，最後使用Haar小波特徵來得到特徵點的主方向。與SIFT相比，SURF能夠加快整體程式運行的時間。

- 配對結果如下圖
![](https://i.imgur.com/fyZpseW.jpg)

### Comparison
![](https://i.imgur.com/DXCywA0.jpg)

以上的結果均為採用不同視角的照片進行配對。
- 以計算速度來說，ORB最快，SURF次之，SIFT則是最慢。
- 以我們實際match的成果來看，三種方法皆只列出40個match的話，ORB的match的線較為集中在某一區，而SIFT跟SURF則是比較分散。
- SIFT與SURF的配對結果看起來較為相似，相較之下SURF的準確度稍高，線也較為整齊；而SIFT也有一定的準確度，但偶爾會有較大的歪斜配對線(如第一列與最後一列)。

## Infinite zooming effect
Infinite zooming的部分，由於alignment的結果到後面畫面會越來越扭曲，且在一次次的裁剪過後，照片畫質也會變得越來越差，因此我們並沒有使用alignment的結果來做zooming，而是用手動的方式去將照片接起來，再去做zooming。

- Infinite zooming成果如下：

![](https://github.com/vivian0310/cvfx_hw4/blob/master/before.gif)

## Image processing
我們使用PhotoImpact修圖，主要利用「仿製-畫筆」的功能來去除照片邊框的部分，使邊界連接處看起來較為平順。

- 修圖後的成果如下：

![](https://github.com/vivian0310/cvfx_hw4/blob/master/after.gif)

#### Youtube URL
https://youtu.be/tMiLPmWC4mc
