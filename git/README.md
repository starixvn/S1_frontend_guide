# Git 手冊 

## 目錄 
 - [Git Flow](#Git Flow)
 - [程式環境](#程式環境)
 - [分支](#分支)
 - [完整開發流程](#完整開發流程)
 - [分支命名規則](#分支命名規則)
 - [疑難雜症](#疑難雜症)
   - [MR有衝突](#mr衝突)
   - [把多個 Commit 合併成一個 Commit](#把多個-commit-合併成一個-commit)

## Git Flow 

基本專案工作流程遵循 Git Flow 並根據團隊衍伸的變形，可先閱讀 [此文](https://gitbook.tw/chapters/gitflow/why-need-git-flow)，了解基礎結構。

## 程式環境

前端專案主要會發布至三個程式環境，分別為：

1. DEV 環境 : 公司內部 QA 測試用 
2. Release ( RLS ) : 市場測試用 
3. Production ( PRD ) : 正式環境

## 分支
### `develop` 分支
在不同專案可能叫 `DEV` 等名字，專案的主要基礎分支，開發新功能時都是從此分支開 branch 出去製作，功能完成並測試通過後會合併回此分支。

### `master` 分支
在不同專案可能叫 `PRD` 等名字，穩定線上版的分支，不可直接 commit 異動，原則上除了 hotFix 外僅能透過 merge develop 分支合併要上線的 code。

### `feature` 分支 
開發新功能時，直接從 `develop` 開新分支做開發，經過 code review 與測試，確定上線前再 merge 回 `develop` 分支。 

### `stage` 分支
內部測試環境所使用的分支，feature 在 review 完成後，經過主管允許可直接 merge 進 stage 並發布至 DEV 測試環境。 

### `release` 分支
在不同專案可能叫 RLS ，經過內部測試完成之後，經主管允許或 PM 通知後，merge feature 至 release branch 並發布至 RLS 環境。

### `hotFix` 分支 
處理上線產品緊急問題時使用，直接從 `master` 開 hotFix 分支修復，修復完成後再 merge 回 `master`，同時也需要 `cherry-pick` 至各環境分支。

## 完整開發流程

1. 接到需求後從 `develop` 開新 `feature` 做開發 
2. 開發完成後發布 MR，選擇 merge 回 `develop`
3. Code review 完成後，merge 該 branch 至 `stage` 並發布至 DEV 測試環境 
4. DEV 測試完成後 merge 該 branch 至 `release` 並發布至 RLS 測試環境 
5. RLS 測試完成，由有 merge `develop` 權限之同仁將 MR 單 merge 回 `develop`
6. merge 最新版本的 `develop` 至 `master` 並發布至 PRD 完成開發。

## 分支命名規則 
原則上所有新功能開發都會放在 `feat` 資料夾底下，如果有 Jira 單則需加上單號，方便辨識

```bash
# on develop branch 
git checkout -b "feat/ms-3433-your-feat-name"
```

以此類推，hotFix 類型則需放於 `hotFix` 資料夾底下。

## 疑難雜症 

## MR衝突 
MR 在發布後可能經過一陣子迭代，中間新增了其他同事的程式碼，導致最後要 merge 時有衝突無法 merge : 

若你的 branch 本身存在多個 commit 時，請先合併成單一 commit 在 rebase 解衝突 ( 合併成單一 Commit 請看下一章 )

請使用你的 branch rebase 最新的 `develop`，解完衝突 rebase 完成後再 `push -f`

```bash
# on your branch 
git pull develop # 拉最新的 develop
git rebase develop 

# 解完衝突後 
git push -f 
```

rebase 後的 branch 會和 origin 上的 branch commit 重複，故使用 `-f` 直接覆蓋掉舊的 commit

> 非操作自己的 feature branch 時嚴禁使用 push -f

## 把多個 Commit 合併成一個 Commit

[教學文章](https://gitbook.tw/chapters/rewrite-history/merge-multiple-commits-to-one-commit)