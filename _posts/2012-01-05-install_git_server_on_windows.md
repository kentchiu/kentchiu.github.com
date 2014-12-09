---
author: Kent Chiu
published: true
layout: post
title: "在Windows上安裝Git Server"
date: 2012-01-05
comments: true
external-url:
sharing: true
footer: true
categories:
  - git
  - apache
  - windows
  - xampp
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



在Windows安裝git，讓每個經密碼認証後的user可以透過http pull跟push資料.

安裝環境如下:

1.  OS : WinXP
2.  Web Server: Apache (xampp-win32-1.7.7-VC9)
3.  Git Client: msysgit(Git-1.7.8-preview20111229-unicode.exe)

xampp解壓到c:\\xampp

git安裝到C:\\Program Files\\Git


```
copy C:\Program Files\Git\libiconv-2.dll C:\Program Files\Git\libexec\git-core
copy C:\Program Files\Git\libiconv2.dll C:\Program Files\Git\libexec\git-core

```


```
git init --bare TestProject

```

這下面這段加到最後面


```
Alias /git "c:/GitRepos"
<Directory "c:/GitRepos">
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order Allow,Deny
    Allow from all
</Directory>
 
#Set this to the root folder containing your Git repositories.
SetEnv GIT_PROJECT_ROOT C:/GitRepos
 
# Set this to export all projects by default (by default, 
# git will only publish those repositories that contain a 
# file named “git-daemon-export-ok”
SetEnv GIT_HTTP_EXPORT_ALL
 
# Route specific URLS matching this regular expression to the git http server. 
ScriptAliasMatch \
  "(?x)^/git/(.*/(HEAD | \
    info/refs | \
    objects/(info/[^/]+ | \
      [0-9a-f]{2}/[0-9a-f]{38} | \
      pack/pack-[0-9a-f]{40}\.(pack|idx)) | \
    git-(upload|receive)-pack))$" \
    "C:/Program Files/git/libexec/git-core/git-http-backend.exe/$1"

```

restart apache


```
#LoadModule dav_module modules/mod_dav.so
#LoadModule dav_fs_module modules/mod_dav_fs.so
 
# Distributed authoring and versioning (WebDAV)
# Attention! WEB_DAV is a security risk without a new userspecific configuration for a secure authentifcation 
Include "conf/extra/httpd-dav.conf"

```




