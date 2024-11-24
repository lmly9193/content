# passwd

## 說明文件

```console
Usage: passwd [options] [LOGIN]
Options:
  -a, --all                     報告所有帳戶的密碼狀態
  -d, --delete                  刪除指定帳戶的密碼
  -e, --expire                  強制使指定帳戶的密碼過期
  -h, --help                    顯示此幫助訊息並退出
  -k, --keep-tokens             只有在密碼過期時才更改密碼
  -i, --inactive INACTIVE       在密碼過期後將密碼設置為 INACTIVE
  -l, --lock                    鎖定指定帳戶的密碼
  -n, --mindays MIN_DAYS        將密碼更改前的最小天數設置為 MIN_DAYS
  -q, --quiet                   靜默模式
  -r, --repository REPOSITORY   在 REPOSITORY 存儲庫中更改密碼
  -R, --root CHROOT_DIR         切換到的目錄
  -S, --status                  報告指定帳戶的密碼狀態
  -u, --unlock                  解鎖指定帳戶的密碼
  -w, --warndays WARN_DAYS      將密碼過期警告天數設置為 WARN_DAYS
  -x, --maxdays MAX_DAYS        將密碼更改前的最大天數設置為 MAX_DAYS
```

## 使用範例

### 修改當前使用者密碼

```sh
passwd
```

### 修改其他使用者密碼

```sh
passwd username
```

### 查看用戶狀態

```sh
passwd -S username
```

## 其他應用

### 列出所有帳號

```sh
cat /etc/passwd
```
