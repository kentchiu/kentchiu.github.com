---
published: true
author: Kent Chiu
layout: post
title: "Spring hateoas"
date: 2015-02-02 09:11
tags: 
- restful
- spring
- hateoas
---


``` java
    @Entity
    public class Person {
        // ... person's properties
    }

    @Entity
    public class Address {

        @OneToOne(optional = false)
        private Person person;

    }
```

如果是用json格式新增address，person因為是關聯關係，所以要直接用href的url當person的值

``` json
  {
        "postalCode": "12345",
        "province": "MO",
        "lines": ["1 W 1st St."],
        "city": "Univille",
        "person": "http://localhost:8080/people/1"
    }
```

另一種方式，是用 `text/uri-list`，可直接傳入 href的url，而不是用json的格式
```
curl -X POST -v -d 'http://localhost:8080/people/1' -H "Content-Type: text/uri-list" http://localhost:8080/family/1/members
```


# Resources
- <https://github.com/spring-projects/spring-hateoas> - Spring hateoas 官網
- <https://github.com/spring-projects/spring-data-rest/wiki> - spring data rest wiki
- <http://www.ibm.com/developerworks/cn/java/j-lo-SpringHATEOAS/> - 使用 Spring HATEOAS 开发 REST 服务


