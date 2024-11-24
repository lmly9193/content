# 在 Linux 上安裝 Java 開發環境

## 確認目前的Java版本

如果你使用的是 OpenJDK 那返回的結果應該像是這樣

```shell
$ java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-b09)
OpenJDK 64-Bit Server VM (build 25.242-b09, mixed mode)
```

如果你使用的是 Oracle JDK 那返回的結果應該像是這樣

```shell
$ java -version
java version "1.8.0_241"
Java(TM) SE Runtime Environment (build 1.8.0_241-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
```

---

## 安裝 OpenJDK

### 設定環境變數

在安裝完JDK之後，你也需要新增環境變數`JAVA_HOME`到你的個人設定之中：
shell、bash: `export JAVA_HOME=path_to_java_home`
csh (C shell): `setenv JAVA_HOME=path_to_java_home`

### RHEL-based 系統

```shell
sudo update yum
sudo yum install java-1.8.0-openjdk         # OpenJDK 8 JRE
sudo yum install java-1.8.0-openjdk-devel   # OpenJDK 8 JDK
```

### Debian、Ubuntu 系統

```shell
sudo apt-get update
sudo apt-get install openjdk-8-jre  # OpenJDK 8 JRE
sudo apt-get install openjdk-8-jdk  # OpenJDK 8 JDK
```

### alternatives

If the correct version of the JDK is not being used, use the alternatives command to switch it:

```shell
$ sudo alternatives --config java
There are 5 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
   1           java-1.7.0-openjdk.x86_64 (/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.161-2.6.12.0.el7_4.x86_64/jre/bin/java)
   2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.151-5.b12.el7_4.x86_64/jre/bin/java)
   3           /usr/lib/jvm/jre-1.6.0-openjdk.x86_64/bin/java
*+ 4           /usr/java/jre-9.0.4/bin/java
   5           /usr/java/jdk-9.0.4/bin/java




Enter to keep the current selection[+], or type selection number:
```

Alternatives 是通過 Symbolic links 來管理預設命令，可用於選擇預設的Java。你只需簡單的輸入選擇號碼即可調整預設情況下應使用的Java版本。

---

## 安裝 Oracle JDK

### 下載資源

* [Java SE Downloads](https://www.oracle.com/java/technologies/javase-downloads.html)
* [Java Cryptography Extension (JCE)](https://www.oracle.com/java/technologies/javase-jce-all-downloads.html)


### RHEL-based 系統

這裡有幾個安裝方法，由不同作者提供。

#### 1. datastax [原文出處](https://docs.datastax.com/en/jdk-install/doc/jdk-install/installOracleJdkRHEL.html)

##### 1.1 安裝

```shell
sudo update yum
sudo rpm -ivh jdk-8uversion-linux-x64.rpm
```

RPM 會將 Oracle JDK 安裝到目錄 `/usr/java/`

##### 1.2 使用 Alternatives 套件功能或手動設定環境變數

```shell
sudo alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_version/bin/java 200000
```

使用 alternatives 指令來創建 Symbolic links 指向 Oracle JDK ，如此你的系統將會使用 Oracle JDK 取代預設的 OpenJDK。

```bash
export PATH="$PATH:/usr/java/latest/bin"
set JAVA_HOME=/usr/java/latest
```

#### 2. DigitalOcean [原文出處](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-centos-and-fedora#prerequisites)

##### 2.1 安裝

```shell
sudo yum localinstall jdk-8uversion-linux-x64.rpm
```

Oracle JDK 應該會被安裝在`/usr/bin/java`目錄並連結到`/usr/java/jdk1.8.0_161/jre/bin/java`目錄。

##### 2.2 Alternatives

```shell
sudo alternatives --config java
```

##### 2.3 環境變數

許多Java應用開發者會使用`JAVA_HOME`或`JRE_HOME`的環境變數來確認Java命令的位置。

舉例來說，如果你將Java安裝在`/usr/java/jdk1.8.0_161/jre/bin`目錄，表示你可能設置了`JAVA_HOME`的環境變數在Bash Shell或是其他腳本中，像是這樣...

```bash
export JAVA_HOME=/usr/java/jdk1.8.0_161/jre
```

如果你希望預設情況下為系統上的每位使用者設定`JAVA_HOME`的環境變數，請將上面的腳本命令加到`/etc/environment`檔案之中，或是運行如下的腳本命令一樣，是一個比較簡單修改這個檔案的辦法。

```shell
sudo sh -c "echo export JAVA_HOME=/usr/java/jdk1.8.0_161/jre >> /etc/environment"
```

#### 3. 據自己以前安裝的經驗

```shell
sudo yum install jdk-8uversion-linux-x64.rpm
sudo yum install jre-8uversion-linux-x64.rpm
```

```bash
export PATH=/usr/lib/jvm/jdk1.8.0_version/bin:$PATH
export JAVA_HOME=/usr/java/jdk1.8.0_version
```

### Debian、Ubuntu 系統

#### 設定環境變數

在安裝完JDK之後，你也需要新增環境變數`JAVA_HOME`到你的個人設定之中：
shell、bash: `export JAVA_HOME=path_to_java_home`
csh (C shell): `setenv JAVA_HOME=path_to_java_home`

#### 建立目錄&解壓縮並安裝Oracle JDK

```shell
sudo mkdir -p /usr/lib/jvm
sudo tar zxvf jdk-version-linux-x64.tar.gz -C /usr/lib/jvm
```

Oracle JDK 將被安裝在名為<code>jdk-8u_**version**</code>的目錄中

#### 告知alternatives有可用的新Java版本

```shell
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_version/bin/java" 1
```

**Note.** 如果你是刪除的先前版本手動進行更新，請執行以上命令兩次，因為你將會在第一次執行該命令時收到錯誤消息。

#### 將新版本作為預設

```shell
sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_version/bin/java
```
