# usermod

## 說明文件

```console
Usage: usermod [options] LOGIN

Options:
  -b, --badnames                允許使用不良名稱
  -c, --comment COMMENT         GECOS 欄位的新值
  -d, --home HOME_DIR           用戶帳戶的新家目錄
  -e, --expiredate EXPIRE_DATE  將帳戶到期日期設置為 EXPIRE_DATE
  -f, --inactive INACTIVE       在帳戶過期後將密碼設置為 INACTIVE
  -g, --gid GROUP               將 GROUP 強制用作新的主要組
  -G, --groups GROUPS           新的輔助 GROUP 清單
  -a, --append                  在不刪除用戶從其他組中刪除的情況下，將用戶附加到 -G 選項提到的補充 GROUP 中
  -h, --help                    顯示此幫助消息並退出
  -l, --login NEW_LOGIN         登錄名稱的新值
  -L, --lock                    鎖定用戶帳戶
  -m, --move-home               將家目錄的內容移動到新位置（僅使用 -d）
  -o, --non-unique              允許使用重複（非唯一）UID
  -p, --password PASSWORD       使用加密密碼設置新密碼
  -R, --root CHROOT_DIR         chroot 到的目錄
  -P, --prefix PREFIX_DIR       位於 /etc/* 檔案所在的前綴目錄
  -s, --shell SHELL             用戶帳戶的新登錄 Shell
  -u, --uid UID                 用戶帳戶的新 UID
  -U, --unlock                  解鎖用戶帳戶
  -v, --add-subuids FIRST-LAST  添加下屬 UID 範圍
  -V, --del-subuids FIRST-LAST  刪除下屬 UID 範圍
  -w, --add-subgids FIRST-LAST  添加下屬 GID 範圍
  -W, --del-subgids FIRST-LAST  刪除下屬 GID 範圍
  -Z, --selinux-user SEUSER     用戶帳戶的新 SELinux 用戶映射
```

## 使用範例

### 將當前使用者加入 Docker 群組

```sh
sudo usermod -aG docker $USER
```
