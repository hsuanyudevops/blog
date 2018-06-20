---
title: Linux 新增、掛載硬碟
date: 2018-06-19 14:47:50
tags:
thumbnailImage: 55.jpg
thumbnailImagePosition: left
coverImage: city-750.jpg
coverCaption: "A beautiful sunrise"
coverMeta: out
coverSize: full
---

## 一、新增磁碟
<!-- more -->
### 1. 顯示目前主機上的硬碟

```
ls /dev/[sh]d*
```
{% asset_img 1.JPG step1 %}
### 2. 確認所新增的硬碟資訊<br>
```
fdisk -l /dev/sdb
```
{% asset_img 2.JPG step2 %}
### 3. 對sdb磁碟進行分割，再輸入"m"獲得說明<br>
```
fdisk /dev/sdb
```
{% asset_img 3.JPG step3 %}
### 4. 輸入"n"新增分割區，再輸入"p" (硬碟全部只要一個分割區) <br>
{% asset_img 4.JPG step4 %}
### 5. 輸入"1"(磁碟代號)，按 Enter (起始磁區使用預設值)，在按 Enter (最後磁區使用預設值)<br>
{% asset_img 5.JPG step5 %}
### 6. 輸入"w" (將分割表寫入磁碟後離開)<br>
{% asset_img 6.JPG step6 %}
### 7. 對sdb1磁碟做ext4格式化
```
mkfs -t ext4 /dev/sdb1
```
{% asset_img 7.JPG step7 %}
## 二、掛載磁碟(使用UUID掛載)
### 1. 列出所有磁碟的UUID<br>
```
sudo blkid
```
{% asset_img 9.JPG step9 %}
### 2. 編輯fstab檔案，將要掛載硬碟的UDID填入，編輯後存檔重啟系統後就會自動掛載。
```
vim /etc/fstab
```
{% asset_img 10.JPG step10 %}


| fstab內容 | 說明 |
| ------| ------ | 
| UUID | 填入需掛載磁碟的UUID | 
| /data | 將此磁碟掛載到/data的路徑 | 
| ext4 | 檔案系統類型 | 
| defaults | 掛載時要使用的掛載參數 | 
| 第一個0 | dump 會根據這個設定決定是否需要備份，一般設定為 0, 即不備份；1 為每日備份；2 為隔日備份。 | 
| 第二個0 | fsck 會根據這個設定，決定在不正常關機後，檢查檔案系統的順序。根目錄要設定成 1, 其他分割區設定成 2, 如果設定成 0, 則不會作 fsck 檢查。 | 