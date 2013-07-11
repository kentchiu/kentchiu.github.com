---
published: true
author: Kent Chiu
layout: post
title: "在 Windwos OS 上安裝git flow"
date: 2013-03-14 21:48
comments: true
sharing: true
footer: true
categories: 
  - git
  - git-flow
---


git-flow on windows主要是參考這篇文章
https://github.com/nvie/gitflow/wiki/Windows

## 下載相關檔案

git官網下載的git是屬於MSysGit版的，所以，要按照MSysGit的安裝步驟進行git flow的安裝

首先要先安裝`util-linux-ng for Windows`,`util-linux-ng for Windows`可以到
http://gnuwin32.sourceforge.net/packages/util-linux-ng.htm 下載，點選下圖紅框的檔案連結下次二進制文件的zip檔

![2013-03-14-install-git-flow-on-windows-001.png][]

需一併下載上圖中下方紅框的`libintl`，libintl的連結會進入另一個網頁 http://gnuwin32.sourceforge.net/packages/libintl.htm，一樣下載Binaries的zip檔即可


再來需要確定一下git安裝的路徑，如果是32位元的os會是在 `C:\Program Files\Git`, 64位元的os會是在 `C:\Program Files (x86)\Git` (**以下的步驟以64位元的os的git安裝目錄為例**)


下載後的 util-linux-ng-2.14.1-bin.zip 解壓，可以在`util-linux-ng-2.14.1-bin\bin`找到 getopt.exe
下載後的libintl-0.14.4-bin.zip解壓，可以在`libintl-0.14.4-bin\bin`找到 libintl3.dll

將getopt.exe跟 libintl3.dll copy到git安裝目錄 `C:\Program Files (x86)\Git` 

## clone git flow

clone github上的gitflow到臨時性的工作目錄 `c:\gitflow` (*這個目錄做完安裝後可以刪除*)


```
	git clone --recursive git://github.com/nvie/gitflow.git c:\gitflow
```


clone完成後，到 c:\gitflow\contrib 執行


``` 
	C:\gitflow\contrib>msysgit-install.cmd "C:\Program Files (x86)\Git"
```

執行後，會看到一些檔案被複制到git安裝目錄

```
	C:\gitflow\git-flow -> C:\Program Files (x86)\Git\bin\git-flow
	C:\gitflow\git-flow-feature -> C:\Program Files (x86)\Git\bin\git-flow-feature
	C:\gitflow\git-flow-hotfix -> C:\Program Files (x86)\Git\bin\git-flow-hotfix
	C:\gitflow\git-flow-init -> C:\Program Files (x86)\Git\bin\git-flow-init
	C:\gitflow\git-flow-release -> C:\Program Files (x86)\Git\bin\git-flow-release
	C:\gitflow\git-flow-support -> C:\Program Files (x86)\Git\bin\git-flow-support
	C:\gitflow\git-flow-version -> C:\Program Files (x86)\Git\bin\git-flow-version
	已複製 7 個檔案
	C:\gitflow\gitflow-common -> C:\Program Files (x86)\Git\bin\gitflow-common
	C:\gitflow\gitflow-shFlags -> C:\Program Files (x86)\Git\bin\gitflow-shFlags
	已複製 2 個檔案
	C:\gitflow\shFlags\src\shflags -> C:\Program Files (x86)\Git\bin\gitflow-shFlags
	已複製 1 個檔案
```

這樣整個安裝步驟就全部完成了

## 試車

可以在windows的command  windows裡打git flow，如果有看到類似以下的訊息就ok了

```
	C:\gitflow\contrib>git flow
	usage: git flow <subcommand>
	
	Available subcommands are:
	   init      Initialize a new git repo with suppo
	   feature   Manage your feature branches.
	   release   Manage your release branches.
	   hotfix    Manage your hotfix branches.
	   support   Manage your support branches.
	   version   Shows version information.
	
	Try 'git flow <subcommand> help' for details.
```

到這裡都ok的話，之前的臨時性的工作目錄 `c:\gitflow`可以刪了它


  [2013-03-14-install-git-flow-on-windows-001.png]: /images/blog/2013-03-14-install-git-flow-on-windows-001.png