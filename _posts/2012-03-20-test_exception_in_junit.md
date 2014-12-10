---
author: Kent Chiu
published: true
layout: post
title: "Test Exception In JUnit"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
tags:
  - testing
  - java
  - junit
---



之前遇到測exception message都還改回用try catch，原來，可以用mock
like的方式去測exception message.


```
@Rule
public ExpectedException thrown = ExpectedException.none();
 
@Test
public void shouldNotLetUnknownUserLogin() throws UnknownUserException {
    thrown.expect(UnknownUserException.class);
    thrown.expectMessage("Unknown user: joe");
    UserDao dao = mock(UserDao.class);
    when(dao.findByName("joe")).thenReturn(null);
    manager.setUserDao(dao);
    manager.login("joe","secret");
}

```


```
thrown.expectMessage(JUnitMatchers.containsString("joe"));

```

code snippet is from
[here](http://weblogs.java.net/blog/johnsmart/archive/2009/09/27/testing-exceptions-junit-47 "http://weblogs.java.net/blog/johnsmart/archive/2009/09/27/testing-exceptions-junit-47")



