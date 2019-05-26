
# 參考

[莫凡 git](https://morvanzhou.github.io/tutorials/others/git/4-3-rebase/)

[Git-Tutorials](https://github.com/twtrubiks/Git-Tutorials)

參考網路資源，仿作並記錄

# 目錄

- [參考](#參考)
- [目錄](#目錄)
- [創建](#創建)
    - [Git 設定資料](#git-設定資料)
    - [first 首先](#first-首先)
        - [git init](#git-init)
        - [git clone](#git-clone)
        - [git fork](#git-fork)
- [查看](#查看)
    - [查看記錄](#查看記錄)
- [修改](#修改)
    - [暫存文件 git add](#暫存文件-git-add)
    - [提交文件 git commit](#提交文件-git-commit)
    - [git checkout -- file](#git-checkout----file)
        - [reset](#reset)
- [git push 提交至遠端](#git-push-提交至遠端)
- [刪除](#刪除)    - [確定要從版本庫中刪除](#確定要從版本庫中刪除)
    - [作業時刪錯](#作業時刪錯)
- [分支 branch](#分支-branch)
    - [遠端分支](#遠端分支)- [合併](#合併)
    - [git merge](#git-merge)
- [git pull](#git-pull)
    - [git pull --rebase](#git-pull---rebase)
- [git rebase](#git-rebase)
    - [git rebase interactive](#git-rebase-interactive)
        - [reword](#reword)
        - [edit](#edit)
        - [未嘗試](#未嘗試)
- [git cherry pick](#git-cherry-pick)
    - [衝突](#衝突)
    - [git revert](#git-revert)
- [解決衝突](#解決衝突)
- [git stash 暫存](#git-stash-暫存)
- [git show](#git-show)
- [git grep](#git-grep)
- [git 其他設定](#git-其他設定)
- [push 多個遠端](#push-多個遠端)


# 創建

## Git 設定資料

    git config --global user.name
    git config --global user.email

## first 首先

### git init

    git init <directory>

### git clone 

如何改善(加速)大型 repo git clone 速度

可以透過 --depth 這個參數來完成，簡單說明一下他的功能，當我們一般執行 clone 之後，

接著執行 git log 你會發現有大量的 log，在某修情況下，你可能不需要那麼多的 log，

也就是說你可能只需要最近 10 筆的 history commit，甚至你只需要 1 筆 ( 也就是根本不需要

history commit )，這時候就很適合使用 --depth

    git clone git@github.com:django/django.git --depth 1


但是會有一個問題，當嘗試切換 branch git checkout stable/2.2.x

( 你會發現你無法切換 remote branch 

原因是因為使用 --depth 相當於是 --single-branch，

所以當然沒有其他的 branch。 )

為了解決這個問題，比較好的做好應該是這樣

    git clone git@github.com:django/django.git --depth 1 --no-single-branch

( 這個和 --single-branch 比會稍微久一點點，因為每個 branch 的最新一個 history commit 都要 clone 下來 )

這樣的話，就可以保留 remote 的 branch 了

最後稍微整理，

如要 clone 最近一次的 history，而且也需要其他 branch，使用如下，

    git clone git@github.com:django/django.git --depth 1 --no-single-branch

如要 clone 最近一次的 history，而且不需要其他 branch，使用如下，

    git clone git@github.com:django/django.git --depth 1 --single-branch

or

    git clone git@github.com:django/django.git --depth 1

[git clone Doc](https://git-scm.com/docs/git-clone)

### git fork



# 查看

查看目前的 repository ( repo 容器 )

    git status 

如果想要查看這次還沒 add (unstaged) 的修改部分 和上個已經 commit 的文件有何不同, 使用

    git diff

也可以看 commits 之間的差異

    git diff commit1_id commit2_id

如果你已經 add 了這次修改, 文件變成了 “可提交狀態” (staged), 我們可以在 diff 中添加參數 --cached 來查看修改

    git diff --cached

## 查看記錄

    git log

按 小寫q 可退出

// "--oneline": 每個commit顯示在一行

    # 短
    241c439 (HEAD -> master, origin/master) first commit
    
    git log --oneline    

    # 長
    241c439... (HEAD -> master, origin/master) first commit

    git log --pretty=oneline

    # 類似短 附加時間-使用者

    git log --graph --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit --date=relative

    使用後以 git log --oneline 較為好看簡潔

# 修改

## 暫存文件 git add

    git add  

## 提交文件 git commit

    git commit -m "frist commit"

“-am”：添加所有改變直接commit

    git commit -am "change 3 in dev"

如何修改最後一次的commit呢 ?

有時候我們 commit 完之後，才發現自己的 commit 內容手殘打錯了

這時候可以使用如下指令，他會跳出編輯視窗給你編輯你上一次的 commit 內容。

    git commit --amend

// "--no-edit": 不編輯，直接合併至上一個 commit,結果同上
    
    git commit --amend --no-edit  

// 合併至上一個 commit ， 修改messmage
    
    git commit --amend -m "XXX"

[git commit Doc](https://git-scm.com/docs/git-commit)

## git checkout -- file

git checkout -- file 可以丟棄工作區的修改：

    git checkout  -- hello.py

命令 git checkout -- hello.py 意思就是，把 hello.py 文件在工作區的修改全部撤銷 ( 丟棄 ) ，

讓這個檔案回到最近一次 git commit 或 git add 時的狀態。

## reset

有時候我們會為了方便，直接使用下面的指令一次加入全部的檔案

    git add .

但是加完後發現其實有些檔案是不需要 add 進入的，這時候就可以使用如下指令去取消 add

    git reset HEAD <file>

如果現在要把目前版本退回到上一個版本，就可以使用 git reset 指令：

上一個版本就是HEAD~1，

    git reset --hard HEAD~1

如果要指定回到某個特定版本

    git reset --hard ad41df36b7

    --hard 這個參數，有三種選擇，分別為 --mixed( default ）--hard --soft，

    --hard 將之前的 commit 都丟掉（ 完全 不保留 ）。

    --soft 將之前的 commit 都丟掉，但 保留 你之前工作區的狀態。

    --soft 很適合使用在將多個無意義的 commit 合併成一個 commit。


版本號 ( ad41df36b7 ) 沒必要全部都寫，寫前幾位就可以了，Git 會自動去找。

當你退回到某個版本，突然隔天後悔了，想恢復到之前的新版本該怎麼做呢?

找不到新版本的 commit id 該怎麼辦呢?

這時候就可以使用一個指令

    git reflog

接著看你要回到哪個版本，再使用 git reset 即可。

    git reset --hard 642e7af

# git push 提交至遠端

有時候想消除( 覆蓋 )已經 push 出去的 commit，這時候我們可以使用

    git push --force
    git push -f

可以強制 push。先回到某個版本，然後再強制 push。

注意！在多人專案共同開發時，盡量不要用 --force 這種方法，因為有時候會害到別人，建議可以使用 revert 。

因為上面這個原因，所以建議用另一種比較安全的方式

    git push --force-with-lease

可以確保你沒有隨便丟掉別人的 commit。（ 如果有人比你早 commit push 上去，你就會無法 push 到 remote ）

# 刪除

## 確定要從版本庫中刪除

有兩種況狀，一種是確定要從版本庫中刪除該檔案，那就用命令 git rm 刪掉，並且 git commit：

    rm hello.py
    git rm hello.py
    git commit -m "remove hello.py"

## 作業時刪錯

另一種況狀是刪錯了，使用 git checkout 可以輕鬆還原檔案:

    rm hello.py
    git checkout -- hello.py

# 分支 branch

查看目前的分支：

    git branch

創建一個分支，bug1 分支 ( 名稱可以隨便取 )，然後切換到 bug1 分支：

    git branch bug1
    git checkout bug1

以上兩行指令，相當於下列一行指令

    git checkout -b bug1

我們在 bug1 分支上進行任何修改操作，

然後再把工作成果 ( 補充一下，修改任何內容後請記得使用 git add 指令和 git commit 指令 ) 合併到 master 分支上：

    git checkout master
    git merge bug1

如果順利合併 ( merge ) 完成後，就可以刪除 bug1 分支：

    git branch -d bug1

如果要丟掉一個沒有被合併過的分支，可以使用 git branch -D 分支名稱 強行刪除。

    git branch -D dev

[git branch Doc](https://git-scm.com/docs/git-branch#git-branch--m)

git branch 也可以修改名稱，而且 commit id 是不會改變的

    git branch -m <name>

## 遠端分支

如果是第一次使用 git clone ，你會發現你只有 master 分支 ，

這時候我們先查看遠端還有什麼分支，

    git branch -r

假設遠端有一個名稱為 develop 的分支，

我們只要 checkout 到該分支底下就可以了

    git checkout develop

# 合併 

## git merge

分支衝突時使用

解決衝突，目標檔案會有以下不同分支的情況
    
    <<<<<<< HEAD
    # edited in master
    ...
    =======
    # edited in dev
    ...
    >>>>>>> dev

手動更改後，add和commit

    git commit -am "solve conflict"

# git pull

通常在開始工作或要 push 之前，會先從遠端抓取分支，如果有衝突，要先解衝突。

可以先簡單想成 git pull = git fetch + git merge

## git pull --rebase

原因是因為當我們使用 git pull --rebase 造成衝突時，修好衝突的內容之後，git add xxxx，接著我們會

直接執行 git rebase --continue。

假設今天你執行了 git pull --rebase 之後，發現很難受 ，想要取消，

直接執行 git rebase --abort 即可回到之前的狀態。


# git rebase

先確定在分支上( checkout )，在rebase

    git checkout v2
    git rebase master

與merge不同在於，v2不是新增一個commit，而是v2及master歷史commit合併(變成同一條線)

## git rebase interactive

Commands:

    p, pick = use commit
    r, reword = use commit, but edit the commit message
    e, edit = use commit, but stop for amending
    s, squash = use commit, but meld into previous commit
    f, fixup = like "squash", but discard this commit's log message
    x, exec = run command (the rest of the line) using shell
    d, drop = remove commit

### reword

以交互方式重新綁定意味著您有機會編輯已重新生成的提交。您可以重新排序提交，並可以刪除它們（清除壞的或其他不需要的補丁）

commit id 424fd22 : Second commit

想改成 : Second commit rebase -i

所以現在要修正他，我們的目標 commit id 為 424fd22，指令為

    git rebase -i <after-this-commit>

after-this-commit 簡單說，就是要選當下的 commit id 的上一個

    741c01f v1-test
    424fd22 Second commit
    241c439 first commit

要改v1-test，就要打

    git rebase -i 241c439

接著，按 *i* 進入編輯模式，然後將目標改成 *r* 或是 reword 都可以，

    r 424fd22 Second commit

接著按Esc退出編輯模式後，輸入 *:wq*

選擇(E) 開始編譯，修改後(Second commit rebase -i)，

按Esc退出再輸入 *:wq*，完成

如果我希望修改第一個 commit 該怎麼辦

    git rebase -i --root

### edit

假設要這之中，加入一個commit

    741c01f v1-test
    add commit             <---
    424fd22 Second commit
    241c439 first commit

先找目標
    
    git rebase -i 241c439

接著，按 *i* 進入編輯模式，然後將目標改成 *e* 或 edit

    e 424fd22 Second commit

### 未嘗試

* s, squash = use commit, but meld into previous commit
* f, fixup = like "squash", but discard this commit's log message

簡單一點，單純想要忽略某一個 commit message 時，使用 fixup，

想要合併 commit 並修改 commit message 時，使用 squash。

* x, exec = run command (the rest of the line) using shell

測試用

    pick 424fd22 Second commit
    x echo "test"
    pick 241c439 first commit
    x error

根本沒有 error 這個指令，

當如果執行到 shell 有錯誤時，他會停下來，讓你修正

修正完了之後，再執行

    git rebase --continue

* d, drop = remove commit

移除這個 commit ( 包含 commit 內容 )


[git rebase -i Doc](https://git-scm.com/docs/git-rebase#_interactive_mode)

# git cherry pick

只要某個分支的某個commit

    git cherry-pick 14dee93

如果你想要一次撿很多的分支過來也是可以，直接使用空白隔開即可

    git cherry-pick 14dee93 xxxxxx xxxxxx xxxxxx xxxxx

## 衝突

使用 git status 看一下狀態

修正後，執行 git add 

執行 git cherry-pick --continue，就時候會跳出一個編輯視窗

輸入完 commit message 之後，再輸入 :wq

# git revert

假設我 commit history 為 

    A1 -> A2 -> A3 -> A4 -> A5 -> A6

我現在想要回 A4 這個 commit , 這時候我就可以使用 git revert

    git revert A4

假如你再看現在的 commit history , 他會長的像這樣

    A1 -> A2 -> A3 -> A4 -> A5 -> A6 -> A4_revert

使用 git revert 的好處，就是可以保留 commit history , 萬一你又後悔了，

也可以在 revert 回去

# 解決衝突

git status 可以告訴我們衝突的文件

修改衝突 conflicts，然後再加個 commit

git add Hello.py
git commit -m "conflict fixed"

放棄合併

    git merge --abort

或退回reset

    git reset --hard HEAD

# git stash 暫存

現在突然有一個bug必須馬上 ( 立刻 ) 處理，但是，啊我手上的事情還沒做完阿~~~~ 這時候，可以利用以下指令

    git stash

暫存現在的程式

假如你想要更清楚自己這次的 stash 原因是什麼，或是這是正在開發什麼功能 可以使用以下指令

    git stash save -u "我是註解"

可以使用下列的指令來觀看 stash 裡面的東西

    git stash lis

取回暫存，也會刪除 stash

    git stash pop

假設今天你有很多的 stash，你可以指定，如下

    git stash pop stash@{0}

如果你希望使用 stash 取回之後，不希望刪除 stash ，可以使用下列的指令

    git stash apply

如果你只是想要刪除暫存，可以使用下列的指令

    git stash clear

如果你想丟棄指定的 stash，可以使用

    git stash drop stash@{0}

# git show

一般來說，只用他來看這個 commit 修改了哪些東西

# git grep

簡單說，就是可以幫你找出符合的 pattern，舉個例子，我希望找出內容

有包含 hello 這個 pattern 的檔案，這時候，就可以執行以下指令

    git grep "hello"

a.py:print("hello world")
b.py:print("hello")

# git 其他設定

Git 工作區的根目錄下新建一個特殊的 .gitignore 文件 ，

然後把要忽略的文件 ( 檔案 ) 名稱輸入進去， Git 就會自動忽略這些文件。

當然不需要自己從頭寫 .gitignore 文件， GitHub 已經幫我們準備了一些文件

[.gitignore](https://github.com/github/gitignore)

.gitignore 檔案格式範例

    # 註解
    *.bak
    *.log

簡寫，讓 Git 以後打 git st = git status，常用指令

    git config --global alias.st status

    git config --global alias.br branch

    git config --global alias.ck checkout

    git config --global alias.cm commit

    git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold bl

設定檔文件位置

通常會在你的使用者底下，例如我這台電腦使用者為 HJ，設定檔文件就會在 C:\Users\HJ 底下，

他是一個 隱藏文件.gitconfig

不知道大家有沒有注意到 --global 這個參數，他代表的意思是全域的，如果說你今天是執行

    git config alias.stu status

代表只有在該目錄底下時才會有作用。

那這個有什麼用呢？ 試想一種情境，假設你在特定的資料夾底下，想要使用特定的信箱去 push，而其他的資料夾，

則一樣使用公司的信箱，這時候，就非常適合使用這種方法完成。

更多資訊細節可使用以下命令查看

    git config -l

# push 多個遠端

這裡用 Bitbucket 當作範例

先使用下方指令查看

    git remote -v

接著我們使用下列指令新增一個 origin 的遠端

    git remote set-url --add origin <url>

重新命名 remote，語法如下，

    git remote rename <old> <new>

刪除 remove

    git remote remove <name>

新增 add

    git remote add origin <url>

如果我們想修改 origin 的 url，可以使用

    git remote set-url origin


[git remote Doc](https://git-scm.com/docs/git-remote)









