---
published: false
author: Kent Chiu
layout: post
title: “Mac開發環境設定”
date: 2013-12-28 12:09
comments: true
sharing: true
footer: true
categories: 
- mac
---

mac os重裝，重裝後開發需要用到的工具及相關設定方式如下:

- JDK 1.7
- Apache Maven 3.1.1
- ruby
- python
- homebrew
- octopress
- postgresql


## 環境變數
設定環境變數可以在`.bashrc`跟`.bashfile`, 
為了保證不管透過遠端登入或在登入後另外開shell都會執行設定環境的動作，可以在`.bashfile`加入環境變數的設定，
但linux下的習慣以’.bashrc’為主，因為在linux環境下，反而是’.bashrc’會保證被執行到

為了保持跟linux的一致，以`.bashrc`為主要的設定檔，可以`.bash_profile`加入這段，讓它去執行`.bashrc`

	# ~/.bash_profile
	if [ -f ~/.bashrc ]; then
    		source ~/.bashrc
	fi

這樣就可以跟linux一樣，所有的設定放`.bashrc`即可

	# 設定 JAVA_HOME 
	export JAVA_HOME=`/usr/libexec/java_home -v 1.7`

	# 設定 MAVEN_HOME
	export MAVEN_HOME=/Users/kent/dev/idcview/toolchain/apache-maven-3.1.1

	# 輸出路徑
	export PATH=$JAVA_HOME\bin:$MAVEN_HOME\bin:$PATH

完成後，記得執行一下 `source ~/.bashrc`看看有沒有錯誤，然後便可以`echo $JAVA_HOME`看看java的部徑有沒有設定進去，
如果都ok，可以執行一下`java -version` 跟 'mvn -v'看一下java跟maven的版號


#### Eclipse找不到jdk
Oracle沒有定義jvm 1.7的相容性，所以在gui環境eclipse會找不到jdk
解決的方式如下

	# COPYjdk7的Info.plist出來修改
	cp /Library/Java/JavaVirtualMachines/jdk.1.7.<…>/Contents/Info.plist ~/Downloads/


Info.plist

	<key>JVMCapabilities</key>
	 <array>
	  <string>CommandLine</string>
	 </array>

改成

	<key>JVMCapabilities</key>
	 <array>
	  <string>JNI</string>
	  <string>BundledApp</string>
	  <string>WebStart</string>
	  <string>Applets</string>
	  <string>CommandLine</string>
	 </array> 

之後再copy回去原來的地上，改完需重新登入或重開機，之後eclipse便可以執行了	 

