---
author: Kent Chiu
published: true
layout: post
title: "Setup Git Server"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - git
---




In this article, we will setup a git server from team access. It is
mimic the [SVN style
workflow](http://whygitisbetterthanx.com/#any-workflow "http://whygitisbetterthanx.com/#any-workflow")(Centralized
Workflow), it's a easy way for integrating all code in one place.

Main Steps
==========

1.  [init bare
    repository](#init_bare_repository "git:setup_git_server ↵")
2.  [add user](#add_user "git:setup_git_server ↵")
3.  [first commited](#first_commited "git:setup_git_server ↵")
4.  [clone](#clone "git:setup_git_server ↵")

#### init bare repository

create a bare repository in /opt/git/project.git


```
$ git --bare init
Initialized empty Git repository in /opt/git/project.git/

```

#### add user

add a user called 'git' for executing git commonds.


```
$ adduser git

```

change git's password


```
$ passwd git

```

#### first commited


```
# on SomeBoday's computer
$ cd myproject
$ git init
$ git add .
$ git commit -m 'initial commit'
$ git remote add origin git@gitserver:/opt/git/project.git
$ git push origin master

```

#### clone


```
git clone git@gitserver:/opt/git/project.git

```

