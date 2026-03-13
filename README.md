# git-setup

> 本專案 Fork 自 [doggy8088/git-setup](https://github.com/doggy8088/git-setup)，感謝保哥的貢獻。

本工具會全自動設定 Git 版控環境，並且跨平台支援 Windows, Linux, macOS 等作業系統的命令列環境，尤其針對中文環境經常會出現亂碼的問題都會完美的解決。

## 先決條件

- [Node.js](https://nodejs.org/en/) 10.13.0 以上版本
- [Git](https://git-scm.com/) 任意版本 (建議升級到最新版)

## 使用方式

```sh
npx @joker.kang/git-setup
```

或者直接指定名稱與 Email:

```sh
npx @joker.kang/git-setup --name "Your Name" --email your.email@example.com
```

或者使用互動模式，逐項確認每個設定：

```sh
npx @joker.kang/git-setup -i
```

- 設定過程會詢問你的 `user.name` 與 `user.email` 資訊
  - 如果在命令列中使用 `--name` 與 `--email` 參數，則不會詢問
  - Email 會進行格式驗證，格式錯誤會拒絕設定下去
- 所有 Git 設定都會以 `--global` 為主 (`~/.gitconfig`)
- Windows 平台會自動設定 `LC_ALL` 與 `LANG` 使用者環境變數
  - Linux, macOS 平台會提醒進行設定
- 使用 `-i` 或 `--interactive` 參數可進入互動模式
  - 每個設定會逐一詢問是否要執行
  - 可按 `y` 確認執行、`n` 跳過該設定、`q` 離開程式

## 設定內容

若為 Windows 平台，請記得使用 Command Prompt 執行以下命令，並跳過 Linux/macOS 專屬的命令。

```sh
git config --global user.name  ${name}
git config --global user.email  ${email}

# 設定打錯命令時 3 秒內會自動做出判斷
git config --global help.autocorrect 30

# 推送新分支時自動設定上游分支
git config --global push.autoSetupRemote true

# 設定預設分支名稱為 main
git config --global init.defaultBranch main

# 統一使用 LF 作為行尾字元，避免跨平台協作時的問題
git config --global core.autocrlf input
git config --global core.safecrlf false

# 為了能正確顯示 UTF-8 中文字
git config --global core.quotepath false

# 在命令列環境下自動標示顏色
git config --global color.diff auto
git config --global color.status auto
git config --global color.branch auto

# 常用的 Git Alias 命令
git config --global alias.ci   commit
git config --global alias.cm   "commit --amend -C HEAD"
git config --global alias.co   checkout
git config --global alias.st   status
git config --global alias.sts  "status -s"
git config --global alias.br   branch
git config --global alias.re   remote
git config --global alias.di   diff
git config --global alias.type "cat-file -t"
git config --global alias.dump "cat-file -p"
git config --global alias.lo   "log --oneline"
git config --global alias.ls   "log --show-signature"
git config --global alias.ll   "log --pretty=format:'%h %ad | %s%d [%Cgreen%an%Creset]' --graph --date=short"
git config --global alias.lg   "log --graph --pretty=format:'%Cred%h%Creset %ad |%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset [%Cgreen%an%Creset]' --abbrev-commit --date=short"
git config --global alias.alias "config --get-regexp ^alias\."
# 顯示建議的 .gitattributes 檔案內容（此 alias 由本工具自動設定，詳見下方說明）

# 必須是 Windows 平台才會執行以下設定
git config --global alias.ignore "!gi() { curl -sL https://www.gitignore.io/api/$@ ;}; gi"
git config --global alias.iac "!giac() { git init -b main && git add . && git commit -m 'Initial commit' ;}; giac"
git config --global alias.acp "!gacp() { git add . && git commit --reuse-message=HEAD --amend && git push -f ;}; gacp"
git config --global alias.aca "!gaca() { git add . && git commit --reuse-message=HEAD --amend ;}; gaca"
git config --global alias.cc  "!grcc() { git reset --hard && git clean -fdx ;}; read -p 'Do you want to run the <<< git reset --hard && git clean -fdx >>> command? (Y/N) ' answer && [[ $answer == [Yy] ]] && grcc"

# 必須是 Linux/macOS 平台才會執行以下設定
git config --global alias.ignore '!"gi() { curl -sL https://www.gitignore.io/api/\