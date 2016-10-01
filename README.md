# 使用 Git 版本控制管理 LabVIEW 程式碼

本文介紹如何使用 Git 版本控制管理 LabVIEW 程式碼。

## 如果你不懂 Git 先來這

學習：

- [Git 如何上手](https://www.atlassian.com/git/tutorials/setting-up-a-repository)（英文）
- [ProGit 電子書](https://git-scm.com/book/zh-tw/v1/Git-基礎)（中文翻譯）

## 前言

Git 版本控制在處理文字類型的檔案非常方便，進行比較（diff）與合併（merge）操作直覺又簡單。當用於處理 LabVIEW 這種二進位（binary）程式檔案時，問題便產生了。即使 LabVIEW 環境本身提供了 LVDiff 與 LVMerge 工具，仍存在一些無從克服的缺點。

文中會介紹如何設定 LVDiff 與 LVMerge 工具，以及避免潛在的會造成問題的 Git 操作。

## 設定 TortoiseGit

### 安裝 TortoiseGit

安裝 Git 主程式（[下載頁面][git-for-win]）。完成後，再安裝 TortoiseGit （[下載頁面][tortoisegit]）。

[git-for-win]: https://git-for-windows.github.io
[tortoisegit]: https://tortoisegit.org/download/

TortoiseGit 是一套 Git 的 GUI 工具。本文將利用 TortoiseGit 設定 LabVIEW 的 LVDiff 與 LVMerge 工具。

![TortoiseGit](img/tortoisegit.png)

### 設定 LVDiff 比較器

在任意 Git 版本庫目錄底下，點右鍵開啟選單，依下列指示設定 LVDiff 比較器：

```
TortoiseGit >> Settings >> Diff Viewer >> Advanced... >> Add...
```

將 Extension 欄位設定為：

```
.vi
```

External Program 欄位設定為該路徑：

```
<NI 軟體的安裝路徑>\shared\LabVIEW Compare\LVCompare.exe
```

例如：

```
C:\Program Files (x86)\National Instruments\Shared\LabVIEW Compare\LVCompare.exe
```

### 設定 LVMerge 合併器

設定方法與上述類似。在任意 Git 版本庫目錄底下，點右鍵開啟選單，依下列指示設定 LVMerge 合併工具：

```
TortoiseGit >> Settings >> Merge Tool >> Advanced... >> Add...
```

將 Extension 欄位設定為：

```
.vi
```

External Program 欄位設定為該路徑：

```
<NI 軟體的安裝路徑>\shared\LabVIEW Merge\LVMerge.exe
```

### 完成 TortoiseGit 設定

完成上述設定後，在 TortoiseGit 介面下對 LabVIEW 的 .vi 檔進行比較與合併操作時，會分別開啟 LVDiff 比較器與 LVMerge 合併器。

### 設定 Git Bash 的比較與合併工具

若要在 Git Bash 下對 LabVIEW 的 .vi 檔開啟外部的 LVDiff 或 LVMerge 工具，設定較繁瑣。請參考 [ProGit 電子書 章節 8.1][progit]（英文）。

[progit]: https://git-scm.com/book/zh-tw/v2/Customizing-Git-Git-Configuration#External-Merge-and-Diff-Tools

## LabVIEW + Git 操作建議

1. __以 LabVIEW 專案（project）來管理 .vi 檔__：將 LabVIEW 專案以及 .vi 檔置入「單一」資料夾內，由 LabVIEW 專案介面統一管理。
2. __謹慎處理 .vi 相依性問題__：如果有用到第三方的 vi 函式庫，將之以次模組（Git Submodule）或子資訊夾的形式儲存在相同目錄中。
3. __LVDiff 與 LVMerge 的使用限制__：至目前為止（2015 年中），LVDiff 與 LVMerge 只能對「單一」 .vi 檔使用。另外，這兩個工具「無法」用在 LabVIEW 專案檔、XControl、物件的類別檔與函式庫檔。
4. __避免 Git 的衝突操作（conflict）__：LabVIEW 上的 Git 衝突操作隱藏很多風險，可以的話，讓所有合併（Merge）的情境都屬於「快進操作」（fast-foward）：
	1. 新的功能應模組化於另一個 .vi 檔上，並以 subVI 的方式呼叫（適用於平行協作）
	2. 若要在單一檔案中併入其它更新，（待補…）

## 相關資料（多數是英文）

- (GitHub) [LabVIEWGitEnv 環境設置工具](https://github.com/joerg/LabViewGitEnv)
- (GitHub) [LabVIEW Hacker 的工具組](https://github.com/labviewhacker)
- (NI Community) [LabVIEW 使用 Git/GitHub 開發流](https://decibel.ni.com/content/docs/DOC-37417)
- (NI Community) [Git 使用者群組](https://decibel.ni.com/content/groups/git-user-group?view=overview)
- (NI Community) [使用 Git+LabVIEW 的優劣分析](http://forums.ni.com/t5/LabVIEW-Idea-Exchange/Improve-LVMerge-and-LVDiff-for-integration-with-third-party-SCM/idi-p/1432322)
- (NI Community) [各種版本控制工具的比較討論](https://decibel.ni.com/content/blogs/Matthew.Kelton/2011/09/30/labview-and-versionsource-code-control--introduction)
- (NI Support) [原始碼分離 source-separate](http://zone.ni.com/reference/en-XX/help/371361K-01/lvconcepts/saving_vis_compiled_code/) 與 [如何設定](http://zone.ni.com/reference/en-XX/help/371361K-01/lvhowto/separate_compiled_code/)
