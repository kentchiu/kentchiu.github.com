---
author: Kent Chiu
published: true
layout: post
title: "Solaris ,RHEL ,Ubuntu - Pros and Cons"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - linux
---




Comparison Table
----------------

Open Solaris

CentOs

Ubuntu

Version

10

5

904

Kernel

unix

linux

linux

commercial Version

Solaris

Red Hat

X

Availability

★★★★★

★★★★

★★★

LAMP package

★★

★★★

★★★★★

package manager

★★

★★★★

★★★★★

Security

★★★★★

★★★★

★★

Performance(x86 PC)

★★★★

★★★★★

★★★★★

Fault Tolerance

★★★★★

★★★★

★★

Popular(server)

★★★

★★★★★

★★

Community

★★

★★★★

★★★★★

Text Mode^[1)](#fn__1)^

No

Yes

Yes

Features

ZFS \
 D-Trace \
 Self-Healing

industry standard \
 Scalable Hardware Support \
 Cluster and Load Balancing Support

### Trend

![trend_linux.png][trend_linux.png]

Solaris (Open Solaris)
----------------------

### Pros

-   High Availability
-   Unix Base Kernel
-   [ZFS](http://en.wikipedia.org/wiki/ZFS "http://en.wikipedia.org/wiki/ZFS")
-   [D-Trace](http://en.wikipedia.org/wiki/Dtrace "http://en.wikipedia.org/wiki/Dtrace")

### Cons

-   Hard to use.
    -   There are not enough document to set functions up of solaris.

-   Difference usage style.
    -   The experience of Linux could not totally apply to Solaris.

-   Sun bought by Oracle, no body know how will Solaris going.

### Conclusion

If we need using Open Solaris as our server, we need more efforts on
this.

The unix base Solaris provides high Availability and more scalability
and the amazing
[ZFS](http://en.wikipedia.org/wiki/ZFS "http://en.wikipedia.org/wiki/ZFS")
and
[D-Trace](http://en.wikipedia.org/wiki/Dtrace "http://en.wikipedia.org/wiki/Dtrace").

if we don't have man who is familiar with it Solaris, it is hard to
manage. **More power more responsibility.**

RHEL (CentOS)
-------------

### Pros

-   industry standard for production servers
-   Stable Builds
-   Scalable Hardware Support
-   Cluster and Load Balancing Support
-   Longer Release Dates and Support
-   Enhanced Remote Management

### Cons

-   for security reason, RHEL disabled most function, we need more
    efforts to set it up if we need those.

### Conclusion

Red Hat Enterprise Linux (RHEL) is quickly becoming the industry
standard for production servers.The CentOS is a Community version of
RHEL.

Regarding our resource, if we don't have any professional support, **I
strong recommended to use RHEL as our production server**.

The RHEL is reliable enough and easy to manage.

Ubuntu
------

### Pros

-   Large Community

### Cons

-   most people using Ubuntu as a Desktop rather than as a Server.

### Conclusion

Not recommended using Ubuntu as a production server.



^[1)](#fnt__1)^ running without GUI
[trend_linux.png]: /images/wiki/linux/trend_linux.png