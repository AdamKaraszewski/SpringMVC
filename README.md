# SpringMVC - xml configuration
Spring MVC projects for educational purposes.

This project presents a simple web application, which is build using spring MVC and xml configuration.

```text
src
 |_main
    |_java
    |  |_com
    |     |_example
    |        |_controllers
    |           |_HelloController.java 
    |_resources 
    |_webapp
       |_WEB-INF
          |_web.xml
          |_DispatcherServlet-servlet.xml
          |_jsp
             |_welcome.jsp
```

## pom.xml file configuration

### 1. packaging as war
Our project will be packaged as a war file. The following line is required in pom.xml. It tells maven that we want war
archive as a result of build.

```xml
<packaging>war</packaging>
```

### 2. spring-mvc dependency
Our project is based on spring-mvc library. This library provides a number of helpful utilities for mvc app.
To add this library, we need to use following dependency in our code:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>7.1.0-M2</version>
</dependency>
```

### 3. jakarta.servlet dependency
In the project we use Dispatcher Servlet, which is responsible for handling HTTP request. To register dispatcher servlet
we need following dependency:

```xml
<dependency>
    <groupId>jakarta.servlet</groupId>
    <artifactId>jakarta.servlet-api</artifactId>
    <version>6.1.0</version>
    <scope>provided</scope>
</dependency>
```

This dependency is marked as provided, because servlet container (tomcat 11 provides jakarta.servlet 6.1 - from tomcat 
documentation).

## web.xml file configuration

### 1. registering Dispatcher Servlet and binding with '/' path
Dispatcher Servlet is a front controller, which is responsible for handling all incoming HTTP requests. We need to bind 
Dispatcher Servlet with path /. It means that every HTTP requests will be processed by Dispatcher Servlet

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

## DispatcherServlet-servlet.xml configuration

### 1. enabling spring-webmvc utilities 

Firstly, we need to 'unlock' spring-webmvc utilities (e.g. annotations)

```xml
<mvc:annotation-driven />
```

### 2. component scan (looking for @Controller classes)

In our application I will use spring beans annotated with @Component. Component beans are responsible for processing
HTTP requests. Controllers get request from Dispatcher Servlet. To instantiate these controllers I need to show Spring
the package with my controllers definitions.

```xml
<context:component-scan base-package="com.example.controllers"/>
```

### 3. InternalResourceViewResolver bean creation

InternalResourceViewResolver is responsible for translate logical view/template name to it is physical instance. It lets
us using easier and more logical template names in code.

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

### 4. HelloController.java configuration

HelloController is a simple controller bean. This class is annotated with @Controller - this mean spring register it as
a Spring Bean and is responsible for its lifecycle. 

Annotation @RequestMapping("/") says that every HTTP request with localhost:8080/SpringMVC/ is processed by this controller

The last important feature is `return "welcome"`. This return statement says that method should send welcome template.
InternalResourceViewResolverBean is responsible for map this name into correct template (welcome.jsp).


## Tomcat (servlet container) configuration and starting developer instance

I will run this project on my local machine. This machine has tomcat 11 instance. 
In my developer environment I have two tomcat folders:
- unziped tomcat from official website (v11.0.24) (CATALINA_HOME)
- CATALINA BASE with startup.bat and shutdown.bat (server.xml `Connector port="8080"`)

```
$CATALINA_BASE\bin> .\startup.bat
```

It sets CATLINA_BASE environment variable and runs CATALINA_BASE\bin\startup.bat

## Build and deploy project on developer environment

To build project it required to run this command:

```
mvn clean install
```

After successful build new folder 'target' should appear. This folder contains war archive: SpringMVC.war

To deploy app you should copy SpringMVC.war into $CATLINA_BASE\webapps folder. Tomcat should automatically detect
deployed app.

In browser type:

```text
http://localhost:8080/SpringMVC/
```


