---
author: Kent Chiu
published: true
layout: post
title: "Show Log In Eclipse Console"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - pdt
---




Using eclipse **External Tools configuration** to run a tail program to
keep monitor log files.

![show_log_in_eclipse_console_01.png][show_log_in_eclipse_console_01.png]

Please ensure you have should tail program

-   Location: point to your tail program
-   Working Directory: Using **Browse Workspace** to select your project
    node.
-   Arguments: -f \<log file name\>


[show_log_in_eclipse_console_01.png]: /images/wiki/php/show_log_in_eclipse_console_01.png