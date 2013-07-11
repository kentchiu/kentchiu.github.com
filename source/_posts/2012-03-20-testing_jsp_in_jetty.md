---
author: Kent Chiu
published: true
layout: post
title: "Testing JSP in Jetty Server (Embedded Mode)"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - jsp
  - testing
  - java
---



```
package com.bellwin.idcview.login.page;
 
import static org.junit.Assert.assertTrue;
 
import org.eclipse.jetty.server.Connector;
import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.server.nio.SelectChannelConnector;
import org.eclipse.jetty.webapp.WebAppContext;
import org.junit.Before;
import org.junit.BeforeClass;
 
import com.gargoylesoftware.htmlunit.WebClient;
 
public abstract class AbstractContainerTest {
    protected static PauseableServer server;
 
    protected static final int port = 9180;
 
    protected static final String BASEURI = "http://localhost:" + port + "/";
 
    protected final WebClient webClient = new WebClient();
 
    @BeforeClass
    public static void startContainer() throws Exception {
        if (server == null) {
            server = new PauseableServer();
            Connector connector = new SelectChannelConnector();
            connector.setPort(port);
            server.setConnectors(new Connector[]{connector});
            server.setHandler(new WebAppContext("src/main/webapp", "/"));
            server.start();
            assertTrue(server.isStarted());
        }
    }
 
    @Before
    public void beforeTest() {
        webClient.setThrowExceptionOnFailingStatusCode(true);
    }
 
    public void pauseServer(boolean paused) {
        if (server != null) {
            server.pause(paused);
        }
    }
 
    public static class PauseableServer extends Server {
        public synchronized void pause(boolean paused) {
            try {
                if (paused) {
                    for (Connector connector : getConnectors()) {
                        connector.stop();
                    }
                } else {
                    for (Connector connector : getConnectors()) {
                        connector.start();
                    }
                }
            } catch (Exception e) {
            }
        }
    }
}
```

```
package com.bellwin.idcview.login.page;
 
import java.io.IOException;
import java.net.MalformedURLException;
 
import org.junit.Before;
import org.junit.Test;
 
import com.gargoylesoftware.htmlunit.ElementNotFoundException;
import com.gargoylesoftware.htmlunit.FailingHttpStatusCodeException;
import com.gargoylesoftware.htmlunit.WebAssert;
import com.gargoylesoftware.htmlunit.html.HtmlCheckBoxInput;
import com.gargoylesoftware.htmlunit.html.HtmlForm;
import com.gargoylesoftware.htmlunit.html.HtmlInput;
import com.gargoylesoftware.htmlunit.html.HtmlPage;
 
public class ContainerIntegrationTest extends AbstractContainerTest {
 
    @Before
    public void logOut() throws IOException {
        // Make sure we are logged out
        final HtmlPage homePage = webClient.getPage(BASEURI);
        try {
            homePage.getAnchorByHref("/logout.jsp").click();
        }
        catch (ElementNotFoundException e) {
            //Ignore
        }
    }
 
    @Test
    public void logIn() throws FailingHttpStatusCodeException, MalformedURLException, IOException, InterruptedException {
        HtmlPage page = webClient.getPage(BASEURI + "login.jsp");
        HtmlForm form = page.getFormByName("loginform");
        form.<HtmlInput>getInputByName("username").setValueAttribute("root");
        form.<HtmlInput>getInputByName("password").setValueAttribute("secret");
        page = form.<HtmlInput>getInputByName("submit").click();
        // This'll throw an expection if not logged in
        page.getAnchorByHref("/logout.jsp");
    }
 
    @Test
    public void logInAndRememberMe() throws Exception {
        HtmlPage page = webClient.getPage(BASEURI + "login.jsp");
        HtmlForm form = page.getFormByName("loginform");
        form.<HtmlInput>getInputByName("username").setValueAttribute("root");
        form.<HtmlInput>getInputByName("password").setValueAttribute("secret");
        HtmlCheckBoxInput checkbox = form.getInputByName("rememberMe");
        checkbox.setChecked(true);
        page = form.<HtmlInput>getInputByName("submit").click();
        server.stop();
        server.start();
        page = webClient.getPage(BASEURI);
        // page.getAnchorByHref("/logout.jsp");
        WebAssert.assertLinkPresentWithText(page, "Log out");
        page = page.getAnchorByHref("/account").click();
        // login page should be shown again - user remembered but not authenticated
        WebAssert.assertFormPresent(page, "loginform");
    }
 
}
```

You may want to logging message during Server starting.

```
<logger name="org.eclipse.jetty.util.log" additivity="false">
    <level value="debug" />
    <appender-ref ref="stdout" />
</logger>
```



