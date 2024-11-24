# 在 Linux 建立虛擬記憶體 (Swap)


系統當主記憶體(RAM)不敷使用的時候，會拿出部分硬碟空間將記憶體內比較久沒用到的資料以檔案的形式儲存，待需要使用的時候再讀取。由於是將資料以檔案的形式儲存，硬體的讀寫速度將會是虛擬記憶體的效能瓶頸。

而在 Linux 系統中「Swap」指的是交換空間，它與 Windows 系統中的「虛擬記憶體」提供相似的功能。

## 建立及掛載虛擬記憶檔案

在這個範例中，我們將使用 `fallocate` 指令創建一個 1GB 大小的交換檔案。接著，我們設定該檔案的權限為 600，確保只有 root 用戶可以讀寫該檔案。然後，我們使用 `mkswap` 指令將該檔案轉換為 Swap 空間。最後，我們啟動這個 Swap 空間，使其立即生效。這樣，我們就成功地創建並啟動了一個 1GB 大小的 Swap 空間。

```sh
# 建立一塊保留空間
sudo fallocate -l 1G /swapfile

# 設置讀寫權限
sudo chmod 600 /swapfile

# 建立Swap空間
sudo mkswap /swapfile

# 啟動Swap空間
sudo swapon /swapfile
```

## 檢查 Swap 狀態

```sh
sudo swapon --show
```

```console
NAME      TYPE  SIZE  USED PRIO
/swapfile file 1024M 20.3M   -2
```


## 隨系統啟動

要在開機後自動使用 `/swapfile` 作為 Swap 空間, 需要修改 `/etc/fstab` 檔案

```sh
# sudo cp /etc/fstab /etc/fstab.backup
echo "/swapfile swap swap sw 0 0" >> /etc/fstab
```

## 參考資料
- [Swap交換空間是什麼？如何在Linux建立Swap？](https://klab.tw/2022/06/what-is-linux-swap-and-how-to-add-it/) - KYLE'S BLOG (KODELAB)
- [建立及掛載虛擬記憶檔案](https://dywang.csie.cyut.edu.tw/dywang/linuxSystem/node47.html)
