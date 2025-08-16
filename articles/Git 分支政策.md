# Git 分支策略

這個流程的核心是：永遠保持 `master` 分支的穩定性與可部署性，並使用 `develop` 分支作為上線前的最後測試關卡。

## 核心分支

1. `master`

    - 用途： 儲存正式上線的產品程式碼。此分支的內容應永遠與正式環境一致。
    - 規則： 只能從 develop 分支合併。絕不直接在此分支上進行任何開發。

2. `develop`

    - 用途： 整合所有已完成的開發，並部署到測試環境供客戶測試。
    - 規則： 是所有新功能開發的基礎分支。

## 功能分支

1. `feature/*` (或任何臨時名稱的開發分支)

    - 用途： 進行所有實際的程式設計工作，包括新功能和錯誤修復。
    - 規則： 從 develop 建立，完成後合併回 develop。這類分支是短期的，用完即刪。

---

# 工作流程

這是一個非常線性的流程，易於遵循。

## 步驟 1：開始新工作 (開發新功能或修復錯誤)

從 `develop` 分支建立一個新的臨時分支。

```shell
# 1. 切換到 develop 分支並確保是最新版本
git checkout develop
git pull origin develop

# 2. 根據任務建立一個新分支 (例如：開發登入功能)
git checkout -b feature/login-page
```

## 步驟 2：進行開發、除錯、緊急修復問題

在新建立的 `feature/login-page` 分支上進行所有程式碼的修改與提交。

```shell
# (進行程式設計...)
git add .
git commit -m "完成使用者登入頁面設計"

# (繼續開發...)
git add .
git commit -m "新增密碼驗證邏輯"

# 將分支推送到遠端
git push -u origin feature/login-page
```

## 步驟 3：合併至 develop 並部署到測試機

當功能開發完成，準備給客戶測試時，就發 PR 並將功能分支合併回 `develop` 分支。

設定 `Auto-complete` 條件時，請記得刪除來源分支。

## 步驟 4：合併至 master 並部署到正式機

客戶在測試環境確認無誤後，就可以更新正式環境，然後將 `develop` 分支的穩定版本發 PR 到 `master` 分支。

## 步驟 5：刪除本地分支

```shell
# 自動清理不在遠端的追蹤分支
git fetch --prune

# 刪除本地的 feature 分支
git branch -d feature/login-page
```

---

# 總結

這個模型非常適合單人或小型團隊：

- 流程簡單： 只有 `master`、`develop` 和 `feature/*` 功能開發分支三種類型。
- 職責清晰： `master` 對應正式機，`develop` 對應測試機。
- 工作隔離： 所有實際開發都在臨時分支完成，確保 `develop` 和 `master` 的相對穩定。
- 無縫處理緊急修復： 即使是緊急修復，也可以遵循同樣的流程 (建立修復分支 -> 合併到 `develop` 測試 -> 合併到 `master` 上線)，無需引入複雜的 hotfix 流程。