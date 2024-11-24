# Git 設定 AutoCRLF 與 SafeCRLF 的差異

一言以蔽之: 告訴git哪些是目錄, 子目錄或文件類型的集合是屬於甚麼屬性

使用情境(摘自於官網):

* 情境一 : **識別二進位檔案**

* 情境二 : **Diffing Binary Files**

## **識別二進位檔案**

在Mac中的Xcode專案都會有一個 `.pbxproj` 結尾的檔案[註1], 簡單來說這個檔案像是一個文字檔類型的小型資料庫, 當資料被兩個人修改時, 是無法進行合併, 也無法使用diff, 只有機器才能進行識別和操作, 這樣的操作不像是一個文字檔, 反而像是二進位檔案, 於是可以讓git將這類型檔案視為二進位檔案

解決的做法可以將以下指令加入到 `.gitattributes` 文件:

```text
*.pbxproj -crlf -diff
```

git 1.6及之後的版本中可以用一個巨集代替 `-crlf -diff` :

```text
*.pbxproj binary
```

> 註1: 它是由記錄設定項的 IDE 寫到磁碟的 JSON 資料集, 是一種純文字 javascript 資料類型

## **Diffing Binary Files**

Git 對於 MS的word檔( *.doc) 當作是binary的檔案, 於是如果想對這類型的檔案做diff時, 只會出現下方結果:

```shell
$ git diff
diff --git a/chapter1.doc b/chapter1.doc
index 88839c4..4afcb7c 100644
Binary files a/chapter1.doc and b/chapter1.doc differ
```

這樣的結果完全無法得知doc檔案類型修改的部分，除非人工細看, 所以gitattribute解決這樣的問題, 將以下指令加入到 `.gitattributes` 文件

```text
*.doc diff=word
```

這樣git 就能知道, 當要對word檔(*.doc)做diff時, git 會使用”word”篩檢程式(filter)[註2], 把 word 文檔轉換成可讀的文字檔, 在進行diff, 如以下結果

```shell
$ git diff
diff --git a/chapter1.doc b/chapter1.doc
index c1c8a0a..b93c9e4 100644
--- a/chapter1.doc
+++ b/chapter1.doc
@@ -128,7 +128,7 @@ and data size)
Since its birth in 2005, Git has evolved and matured to be easy to   use and yet retain these initial qualities. It’s incredibly fast, it’s very efficient with large projects, and it has an incredible branching
-system for non-linear development.
+system for non-linear development (See Chapter 3).
```

> [註2] ”word” 篩檢程式: 必須事先設定

```shell
git config diff.word.textconv catdoc

# 再到 .git/config 設定
# [diff "word"] textconv = catdoc
```

> 資料來源
> https://medium.com/@ceall8650/gitattributes-%E7%9A%84%E7%94%A8%E8%99%95-bb165826df33


## AutoCRLF / SafeCRLF

這兩個設定都是針對換行符的設定，主要是 Git 早期是為了維護 Linux kernel 設計而成的，所以絕大部分使用環境都是 Linux 平台，而在 Linux 平台下，文字檔案的斷行符號預設為 `LF` 字元 (`\n `) ( `0x0A` )。不過，在 Windows 環境下，文字檔案的預設斷行符號卻是 `CRLF` ( `\r\n` ) ( `0x0D` `0x0A` )。這種轉換功能，加上 Git 的檔案比對，此種設定會造成一些可能影響與風險

* 若是儲存庫中同時包含 `LF` 與 `CRLF` ，經過自動轉換之後，就會造成原始檔案內容不同，等於竄改或破壞原始檔案，增添辨識問題。

### AutoCRLF

此功能就上述提及，會自動轉換換行字元，主要的設定影響如下：

* 設定成 `true` 的作用是 `commit` 時會自動將 `CRLF` 轉成 `LF` ； checkout 時會自動將 `LF` 轉成 `CRLF`

    ```shell
    git config --global core.autocrlf true
    ```

* 設定成 `input` 的作用是 commit 時會自動將 `CRLF` 轉成 `LF` ； checkout 時不轉換

    ```shell
    git config --global core.autocrlf input
    ```

* 設定成 `false` 的則是停止自動轉換，不管 `commit` 或是 checkout 都不會進行轉換

    ```shell
    git config --global core.autocrlf false
    ```

### SafeCRLF

這設定是更加嚴格的過濾換行符，只要 `git add` 或是 `commit` 或是 `push` 都會過濾

* `不允許` 有 `LF` 與 `CRLF` 混合的檔案

    ```shell
    git config --global core.safecrlf true
    ```

* `允許` 有 `LF` 與 `CRLF` 混合的檔案

    ```shell
    git config --global core.safecrlf false
    ```

* `允許` 有 `LF` 與 `CRLF` 混合的檔案，但是會出現 warning 警告訊息

    ```shell
    git config --global core.safecrlf warn
    ```

> 資料來源：[Git Autocrlf 與 Safecrlf - ShunNien's Blog](https://shunnien.github.io/2018/06/03/git-autocrlf-and-safecrlf/)
