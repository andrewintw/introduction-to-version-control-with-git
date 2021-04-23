# Git & GitHub 前導知識


## 關於這份文件

這份文件是為了配合一些 Git/GitHub 教學所寫的前導文件。不會涉及太深入的版本管理議題，目的是讓你在學習 Git/GitHub 之前對一些背景知識以及名詞有些了解。


## 好吧，說真的 Git 到底是什麼?

要談 Git 之前，要先了解什麼是版本管理，以及它為何重要？

想像一下你是個正在寫論文的研究生。從題目訂好後你就新增了一份 WORD 文件，然後從碩一開始，每隔一段時間增加一點內容，直到碩二畢業前才寫出來。畢業之前，你把論文所有的文件、引用的文獻 pdf 檔、引用或自行繪製的說明圖片檔，全部放在同一個資料夾。且為了怕電腦掛點，所以狡兔要有三窟 -- 除了電腦要有一份，你還上傳一份到 Google 雲端硬碟，以及在隨身硬碟中也備份了一份。

由於寫論文的特性通常是 "越寫越完整"，因此如果你是碩二生，你通常不太會在意碩一時在哪個時間點所寫的內容。也就是說 "**版本**" 以及 "**如何管理版本**" 這件事情對你寫論文這件事情來說是不太重要的。你唯一要在意的可能只是確保這三窟的內容都一致。

但現在把情況換成軟體開發。軟體開發的狀況可就不是這樣的。假設今天你寫了 v1.0 版的軟體，一個禮拜後你新增了一些功能，釋出 v2.0 版之後。結果客戶發現 V2.0 有重大 Bug，所以你得「**再回頭去看看從 v1.0 到 v2.0 的這一個禮拜之中**」到底程式碼改了些什麼東西？因為如果 v1.0 是正常的，發生問題的原因通常會是在這個過程中所做的更動造成的。

上面的描述，就是所有軟體開發者的日常生活。你可以發現與寫論文的例子相比，軟體開發的過程中似乎更在乎「**何時改了什麼**」。甚至如果這個開發工作是多人合作的，則會再加上人的因素變成是 -- 必須在乎「**是誰？在何時？改了什麼？**」。

我們之所以會定義 "v1.0", "v2.0"，是為了用那些版本編號來替代說明檔案「**在當時的狀態**」。這裡的檔案泛指：程式碼、文件、圖片...等資源。所以如果 v1.0 是在 1/31 釋出的，那 v1.0 就代表你的程式碼在 1/31 當天的狀況。同理 v2.0 可能也代表例如 4/12 當天的狀況。所以只要你能用某種方法記錄下 1/31 ~ 4/12 之間你對程式更改了什麼？你就有機會從中發現問題並解決它。

這種「**紀錄檔案在哪個時間由誰更改了什麼**」的動作，就稱為「**版本控管**」，也有人稱為 SCM (Source Code Management)。而 git 就是用來協助你做版本控管的工具。


## 所以，我一定要用 Git 嗎？

Ｑ: 為什麼不直接使用 Google Drive？那古早的人們是怎麼做版本管理的？

其實版本管理的工具不是只有 git，而且還發展了好幾年了。如果你聽說 CVS, Subversion, SVK, BitKeeper... 其實他們也都跟 git 一樣也都是版本控管系統。

但在還沒有這些管理工具之前，據我 "聽說的"，許多軟體公司都使用 zip 檔 + excel 來管理。做法可能是在網路芳鄰（沒錯，當時應該沒有 google drive 這種雲服務）開一個共享資料夾。然後假如我是 Andrew，我下班前把我的程式碼壓縮成 project_andrew.01-31.zip 放在共享資料夾下，然後可能發信跟負責做整合的人說我改了什麼。

另外 Sabrina 或是 Zoe 可能也做了一樣的事情，所以網路芳鄰的共享資料夾下可能會有：

* project_andrew.01-31.zip # andrew 在 1/31 寫的程式進度
* project_andrew.01-31.txt　# andrew 在 1/31 修改的說明（給負責整合的人看的）
* project_sabrina.01-31.zip
* project_sabrina.01-31.txt
* project_zoe.01-31.zip
* project_zoe.01-31.txt

你可以發現上面的機制試圖以土砲的方式記錄「**是誰？在何時？改了什麼？**」！

假設當天負責整合的倒楣鬼是 Alice，Alice 就必須把 Andrew/Sabrina/Zoe 這三個人在 zip 檔案內寫的程式碼內容整合變成同一份。這個整合工具通常是透過文件比對軟體人工作業的。

