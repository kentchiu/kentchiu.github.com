---
author: Kent Chiu
published: true
layout: post
title: "Subversion Installation"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - centos
---



This articles is a guild for installing Subversion (a.k.a SVN) in
CentOS5.

Requirement
===========

1.  apache server 2.x

Install
=======

### Install Subversion and mod\_dav\_svn

Install Subversion server and WEBDev module of Apache - mod\_dav\_svn

```
yum install subversion mod_dav_svn 
```

mod\_dav\_svn is a module of Apache server, we need it for accessing svn
via http(s) protocol.

### Create Repository

Using following command to create a repository home dictionary.

In this case, we create repository under ”/opt/svn” folder.

```
svnadmin create /opt/svn/repos 
```

### Modify Apache Configuration

Add a location section into Apache config file, it makes web server
accessing /opt/svn/repos directory as public folder.

```
vi /etc/httpd/conf/httpd.conf
#appending flowing section
 
<Location /svn/repos>
  DAV svn
  SVNPath /opt/svn/repos
  AuthType Basic
  AuthName "Subversion repository"
  AuthUserFile /opt/svn/password
  Require valid-user
</Location>
```

### Changing Directory Owner

```
chown -R apache.apache /opt/svn
```

### Add Users For Accessing SVN

```
# create a new user in password file under /opt/svn with argument -c
htpasswd -c /opt/svn/password Account1 
# appending new users by htpasswd WITHOUT -c argument
htpasswd /opt/svn/password Account2
```

you can see all user list in password file under /opt/svn/password, if
you would like to disable a user, just put '\#' in front of it.

\<code note\> if you got permission denied when committing, you may
check the SELinux config,disabled it when you are still encoutting the
permission problem. \</note\>

Verify
======

open this url in web browser - <http://SVN_SERVER_IP/svn/repos>, it will
ask you an account and password.

