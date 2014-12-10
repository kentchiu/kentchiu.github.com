---
author: Kent Chiu
published: true
layout: post
title: "Add New Virtual Host"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - apache
---





```
...
# remember to uncomment  NameVirtualHost 
NameVirtualHost *:80
...
<VirtualHost *:80>
  ServerName svn.domain.com.tw
  ServerAdmin admin@domain.com.tw
  DocumentRoot "/opt/svn/repos"
  <Location />
    DAV svn
    SVNListParentPath on
    SVNParentPath /opt/svn/repos
    AuthType Basic
    AuthName "Subversion repository"
    AuthUserFile /opt/svn/password
    AuthzSVNAccessFile /opt/svn/access
    Require valid-user
  </Location>
</VirtualHost>
 
RedirectMatch ^(/svn)$ $1/
 
<VirtualHost *:80>
  ServerName site1.domain.com.tw
  ServerAdmin admin@domain.com.tw
  DocumentRoot "/var/www/html/site1"
  alias /site1 "/var/www/html/site1"
  <Directory "/var/www/html/site1">
        AllowOverride all
        Order Deny,Allow
  </Directory>
</VirtualHost>
 
<VirtualHost *:80>
    ServerName issue.domain.com.tw
    ServerAdmin admin@domain.com.tw
    DocumentRoot "/var/www/html/mantis"
</VirtualHost>

```

remember to uncomment NameVirtualHost, like this NameVirtualHost \*:80