Alice 做完整合工作後，就會把三個人的結果變成 project_merge.01-31.zip 同時寫一份 ChangeLog.txt 的文件，並在這份文件中整合 Andrew/Sabrina/Zoe 在各自提交的 .txt 檔中的更改說明。最終的產出是：

* project_merge.01-31.zip
* ChangeLog.txt # 也有人使用 excel 替代

這個版本管理的重點是，當隔天 2/1 上班時，所有參與開發工作的人，都需要去抓「**最新整合後(merged)的**」 project_merge.01-31.zip，然後再以這個版本為基礎 (有些人稱為: code base) 繼續開發。

你可以看到這裡最大的問題是 -- 整合 (merge) 工作幾乎都是人工作業的。負責整合的人需要有一定的能力，但最重要的其實是細心度。假如 Alice 忘記把 Andrew 寫的部分整合進去。則隔天大家的 code base 中就不會出現 Andrew 在 1/31 所寫的內容，就可能導致後續產生更多難以追蹤的錯誤。

另一個困難點是，假如 Sabrina 和 Zoe 好死不死剛好改到同一份文件那怎麼辦？如果負責整合的人無法看出這兩個人修改的意圖、無法判斷最終應該使用誰的版本、或是該怎樣融合兩者的修改，就得等隔天上班再把這兩位找來開會討論。

光是聽我這樣描述你應該能夠體會，以上的那些事情如果沒有版本控制系統的幫忙，開發者將會耗費許多成本在控管版本上。

而 Google Drive 就跟上面所說的網路芳鄰一樣，並沒有任何版本控管的能力。


## 版本控制系統的使用邏輯

git 這類的版本控制系統的使用邏輯都很類似。

首先它是 client-server 架構。意思是通常需要有個主機當作 git server，然後在你自己的電腦上使用 git client 工具與遠端的 git server 溝通。通常在 git 說明的文件中會將 git server 稱為 remote 端，而運作 client tools 的你的電腦則稱為 local 端。

```
local: git client tools <========> remote: git server
```

以下的說明會以 remote 和 local 代稱 server 與 local。

通常**首次**使用版本控制系統的時候，我們會先把目前最新的開發進度，例如: project_01-31 的進度都先上傳到 remote 端。所以你需要在 remote 端開一個放置當前最新程式碼的地方。這個 "地方"，git 稱為 repository。有人翻譯為檔案庫，也有人簡寫為 repo. 其實不用想得太複雜，使用上你所看到的也會是一整個像是名為 IoT_Project/ 的資料夾。只是這個資料夾會在 remote 端以 git repository 的檔案庫的方式存在。

所以開始使用版本控管的第一件事情，就是在 remote 端建立一個 git repository，然後給它一個名稱 (ex: IoT_Project) 然後把你在 local 目前最新的進度給上傳上去。順帶一提，在 git 中，把檔案從 local 端推到(上傳到) remote 端的行為，稱為 push。

```
1. remote 端建立一個 repository (ex: IoT_Project.git)
2. local 端將當前最新的狀況 "push" 到 remote 端
```

```
local === push ===> remote: IoT_Project.git
```

接著，隔天上班後，Andrew/Sabrina/Zoe 就可以把 remote 端的 IoT_Project repository 下載回來自己的電腦上 (local)修改。這個第一次所進行的下載動作，git 稱為 clone。從字面上很好理解，就是原封不動從 remote 端複製一份副本 (copy) 到 local 端。

所以 Andrew/Sabrina/Zoe 分別在自己的電腦上從 remote 端 clone 一份到自己的電腦上，然後開始進行當日各自的修改。

```
(Andrew)  andrew_copy  <=== clone === (remote: IoT_Project.git)
(Sabrina) sabrina_copy <=== clone === (remote: IoT_Project.git)
(Zoe)     zoe_copy     <=== clone === (remote: IoT_Project.git)
```

下班前，Sabrina 率先完成了工作，於是她把今天的進度再次 push 到 remote 端。假設 Sabrina 修改的部分表示為 sabrina_**diff**，一旦 Sabrina psuh 成功後，你可以想像 remote 端的 repository 狀態應該為 "IoT_Project.git **+ sabrina_diff**" ==> 也就是將今天 clone 下來後 Sabrina 所作的 "**修改部分**" push 到 remote 端。


```
(Andrew)  andrew_copy  
(Sabrina) sabrina_copy === push ===> (remote: IoT_Project.git + sabrina_diff)
(Zoe)     zoe_copy
```

然後換 Zoe 準備要下班，當她準備把自己今天所做的修改 zoe_diff push 到 remote 端的時候...精彩的部分來了!!

此時因為 remote 端目前最新的狀態是 IoT_Project.git + sabrina_diff。

