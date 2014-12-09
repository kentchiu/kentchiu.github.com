---
author: Kent Chiu
published: true
layout: post
title: "XMLHttpRequest (a.k.a AJAX)"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------


Create
======



```
    var xhr;
    if (window.XMLHttpRequest) {   
      xhr = new XMLHttpRequest();
    }
    else if (window.ActiveXObject) {   
      xhr = new ActiveXObject("Msxml2.XMLHTTP");
    }
    else {   
      throw new Error("Ajax is not supported by this browser"); 
    }

```

Method
------

  Method                                                             Description
  ------------------------------------------------------------------ ------------------------------------------------------------------------------------- -- --
  `abort()`                                                          Stops the current request                                                                
  `getAllResponseHeaders()`                                          Returns complete set of headers (labels and values) as a string                          
  `getResponseHeader(”headerLabel”)`                                 Returns the string value of a single header label                                        
  `open(”method”, ”URL”[, asyncFlag[, ”userName”[, ”password”]]])`   Assigns destination URL, method, and other optional attributes of a pending request      
  `send(content)`                                                    Transmits the request, optionally with postable string or DOM object data                
  `setRequestHeader(”label”, ”value”)`                               Assigns a label/value pair to the header to be sent with a request                       

Properties
==========

  -----------------------------------------------------------------------------------------------------------
  Property               Description
  ---------------------- ------------------------------------------------------------------------------ -- --
  `onreadystatechange`   Event handler for an event that fires at every state change                       

  `readyState`           Object status integer:\                                                           
                          0 = uninitialized\                                                               
                          1 = loading\                                                                     
                          2 = loaded\                                                                      
                          3 = interactive\                                                                 
                          4 = complete                                                                     

  `responseText`         String version of data returned from server process                               

  `responseXML`          DOM-compatible document object of data returned from server process               

  `status`               Numeric code returned by server, such as 404 for “Not Found” or 200 for “OK”      

  `statusText`           String message accompanying the status code                                       
  -----------------------------------------------------------------------------------------------------------

Status code
===========

  code   description
  ------ -------------------------------
  100    Continue
  101    Switching protocols
  200    OK
  201    Created
  202    Accepted
  203    Non-Authoritative Information
  204    No Content
  205    Reset Content
  206    Partial Content
  300    Multiple Choices
  301    Moved Permanently
  302    Found
  303    See Other
  304    Not Modified
  305    Use Proxy
  307    Temporary Redirect
  400    Bad Request
  401    Unauthorized
  402    Payment Required
  403    Forbidden
  404    Not Found
  405    Method Not Allowed
  406    Not Acceptable
  407    Proxy Authentication Required
  408    Request Timeout
  409    Conflict
  410    Gone
  411    Length Required
  412    Precondition Failed
  413    Request Entity Too Large
  414    Request-URI Too Long
  415    Unsupported Media Type
  416    Requested Range Not Suitable
  417    Expectation Failed
  500    Internal Server Error
  501    Not Implemented
  502    Bad Gateway
  503    Service Unavailable
  504    Gateway Timeout
  505    HTTP Version Not Supported

[more
details...](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10 "http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10")