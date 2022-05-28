---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Yolo on Raspberry Pi 3"
subtitle: "在樹梅派上安裝及使用 yolo"
summary: ""
tags: ["raspberry-pi-3", "yolo"]
categories: []
date: 2018-03-28T00:57:00+08:00
lastmod: 2020-06-28T01:57:53+08:00
featured: false
draft: false

# toha params
menu:
  sidebar:
    name: Yolo on Raspberry Pi 3
    identifier: yolo-on-raspberry-pi-3
    weight: 500
---

### 簡介
Yolo 是一開放原始碼的影像辨識工具，可以進行圖片、影片、即時影像辨識。因為我的專題需要一個偵測人物走進一個空間的功能，因此認識了這套工具。但我對影像辨識實在不熟，因此接下來不會討論有關 yolo 的技術或是他的演算法等。有興趣的人可以自行去找它的論文慢慢看。

> Yolo 官方網站 : https://pjreddie.com/darknet/yolo/
> Yolo 目前最新版為第 3 版

這篇文章會教你如何在樹梅派 (使用 raspberry pi model 3b) 上安裝及使用 yolo，由於在樹梅派跑 v3 會有問題(下面會提到 )，所以這篇文章的示範會以 yolov2 為主。



### 安裝 Yolo
Yolo的安裝相當簡單，僅需將 repo clone 下來 make 即可

```
git clone https://github.com/pjreddie/darknet
cd darknet
make
```

### 下載 weights 檔
```
wget https://pjreddie.com/media/files/yolov2.weights
```

如果要下載不同的權重檔，請參考 `cfg/` 資料夾內的 .cfg 的檔名，將檔名複製下來，附檔名改成 .weights 即可用 wgets 下載

## 來試試 yolo 吧

```
./darknet detect cfg/yolov2.cfg yolov2.weights data/dog.jpg
```

以上指令會辨識一張範例圖片，並生成 `prediction.png` 檔，這個檔案是經過辨識後 yolo 將他辨識出來的物件加上外框而成 ( 實際效果可參考官網 )，如果執行指令都沒問題就表示安裝成功 !

在執行上述指令後，你可能會發現一個問題: 辨識速度也太慢了吧 ! 但因為樹梅派的運算能力有限，所以這個問題無解。不過 yolo 也有很多 model 可以選，我們可以用 *tiny yolo*，這個比較輕量的 model 來跑我們的辨識，但缺點就是辨識率沒有這麼高了。而且我看了一下 `cfg/` 資料夾，*tiny yolo* 似乎只有到 v2，第 3 版還沒出來的樣子。另外使用v3 會被 kill 掉 process，個人猜測是樹梅派的記憶體不足導致。

首先先下載 tiny yolo 的權重檔
```bash
wget https://pjreddie.com/media/files/yolov2-tiny.weights
```

現在再用 tiny-yolo 跑一次範例圖片測試效果
```bash
./darknet detect cfg/yolov2-tiny.cfg yolov2-tiny.weights data/dog.jpg
```

以下是我從網路抓隨便抓一個圖片測試的結果

原始圖片 ([圖片來源](https://www.theglobeandmail.com/news/toronto/worried-toronto-businesses-consider-protest-of-king-street-pilot-project/article37145174/) )
![source](https://s3-ap-northeast-1.amazonaws.com/jay-blog/yolo-on-raspi/source.JPG)

使用 yolov2 ( 辨識時間 158 秒 )
![yolov2](https://s3-ap-northeast-1.amazonaws.com/jay-blog/yolo-on-raspi/predictions-yolov2.jpg)

成功辨識出街上的人物！
