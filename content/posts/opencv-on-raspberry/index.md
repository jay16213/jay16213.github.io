---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "OpenCV on Raspberry Pi 3"
subtitle: "opencv 安裝教學"
summary: ""
authors: []
tags: ["linux", "opencv", "raspberry-pi-3"]
categories: []
date: 2018-04-02T23:44:05+08:00
lastmod: 2020-06-28T02:05:32+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### 簡介
[OpenCV](https://opencv.org/) 是由 intel 主導的一個電腦視覺的跨平台 library，可以用在許多應用上。yolo real-time detection ( 關於 yolo 的安裝與使用可以看[我之前寫的文章: Yolo on Raspberry Pi 3](/posts/yolo-on-raspberry/) ) 就有用到 OpenCV。這篇文章就是要教如何在樹梅派上自行編譯、安裝 OpenCV 函式庫。雖然是強調在樹梅派上安裝 OpenCV，不過都是流程都是按照[官方文件的 Linux 平台安裝教學](https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html)走，所以只要是 Linux 平台都可以理論上都可以照著本篇文章來安裝 OpenCV！（當然裡面有些指令可能會不太一樣，例如 `apt-get` 是 debian 流派，如果是 redhat 就要換成 `yum`）


要提醒的是 OpenCV 還有很多安裝的參數可以調整（例如對更多 Python / Java 的支援等）但是這篇文章目的是基本的 OpenCV 安裝，所以會略過那些非必要的步驟，需要更多自訂設定的人可以點上面的官方教學自行研究。

### 安裝教學

1. 首先安裝套件
```bash
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev # 這行的套件非必要，可裝可不裝
```

2. 從 Github 上把原始碼 clone 下來，並進入 opencv 資料夾
```bash
cd ~/ # 可自行選擇 clone 的位置
git clone https://github.com/opencv/opencv.git
cd ~/opencv
```
  整個 repo 有快 500 MB，clone 需要花一點時間

3. 切換 OpenCV 版本

  非常重要！如果你有要用在 yolo 上面，或是你有其他用 C 編譯的套件是需要 OpenCV 的，請記得 checkout 到 3.4.0 版或以下，因為 3.4.1 版的 opencv 有 bug（不過根據開發者的說法是 OpenCV 3.X 不再支援 C compilation mode，所以嚴格來講不算是 bug，相關討論可以看 [opencv-issue#10963](https://github.com/opencv/opencv/issues/10963)，如果你堅持要用 3.4.1，裡面也有教你如何修正）如果沒有要用到 C compiler 的可以略過這步。

  先用 `git tag -l` 確認有哪些版本可供使用，接著就可以 checkout 過去了，例如我現在要示範的 3.4.0
```bash
git checkout 3.4.0
```

1. 建立一個 build 資料夾並進入
```bash
mkdir build
cd build/
```

5. 執行 cmake 產生 makefile
```bash
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

6. 編譯函式庫
    由於樹梅派只有 4 核，所以我們開 4 個核心編譯即可
```bash
make -j4
```
這裡是最困難的一步！因為我們都知道樹梅派的處理器效能沒有一般電腦那麼好，所以這裡會花很多時間，我自己的經驗是 `-j4` 開下去最快也要 90 ～ 120 分鐘（時間也會受到 SD 卡的讀寫速度影響，所以實際時間因人而異），而且樹梅派有可能編到一半當掉，所以一定要有耐心！如果一直當掉的話可以考慮降低核心數，但是相對的就會花更多時間。

7. 安裝函式庫
    到了這步基本上就算成功了！接下來只須執行安裝指令：
```bash
sudo make install
```


#### 確認 OpenCV 有無安裝成功

重開樹梅派後，執行 `python` 進到 python 的 interpreter，測試看看是否可以正確的 import OpenCV 的函式庫及印出版本資訊，有印出版本就代表安裝成功。

```bash
>>> import cv2
>>> cv2.__version__
3.4.0
```
