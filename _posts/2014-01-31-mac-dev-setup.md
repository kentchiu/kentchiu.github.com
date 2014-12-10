---
published: true
author: Kent Chiu
layout: post
title: “Mac開發環境設定”
date: 2013-12-28 12:09
comments: true
sharing: true
footer: true
tags: 
- mac
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




mac os重裝，重裝後開發需要用到的工具及相關設定方式如下:

- JDK 1.8
- Apache Maven 3.2.1
- ruby
- python
- homebrew
- octopress
- postgresql


## 環境變數
設定環境變數可以在`.bashrc`跟`.bashfile`, 
為了保證不管透過遠端登入或在登入後另外開shell都會執行設定環境的動作，可以在`.bashfile`加入環境變數的設定，
但linux下的習慣以’.bashrc’為主，因為在linux環境下，反而是’.bashrc’會保證被執行到

`vi ~/.bash_profile`


	function setjdk() {
	  if [ $# -ne 0 ]; then
	   removeFromPath '/System/Library/Frameworks/JavaVM.framework/Home/bin'
	   if [ -n "${JAVA_HOME+x}" ]; then
	    removeFromPath $JAVA_HOME
	   fi
	   export JAVA_HOME=`/usr/libexec/java_home -v $@`
	   export PATH=$JAVA_HOME/bin:$PATH
	  fi
	 }
	 function removeFromPath() {
	  export PATH=$(echo $PATH | sed -E -e "s;:$1;;" -e "s;$1:?;;")
	 }
	setjdk 1.8

	export MAVEN_HOME=/Users/kent/dev/apache-maven-3.2.1
	export M2_HOME=$MAVEN_HOME
	export CATALINA_HOME=/Users/kent/dev/apache-tomcat-8.0.5
	export GRADLE_HOME=/Users/kent/dev/gradle-1.10

	export PATH=$MAVEN_HOME/bin:$GRADLE_HOME/bin:$PATH

完成後，記得執行一下 `source ~/.bash_profile`看看有沒有錯誤，然後便可以`echo $JAVA_HOME`看看java的部徑有沒有設定進去，
如果都ok，可以執行一下`java -version` 跟 'mvn -v'看一下java跟maven的版號


#### Eclipse找不到jdk (JDK 7 only)
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

