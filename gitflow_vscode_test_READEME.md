
# gitflow 

git init
git flow init

輸出:

    Branch name for production releases: [master]
    Branch name for "next release" development: [develop]

    How to name your supporting branch prefixes?
    Feature branches? [feature/]
    Bugfix branches? [bugfix/]
    Release branches? [release/]
    Hotfix branches? [hotfix/]
    Support branches? [support/]
    Version tag prefix? []
    Hooks and filters directory? [D:/CODE/GitHub/Git_test/gitflow_vscode_test/.git/hooks]

# Git-Flow Model
在 Git-Flow 中，有兩個主要的分支：

*Master*

這個分支是非常穩定的，任何時刻都是 production-ready ，包含最新的 release ，也就是產品的原始碼，當任何一個人使用你的產品，第一眼看到的都是 master，我們不能直接在 master 分支上進行任何的 commit ，只能從 Release 或 Hotfix merge 回來。

*Develop*

開發中的分支，develop 分支是由 mater 分支分出來的，該 ( develop ) 分支提供整合不同即將 release 的 feature ( 功能 ) ，所以該分支有可能不穩定 ( 有bug ) 。通常我們也不會直接在該分支下 commit，而是透過 merge 的方式將 feature ( 功能 ) merge 進來。

Master 以及 Develop 這兩個分支非常重要，理論上要保護好這兩個分支，避免被意外刪除。

除了 Master 以及 Develop 這兩個分支之外，Git-Flow 還提供其他的分支：

*Feature*

功能開發，該分支是由 develop 分支分出來的，最後會被合併回 develop 分支。舉個例子，假設目前有 A 和 B 兩個功能經由兩個人下去開發，這兩個人會分別從 develop 分支分 A 和 B 兩個分支出來，當完成後，再 merge 回 develop 。

*Release*

當我們認為 develop 已經是一個很穩定的版本時，我們會進入 release，該分支是由 develop 分支分出來的。通常我們會在該分支底下再做一次全面的測試以及上線前的準備 ( 發佈版本的記錄 )，完成 release 後，我們會 merge 回 master 以及 develop。

*Hotfix*

該分支是由 master 分支分出來的，通常是已經上線了，但突然發現一個非常緊急的 bug ，這時候我們就會開一個 Hotfix 出來修復該 bug ，完成後我們會再 merge 回 master 以及 develop。

https://www.xiaolaiwo.com/how-to-use-git-and-git-flow-in-vscode.html


git flow feature start feature-test-1

新增 gitflow_vscode_test\feature-test-1.txt

git add  

git commit feature-test-1

git flow feature finish feature-test-1

- The feature branch 'feature/feature-test-1' was merged into 'develop'
- Feature branch 'feature/feature-test-1' has been locally deleted
- You are now on branch 'develop'

git flow release finish 1.0

失敗 Fatal: Branch 'release/1.0' does not exist and is required.

git flow hotfix start bug1

- A new branch 'hotfix/bug1' was created, based on 'master'
- You are now on branch 'hotfix/bug1'

新增 gitflow_vscode_test\bug1.txt

Follow-up actions:
- Start committing your hot fixes
- Bump the version number now!
- When done, run:
    git flow hotfix finish 'bug1'

git add

git commit bug1

git flow hotfix finish bug1

# 參考

[VS code gitflow](https://www.xiaolaiwo.com/how-to-use-git-and-git-flow-in-vscode.html)

[gitflow Model](https://github.com/twtrubiks/Git-Tutorials/tree/master/Git-Flow)

# 嘗試後

對 git flow 有初步認識，但當案均消失，也有些地方失敗，可能有GUI會比較好