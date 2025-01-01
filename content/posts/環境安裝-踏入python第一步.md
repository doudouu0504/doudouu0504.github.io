+++
date = '2024-11-17T18:46:07+08:00'
draft = false
title = '環境安裝-踏入python第一步'
tags = ['環境安裝','教學','Django']

+++

<img src="/images/article/python.jpg" alt="Forest" width="600px">
<p style="color:"><strong>內容提及:</strong>REPL、pyenv、Vscode、venv、poetry</p>
進入python領域之前，可以先把套件安裝下載後學習，就可邊學習邊實作練習。
<!--more-->
<hr>
<p>（以下是使用mac筆電）</p>

# 一、到網站上找 Python，安裝完後進行下一步

<p>在內建開終端機並輸入以下內容:</p>

```py
1、which python3 #可以看位置
2、python3 #此時會出現>>>，這是一個REPL環境
3、exit() or 按control+d #可以離開環境
```

# 二、了解何謂 REPL

<p><strong>REPL</strong>(Read-Eval-Print-Loop):</p>
<p>在這個環境底下會<span style="color:red">讀取</span>輸入的指令，然後<span style="color:red">評估</span>是否可執行。</p>
<p>如果可以就把結果<span style="color:red">印出來</span>，不行就會出現錯誤的訊息。</p>
<p>最後不管能不能執行，都會進入下一個<span style="color:red">指令</span>的迴圈。</p>
<p>（適合跑簡單的程式）</p>

# 三、安裝 pyenv

<p>pyenv是什麼：是一個工具箱，可以讓同一台電腦裡面，安裝不同版本的python的工具。</p>
<a href="https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv" target="blank">安裝方式參考</a>

<p>在終端機輸入以下內容，並分別為：</p>

```py
1、which pyenv #看pyenv的位置
2、pyenv #出現指令
3、pyenv -v #出現版本
4、pyenv versions #列出透過pyenv安裝的python版本
5、pyenv install --list #列出所支援的內容
6、pyenv install 3.12.4 #下載版本，可下載最新的
7、pyenv global 3.12.4 #可以改成預設值，此時就可以隨意打python不用加3
8、pyenv shell 3.12.4 #若沒改成預設值，可以用叫的叫出這個版本
```

# 四、安裝 Vscode

<p>常用快捷鍵：</p>

```py
Command+Shift+P #輸入指令
Control+backtick #開啟終端機
Command+D #往下選取
Option+Shift+往下按鍵 #複製整行
直接再游標那行按複製 #可貼上到任一游標處
Option+游標處＋上下鍵 #可移動整行
Command+/ #快速註解
```

# 五、第一個程式如何開始

<p>在創建資料夾後...</p>
<p>1、內建終端機執行python，輸入：</p>

```py
cd #拉取資料夾進來，並按enter
ls -al #可查看資料夾內容
python3 + 檔名 #例如：python3 hi.py
```

<p>2、Vscode終端機執行</p>

```py
拉取資料夾進來
pwd #印出目前位置(print working directory)，確認在對的資料夾
python3 + 檔名 #例如：python3 hi.py
```

# 六、在 Vscode 做文字註解

<p><span style="background-color:rgb(0,0,0,0.1)">#</span> >>> 後面可以接描述</p>
<p><span style="background-color:rgb(0,0,0,0.1)">Command + / </span>>>>可以快速選取的範圍註解</p>

# 七、安裝套件

<p>之後做專案，要安裝套件可以這麼做：</p>
<p>先到<a href="https://pypi.org/" target="_blank">PyPI</a>的網站，利如要下載request套件，搜尋request。</p>
<p>複製<span style="background-color:rgb(0,0,0,0.1)">pip install request</span>，
到Vscode的console打<span style="background-color:rgb(0,0,0,0.1)">pip3</span>，並安裝。</p>
<p>輸入：<span style="background-color:rgb(0,0,0,0.1)">pip install request</span>，就安裝成功。</p>
<p>卸載套件輸入：<span style="background-color:rgb(0,0,0,0.1)">pip uninstall request</span><br>(但不會全部刪掉，會有混雜的套件留存，造成空間浪費)</p>
<p>可以輸入：<span style="background-color:rgb(0,0,0,0.1)">pip list</span>看空間內容。</p>

# 八、虛擬環境 venv

<p>目的：不會影響其他專案，因為同樣套件只能安裝一個版本，所以沒加會覆蓋。</p>
<p>指令如何下(在Vscode，有兩種方式)：</p>

```py
1、python -m venv kitty
#-m是指載入，venv是內建虛擬模組，kitty是自己取的虛擬環境名字
2、python -m venv kitty --prompt="hello"
#如同上述，不過這行--prompt="自己取名字"，為Vscode開啟終端機時「名稱」會變hello
```

<p>啟用虛擬環境，輸入以下：</p>

```py
1、python -m venv kitty
#在Vscode開啟終端機，輸入以上載入虛擬環境
2、source kitty/bin/activate
#左側會出現一個kitty資料夾，裡面有一些資料夾，在使用虛擬環境時，不太管它。
#但啟動虛擬環境，裡面有一個bin資料夾，底下有activate，執行輸入以上
3、pip install django
#安裝套件輸入上述(不會滲透出去，例如安裝django)
4、pip list
#若要看內容，領域展開，輸入以上
5、pip freeze #會出現已經安裝的套件及指定版本，而輸出成文字檔可打：
pip freeze > requirements.txt #之後會出現在左側，這是套件列表。
pip install -r requirements.txt #若要呼叫列表時輸入
6、deactivate #離開環境時輸入
```

# 九、虛擬環境 poetry

<p>目的：可安裝並管理套件版本、打包、發布套件功能。</p>
<p>可以到poetry網站看下載教學，下載流程輸入：<span style="background-color:rgb(0,0,0,0.1)">pip install poetry</span></p>
<p>指令如何下(在Vscode，有兩種方式)：</p>

```py
1、poetry new kitty
#在Vscode終端機輸入以上，就會長出資料夾，kitty是自己取的虛擬環境命字
2、#先在Vscode自己建立一個資料夾(例如：hello)，並依照下列方式：
cd hello #在終端機切到此資料夾
pwd #確認輸入位置沒錯
poetry init #載入虛擬環境，一路按Enter，結束會多一個pyproject.toml
poetry install #創一個.venv資料夾
```

<p>啟用虛擬環境及其他指令，輸入以下：</p>

```py
1、poetry shell #先切換到虛擬目錄裡面，在poetry執行
2、poetry add requests #安裝套件，例如套件名稱為requests
3、poetry remove requests #移除上面套件，或是可以刪掉資料夾
4、pip list #看內容
5、poetry show #可以看裝的套件及版本資訊（較詳細）
6、poetry show --tree #看相異性及套件細節
7、poetry add pre-commit --group dev #裝到不同群組裡佈置時使用
8、poetry env info #看poetry位置裝到哪
9、poetry env remove 位置 #刪除指定位置虛擬環境
10、poetry cofing --list #檢查內容
```

# 十、語意化版本

```py
"^5.0.3"  #^是中間數字0有跳動，會自動抓到新版本，是部分更動，升級中間的版本
"~5.0.3"  #~是3有跳動，只裝這個後面的，算是小地方更動
">=5.0.3" #只要大於等於這個版本就安裝
"==5.0.3" #不升級，這個就好！
```
