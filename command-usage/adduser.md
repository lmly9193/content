# adduser

## 說明文件

```console
adduser [--home DIR] [--shell SHELL] [--no-create-home] [--uid ID] [--firstuid ID] [--lastuid ID] [--gecos GECOS] [--ingroup GROUP | --gid ID] [--disabled-password] [--disabled-login] [--add_extra_groups] [--encrypt-home] USER
   新增一般使用者

adduser --system [--home DIR] [--shell SHELL] [--no-create-home] [--uid ID] [--gecos GECOS] [--group | --ingroup GROUP | --gid ID] [--disabled-password] [--disabled-login] [--add_extra_groups] USER
   新增系統使用者

adduser --group [--gid ID] GROUP
addgroup [--gid ID] GROUP
   新增一個使用者群組

addgroup --system [--gid ID] GROUP
   新增一個系統群組

adduser USER GROUP
   將現有使用者加入現有的群組

general options:
  --quiet | -q      不要將處理資訊輸出到 stdout
  --force-badname   允許不符合 NAME_REGEX[_SYSTEM] 設定變數的使用者名稱
  --extrausers      使用額外的使用者作為資料庫
  --help | -h       使用說明訊息
  --version | -v    版本號碼和版權資訊
  --conf | -c FILE  使用 FILE 作為設定檔案
```

## 使用範例

### 新增使用者

```sh
sudo useradd username
```
