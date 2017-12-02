title: 使用Spring-MVC开发Web应用
date: 2016-05-18 19:09:22
tags:
- Spring-MVC
categories:
- Tech
- Web
- Spring
---

# 使用Spring-MVC开发Web应用

### 开发环境

- Intellij 14
- Linux Ubuntu

---

### WebApp搭建步骤

#### 1. 新建项目

选择 [create from archetype] 并点击maven-archetype-webapp
![](/img/spring-mvc-tutorial/1.png)


命名项目的GroupId和ArtifactId
![](/img/spring-mvc-tutorial/2.png)


确认maven项目的属性和maven配置
![](/img/spring-mvc-tutorial/3.png)

命名项目名称和模块名
![](/img/spring-mvc-tutorial/4.png)

#### 2. 配置依赖

配置pom.xml，添加属性和依赖，其中还有tomcat的插件用于部署和运行。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.whatever</groupId>
  <artifactId>demo</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
    <name>CounterWebApp Maven Webapp</name>
  <url>http://maven.apache.org</url>

    <properties>
        <jdk.version>1.7</jdk.version>
        <spring.version>4.1.1.RELEASE</spring.version>
        <jstl.version>1.2</jstl.version>
        <junit.version>4.11</junit.version>
        <logback.version>1.1.3</logback.version>
        <jcl-over-slf4j.version>1.7.13</jcl-over-slf4j.version>
    </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
          <version>${spring.version}</version>
          <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
          </exclusions>
      </dependency>

      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>jcl-over-slf4j</artifactId>
          <version>${jcl-over-slf4j.version}</version>
      </dependency>

      <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
          <version>${logback.version}</version>
      </dependency>

      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>${spring.version}</version>
      </dependency>

      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>${spring.version}</version>
      </dependency>

      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>
  </dependencies>
  <build>
    <finalName>demo</finalName>

      <plugins>

          <!-- Set JDK Compiler Level -->
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.2</version>
              <configuration>
                  <source>${jdk.version}</source>
                  <target>${jdk.version}</target>
              </configuration>
          </plugin>

          <!-- For Maven Tomcat Plugin -->
          <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <version>2.2</version>
              <configuration>
                  <!--Deploy to server-->
                  <url>http://localhost:9000/manager/text</url>
                  <port>9000</port>
                  <server>Tomcat7Server</server>
                  <path>/CounterWebApp</path>
              </configuration>
          </plugin>
      </plugins>

  </build>
</project>

```

当然我们还要在settings处设置自动导入maven项目，才可以在本机中download需要的maven依赖
![](/img/spring-mvc-tutorial/5.png)


在项目视图中添加java目录，位于main文件夹中，并在Project Structure中设置java为源文件
![](/img/spring-mvc-tutorial/6.png)


在java文件夹中新建包的GroupId目录，并添加BaseController的java文件
![](/img/spring-mvc-tutorial/7.png)

#### 3. 实现控制器

实现BaseController.java
```java
package com.whatever.Controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Created by mino on 16-5-18.
 */
/*There must be a Controller annotation or the application will doesn't work .*/
@Controller
public class BaseController {
    private static int counter=0;
    private static final String VIEW_INDEX="index";
    private static final Logger logger= LoggerFactory.getLogger(BaseController.class);

    @RequestMapping(value = "/" ,method = RequestMethod.GET)
    public String welcome(ModelMap model){
        model.addAttribute("message","Welcome");
        model.addAttribute("counter",++counter);
        logger.debug("[Welcome counter :{}",counter);
        return VIEW_INDEX;//返回index.jsp
    }

    @RequestMapping(value = "/{name}" ,method = RequestMethod.GET)
    public String welcome(@PathVariable String name , ModelMap model){
        model.addAttribute("message","Welcome "+name);
        model.addAttribute("counter",++counter);
        logger.debug("[Welcome counter :{}",counter);
        return VIEW_INDEX;//返回index.jsp
    }
}
```

#### 4. 实现事件分发器

在webapp/WEB-INF/目录下的web.xml文件中添加mvc事件分发器
```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         version="2.5">

    <display-name>Counter  Web  Application </display-name>

    <servlet>
        <servlet-name>mvc-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>mvc-dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```


在webapp/WEB-INF/目录中添加mvc-dispatcher-servlet.xml分发器配置文件，将请求匹配到的模式分发到对应的前缀后缀文件中
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.whatever.Controller"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/WEB-INF/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
</beans>
```


当然我们的jsp文件也要简单的实现一下
```html
<html>
<body>
<h1>Maven + Spring MVC Web Project Example</h1>
<h2>Hello World!</h2>
<h3>Message:${message}</h3>
<h3>Counter:${counter}</h3>
</body>
</html>
```


最后再在resources文件夹中添加logback.xml用于记录事件即可
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">

            <Pattern>
                %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
            </Pattern>

        </layout>
    </appender>

    <logger name="com.whatever.Controller" level="debug"
            additivity="false">
        <appender-ref ref="STDOUT" />
    </logger>

    <root level="error">
        <appender-ref ref="STDOUT" />
    </root>

</configuration>
```

#### 5. 运行和发布

打开View -> Tool Windows -> Maven Projects 找到tomcat插件，点击run即可运行，点击deploy即可部署


在浏览器上输入
localhost:9000/CounterWebApp/
![](/img/spring-mvc-tutorial/8.png)

localhost:9000/CounterWebApp/user1
![](/img/spring-mvc-tutorial/9.png)

























