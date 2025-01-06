+++
date = '2024-11-29T18:21:44+08:00'
draft = false
title = '關於git的常見指令'
tags = ['軟體教學','git','GitHub']
categories = ["軟體教學","git"]
+++

<img src="/images/article/aboutgit.jpg" alt="Forest" width="600px">
<br>
<p style="color:"><strong>內容提及:</strong>git的指令、如何上傳到GitHub、練習小遊戲</p>
<!--more-->
<section style="background:	#D0D0D0">
❥❥❥下面註記的內容：<br>
<ul>
<li><>:要加的內容</li>
<li>( ):解釋</li>
</ul>
</section>

<span style="color:red">注意</span>：分支切錯換名字就好了！！！！！分支就是個貼紙而已

# 一、在 vscode 可以安裝

git graph（可以在執行時，在終端機底下那一條看歷史紀錄）

# 二、一般指令

```py
* git help commit(可以看內建的commit有什麼呼叫的方式)
* git restore --staged +<file>(刪掉你不要上傳的資料夾)
* git log(看歷史紀錄，卡住不能動按Q)
* git checkout +<commit編號>(編號看git graph，或是右鍵點擊checkout回去剛剛的步驟，老指令，容易刪掉別的東西)
* git restore+<file>(把東西刪掉可以救回來)
* git branch (看指向的分支，＊main)
* git branch+<名字>（新增一個名字的分支，開分支做新功能）
* git branch+<名字>+<commit編號>(把不見得這條編號分支找回來)
* git switch+<名字>(切分支)
* git merge+<分支名字>(合併分支，是指把標籤撕起來貼到前面)
* git merge cc —no-ff -m’merge’（讓合併出現路徑）
* git blame <文章.html>(看誰寫的)
* git rebase+<名字>(我要在名字git的後面，把東西複製貼上，原本的在後面飄)
```

<img src="/images/article/gitrebase.jpg" alt="Forest" width="600px">

```py
* merge有留下歷史紀錄rebase不會有歷史紀錄所以乾淨整潔
* 使用merge發現內容重複產生衝突，去改文件再git add .&git commit -m “”
* 使用rebase發現內容重複產生衝突，去改文件再git add +<html>&git rebase --continue
* git reset +<commit編號>+ --mixed(把東西丟到工作目錄，為預設值)/--soft(把原本的東西放在暫存區)/--hard(把原本的東西刪掉)
* git reset HEAD^ (倒退一步＾＾倒退兩步)
* git reset HEAD~2(倒退兩步，~1倒退一步，~50倒退五十步)
* git reflog(看你剛剛移動的紀錄)
* Reset全刪掉被用了，那就用回來～～git reset+<commit編號>--hard
* git pull=git fetch+auto git merge(在git線上進度 超過編輯的進度)
* git clone +<別人的git開放式網站> （把別人的網址放編輯器加這串就不用下載）
```

# 三、Git 練習小遊戲

<a href="https://learngitbranching.js.org/?locale=zh_TW " target="_blank">遊戲連結</a>

# 四、上傳文章

先創好 GitHub 資料夾 project，會有以下指示，在編輯器終端機照著以下內容輸入。

```py
* git init(格式化)
* git status(巡視有什麼東西，看狀態)
* git add .(全部上車，staging area 暫存區，「.」是 here，「..」是上一層)
* git commit -m "first commit"(名稱可以自己打)
* git branch -M main(路徑)
* git remote add origin ＋<網址>(聯繫到你的git)
* git push -u origin main(去吧～把 main 分支推到origin)
```

# 五、如何上傳 github

日後想要更新你的文章，輸入以下指令就可以更新文章內容

```py
git status (讓電腦跟 github 確認剛剛寫了什麼檔案)
git add . (加入內容文章，空白點很重要)
git commit -m "你的備註"（確認載入）
git push (公開上 github)
```

push 完後，打開 GitHub 的資料夾，點選 action 看上傳進度（不會立刻傳），等一陣子後去 setting 看你的網站，就是更新成功了！
