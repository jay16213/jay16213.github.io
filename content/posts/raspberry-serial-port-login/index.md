---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Raspberry Pi 透過 Serial Port 連線 - 踩雷筆記"
subtitle: ""
hero: featured.jpg
summary: ""
tags: ["raspberry-pi", "raspberry-pi-4", "linux"]
categories: []
date: 2021-02-10T00:14:53+08:00
lastmod: 2021-02-10T00:14:53+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Raspberry Pi 透過 Serial Port 連線 - 踩雷筆記
    identifier: raspberry-pi-serial-port
    weight: 500
---

最近因為想要從 serial port 登入樹梅派而入手了一個 USB 轉 TTL 傳輸線，沒想到設定過程踩了不少雷，故寫了這篇作為提醒用。

我是從電子材料行買的，型號為[CONCBLUSBTTL](https://www.ltc.com.tw/Product/Detail?ICODE=CONCBLUSBTTL)，使用 PL2303TA 晶片

{{< img src="pl2303ta.jpg" align="center" title="pl2303ta">}}
{{< vs 3 >}}

理論上 win10 可以自動抓可以用的驅動，不需要特別下載。但如果有問題的話還是可以從晶片製造商的[驅動程式下載頁面](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=225&pcid=41)下載最新的驅動。解壓縮後除了驅動程式外還有一個小程式可以用來檢查晶片型號。

這裡遇到我採的第一個雷 (自己雷)：在我一開始無法順利設定時，經爬文得知，由於舊款晶片 (PL2303HX 系列) 有偽造情形，故 win10 自動抓的驅動並不支援該款晶片的產品，需要下載舊版驅動才行，我還以為自己真幸運在 2021 還買得到舊款晶片，大費周章跑去前人的 blog 載舊版驅動，結果還是不行；又因為在舊版驅動下跑上面原廠提供的晶片型號檢查程式會將我的傳輸線誤判為 PL2303XA/PL2303HXA

{{< img src="pl2303hx.jpg" align="center" title="pl2303hx">}}
{{< vs 3 >}}

害我誤以為自己真的買到 PL2303HX 晶片，一直往錯誤的方向 google。後來完全解決不了，改用新版驅動再測最後一次，想說不行就要放棄這條線了，才發現原來都是驅動搞的鬼。

這裡提醒一下接線的注意事項，傳輸線和樹梅派的 TX/RX 要反著接，也就是 線的 TX 要接到 Pi 的 RX，線的 RX 要接到 Pi 的 TX。GND 當然也要記得接。我這條線還有一個 5V 線，接上去後可以為 Pi 供電並開機，不過看其他人文章表示不一定要接這條線，而且我怕 GPIO 的供電不穩 (我用的是 pi 4, pi 4 在供電不足的情況下 wifi 會無法連線，就算能順利連上品質也不好，所以供電一定要足) 所以就沒接了。

解決完驅動問題接下來就剩連線設定了，這裡是第二個要注意的地方，所有參數都要設定正確不然就會連不上！像是 speed，網路上的教學大都多是教人設定為 115200，但我的這條線，在裝置管理員的預設值為 9600，所以連線時要設 9600；還有 flow control 不論是 putty 或是 MobaXterm 預設都會是 Xon/Xoff，同樣與這條線的預設值不符，要記得關掉。

下圖為不更改 CONCBLUSBTTL 這條線的預設值的情況下，我的 MobaXterm 的連線設定

{{< img src="config.jpg" align="center" title="config">}}
{{< vs 3 >}}

關於 CONCBLUSBTTL 的連線預設值可以去 裝置管理員 -> 連接埠設定找到

{{< img src="default.jpg" align="center" title="default-value">}}
{{< vs 3 >}}

所有設定都完成後就可以透過 serial port 連上 Pi 了！

{{< img src="login.jpg" align="center" title="success login via serial">}}
{{< vs 3 >}}


P.S. 建議連線設定完後要重插傳輸線並將 Pi 重開機比較不會遇到奇怪的問題，如果還是沒顯示 login 提示的話，可以試試看按 enter (我猜可能是要按 enter 才會觸發 pi 或是 傳輸線傳輸資料)
