---
published: true
author: Kent Chiu
layout: post
title: "透過JMX重新載入Tomcat上的Web Application"
date: 2013-11-16 15:45
comments: true
sharing: true
footer: true
categories: 
- JMX
- spring
- tomcat
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



## tomcat 

## test by jconsole

啟動tomat

	kents-mbp:~ kent$ tail -f ~/dev/apache-tomcat-7.0.42/logs/catalina.out
	Nov 16, 2013 4:05:29 PM org.apache.catalina.startup.HostConfig deployWAR
	INFO: Deploying web application archive /Users/kent/dev/apache-tomcat-7.0.42/webapps/my-webapp.war

jconsole的位置在JDK的bin目錄下，執行後可以看到本機jvm運行中的程式。

一層一層展開下去可以看到tomcat有一個reload的節點，把節點的object name複製出來，程式會用到

`Catalina/WebModule/"//localhost/my-webapp"/non/e/none/Operations/reload`

jconsole可以直接執行reload的節點，執行後可以看到tomcat進行reload

	
	Nov 16, 2013 4:16:24 PM org.apache.catalina.core.StandardContext reload  # 透過JMX執行reload operation後可以看到tomcat對my-webapp進行reload
	INFO: Reloading Context with name [/my-webapp] has started
	Nov 16, 2013 4:16:25 PM org.apache.catalina.core.StandardContext reload
	INFO: Reloading Context with name [/my-webapp] is completed


也可以透過程式進行jmx呼叫，下面的程式是透過spring3執行jmx

    // bean的configration檔
    public MBeanServerConnection jmxConnector() {
        MBeanServerConnectionFactoryBean factoryBean =new MBeanServerConnectionFactoryBean();
        try {
            factoryBean.setServiceUrl("service:jmx:rmi://localhost/jndi/rmi://localhost:1099/jmxrmi");
            factoryBean.afterPropertiesSet();
            MBeanServerConnection connection = factoryBean.getObject();
            return connection;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }


    // 程式
    @Autowaire
    private MBeanServerConnection connection;
    
    public void reloadWebApplication() {
        try {
            ObjectName objectName = new ObjectName("Catalina:j2eeType=WebModule,name=//localhost/idcview,J2EEApplication=none,J2EEServer=none");
            connection.invoke(objectName, "reload", null, null); // 執行tomcat的reload
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
