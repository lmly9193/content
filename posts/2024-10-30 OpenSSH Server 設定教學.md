# OpenSSH Server 設定教學

## 使用 root 登入

```sh
sudo -i

# 自行設置一個密碼
passwd

# change "PermitRootLogin no"->"PermitRootLogin yes"
vi /etc/ssh/sshd_config
```

## 禁止 root 登入

```sh
# change "PermitRootLogin yes"->"PermitRootLogin no"
sudo vi /etc/ssh/sshd_config

# 重新啟動ssh
sudo service ssh restart
```