而 Zoe 本地端(local)的 zoe_copy 狀態應該是 "IoT_Project.git + zoe_diff"。

所以當 Zoe 嘗試將 zoe_diff push 上 remote 時，git 會檢查 Zoe 的 local 和 remote 的狀態。當 git 發現 remote 的狀態已經更新過了 (被先 push 的 Sabrina 更改的)，所以 remote 比較新。git 會要求 Zoe 必須要「**先更新到跟 remote 端同步的狀態**」，然後才能 push 你的修改部分 (zoe_diff)。


```
(Andrew)  andrew_copy  
(Sabrina) sabrina_copy
(Zoe)     zoe_copy === push fail! ===>XXX (remote: IoT_Project.git + sabrina_diff)
```

而這個「**把 remote 端最新的狀況同步回 local 端的動作**」，git 稱為 pull (把最新的狀態從 remote "拉回" local)。

```
(Andrew)  andrew_copy  
(Sabrina) sabrina_copy
(Zoe)     zoe_copy <=== pull === (remote: IoT_Project.git + sabrina_diff)
```

一旦 Zoe 做了 pull，你可以想像 Zoe 的 local 應該等同於 "IoT_Project.git + sabrina_diff + zoe_diff"

```
(Zoe) IoT_Project.git + sabrina_diff + zoe_diff  <=== pull === (remote: IoT_Project.git + sabrina_diff)
```

所以當 Zoe 再次嘗試 push 的時候，git 就會發現 Zoe 的 local 端 (= IoT_Project.git + sabrina_diff + zoe_diff)，和 remote 端 (= IoT_Project.git + sabrina_diff) 只差在 zoe_diff 有差異而已，所以就讓 Zoe 把 zoe_diff push 到 remote 端。


```
(Zoe) IoT_Project.git + sabrina_diff + zoe_diff  === push ===> (remote: IoT_Project.git + sabrina_diff + zoe_diff)
```

等到 Zoe push 成功了。你可以想像得到 remote 端就會有今天一早的狀態 "IoT_Project.git" 加上 Sabrina 修改的 diff 和 Zoe 所修改的 diff。


同理，你可以想像，當 Andrew 下班的時候，他想 push 自己的 andrew_diff 之前，還是得先將 remote 端最新的狀態 pull 回自己的 lcoal 端，然後才能 push 自己的 diff。


以上就是所有版本控制系統最基礎的運作邏輯。


## 小故事: Git 怎麼誕生的

版本控制系統在大型專案上扮演非常重要的角色，例如像 Linux Kernel 這樣的超大型協作專案是非常需要的。早期 Linux kernel 的整合工作都是由 Kenel 的原作者 Linus Torvalds 以最原始的人工完成的。

後來 1999 年有個公司 BitMover 發展了一個版本控制系統 BitKeeper，並免費授權給 Linux 社群使用，條件則是不能試圖破解這個商用軟體。因為它分散式的特性讓 Linus 可以把些審查工作交給一些比較信任的開發者。2002 年開始，Linux Kernel 的版本管理基本上已全部由 BitKeeper 管理。

畢竟 BitKeeper 並非開源軟體，所以就有些開源基本教義派的人，像是 RSM 就會批評 Linus 不該使用一個非自由軟體來管理一個全世界最大的開源專案。但我想以 [Linus 的個性](https://www.youtube.com/watch?v=iYWzMvlj2RQ)應該是沒在鳥他的 XD

2005 年 Linus 當時所屬的公司的老闆 Andrew Morton 資助了一個專案，開始試圖對 BitKeeper 進行逆向工程破解，打算自製一個類似的開源工具。BitMover 公司後來知道這件事情，為了維護公司的利益就停止對 Linux 社群提供免費使用 BitKeeper 的授權。失去 BitKeeper 後，Linus 非常苦惱，他研究了當時所有的版本控制系統，但卻沒有一個是他滿意的。於是...

這位大神花了 10 天寫了約 1244 行程式，完成了 git 版本控制系統。之後這個系統就在往後的日子裡變成全世界最受歡迎的版本控制系統了。

2016 年，git 誕生的 10 年後，BitKeeper 也開源了，但我想早就已經沒人理它了 (幫QQ)。


## Git 和 GitHub 之間的關係是什麼?

git 於 2005 年問世後，GitHub 在 2008 年 2 月上線。剛剛提到 git 是個 client-server 架構。你可以把 GitHub 想像為一個幫你處理 git server 端事情的雲服務平台。你可以在 GitHub 所提供的網頁介面上，透過按鈕和簡單的設定建立 repository，也可以進行編輯、查看程式碼提交紀錄...甚至還可以討論 issue。簡單說就是原本你需要使用指令做的一些事情，現在只要在網頁上點一點、輸入一些設定值就可以完成。

因為 GitHub 扮演 git server 的角色，而因為 server 也是儲存 repository 的地方。等於你必須將你的程式碼 push 到 GitHub 的主機上，因此這類的服務也稱為「程式碼託管」平台。

GitHub 目前仍然是全世界最大的程式碼託管平台...也是全世界最大的工程師交友中心 (見下圖, 誤 XD)

![](https://live.staticflickr.com/6216/6251011880_5e5f9f3e74.jpg)


但同樣的服務不只 GitHub 這一家，事實上有另一個稱為 **GitLab** 的程式碼託管平台也很熱門。2018 年 6/4，微軟以 75 億美元的股票收購了 GitHub。當時有很多視微軟為邪惡帝國的 GitHub 使用者還舉家遷出，將程式碼轉移到 GitLab，網路上還戲稱是「逃難潮」XD


## Git GUI 工具做了什麼事情?

記得剛剛提到 git 是個 client server 架構。通常你需要找一台主機，在上面安裝 git server 來管理你的 repository。你也可以使用像是 GitHub 或 GitLab 這樣的雲服務來免去自己設設 git server 的麻煩事。

以上講的部分都是 remote 端管理的事情，但是對於 local 端 -- 也就是在你的電腦上，通常使用 git client tools 的方式就是透過 git 指令。

其實 git 的指令不難學，如果你的 repository 只有自己一個人在使用 (即只有你自己會 push 檔案到 remote)，事實上你只需要會 5 個 git 指令就好 (clone, add, commit, pull, push)。

但是這些指令需要在終端畫面中執行，(個人覺得) Linux 和 Mac 的終端環境比 Windwos 還好。但不是每個人都熟悉 Command Line 的使用方式。

因此就有 Git GUI Tool 這樣的視窗介面工具出現。目前比較有名的有

* Linux
	* 嗯...不想介紹，都用 Linux 了給我乖乖用 command line!
* Windows
	* Sourcetree (很多人推薦)
	* TortoiseGit
	* GitHub Desktop
* MacOS
	* SourceTree (很多人推薦)
* 跨平台 (Linux/Windows/Mac)
	* GitKraken (很多人推薦)

這裡不會介紹怎麼使用 GUI tool，至於該選用哪一個？最好的方式就是下載回來試用看看，你也可以配合自己的操作環境優先試用上面標示有 "很多人推薦" 的工具。

使用 GUI tool 的概念其實就是幫你 "封裝指令操作"。舉個例子，當你要把 windows 桌面上的 file.txt 檔案複製到 D 槽下時，你會怎麼做？

你應該會先用滑鼠點選 file.txt，按滑鼠右鍵叫出選單，點選 "複製"，然後開啟檔案總管到 D 槽目錄下，再次按滑鼠右鍵叫出選單，點選 "貼上"。

事實上這樣的動作在 windows 也有指令幫你做這件事

```
C:\Users\andrew\Desktop>cp recommect.txt D:\
```

所以你可以說這個複製檔案的行為，在 windows 下原本可以用 cp 指令操作，但 windows 將它包裝成讓你透過操作滑鼠的右鍵選單來做到。

同樣地，我們在使用 Git GUI tool 的時候，GUI tool 也會包裝那些 git 操作的概念。剛剛所提到的 clone、push、pull 的操錯概念，在 Git GUI tool 都會有對應的方式讓你不需要記指令就能操作。這就是使用 GUI tool 的意義。

補充: Git GUI Tool 的另一個好處是提供豐富的圖形化呈現方式，包含對 commit log 的呈現以及 branch 的關係圖。因此有些人既使熟悉 git 的指令操作也會安裝 GUI Tool 當作 viewer，用來看 code、看 commit log。

這邊提到的 commit log 是指開發者每次提交程式碼的時候，都需要寫下一段簡易的描述，說明這次修改了什麼？

而 branch 是版本控制系統中的一種管理方式，概念是你可以基於某一個狀態的程式碼建立一個分支 (branch)，這個分支版本通常可能是為了增加新的功能，或是解決 BUG，使用 branch 的用意在於不再修改的過程中更動主要的 code base。通常分支版本完成後，會再 merge 回主要的 code base。關於 branch 的管理屬於比較進階的議題，這裡不會提到。針對一般的個人專案與小型協作專案也不太會用到。 

![](https://miro.medium.com/max/1600/1*RTgn1s0GY8r0rSPsAzf8NQ.png)

~ END ~
