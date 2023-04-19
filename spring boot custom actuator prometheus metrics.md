## Add custom actuator prometheus metric in spring boot

This article describes a elegant way to add custom actuator prometheus metric to our spring boot article. Spring boot, a powerful java based framework to develop micro services web application provides many easy to configure solutions for developers need. Developing a microservices application is complete only when we can monitor these microservices. Spring boot provides actuator to help monitor these microservices. Actuator uses HTTP endpoints to provide information about the application. Actuator provides lot of metrics like the database connections , thread information, memory information, HTTP client and server requests etc. 

In order to add actuator to your spring boot application add the following to your pom.xml 


For Maven
```java
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-actuator</artifactId>
	</dependency>
</dependencies>

```

For Gradle
```java
dependencies {
	compile("org.springframework.boot:spring-boot-starter-actuator")
}
```

Run your spring application using 

```bash
mvn spring-boot:run
```

The application will run on port 8080. Go to http://localhost:8080/actuator to view the actuator end points. 


![Actuator End Point](https://user-images.githubusercontent.com/7569031/233104414-32731ea3-a848-4b41-bf13-671fd3a73931.png)

The actuator provides a health check endpoint which shows the status of the application. The status is "UP" if the application is healthy and "DOWN" if there are any issues

![Health EndPoint](https://user-images.githubusercontent.com/7569031/233108139-1bd8bb24-e095-4a07-a5e5-0ba972e79eea.png)

Actuator does provide more than just the health information. To unlock its true potential we need to expose its endpoint to the outside world. To do this include the following in your application.properties

```file
management.endpoint.info.enabled=true
management.endpoints.web.exposure.include=*
```

Now go to http://localhost:8080/actuator to see more actuator end points exposed like below

```json
{"_links":{"self":{"href":"http://localhost:8080/actuator","templated":false},
"beans":{"href":"http://localhost:8080/actuator/beans","templated":false},
"caches-cache":{"href":"http://localhost:8080/actuator/caches/{cache}","templated":true},
"caches":{"href":"http://localhost:8080/actuator/caches","templated":false},
"health-path":{"href":"http://localhost:8080/actuator/health/{*path}","templated":true},
"health":{"href":"http://localhost:8080/actuator/health","templated":false},
"info":{"href":"http://localhost:8080/actuator/info","templated":false},
"conditions":{"href":"http://localhost:8080/actuator/conditions","templated":false},
"configprops":{"href":"http://localhost:8080/actuator/configprops","templated":false},
"configprops-prefix":{"href":"http://localhost:8080/actuator/configprops/{prefix}","templated":true},
"env":{"href":"http://localhost:8080/actuator/env","templated":false},
"env-toMatch":{"href":"http://localhost:8080/actuator/env/{toMatch}","templated":true},
"loggers":{"href":"http://localhost:8080/actuator/loggers","templated":false},
"loggers-name":{"href":"http://localhost:8080/actuator/loggers/{name}","templated":true},
"heapdump":{"href":"http://localhost:8080/actuator/heapdump","templated":false},
"threaddump":{"href":"http://localhost:8080/actuator/threaddump","templated":false},
"metrics":{"href":"http://localhost:8080/actuator/metrics","templated":false},
"metrics-requiredMetricName":{"href":"http://localhost:8080/actuator/metrics/{requiredMetricName}","templated":true},
"scheduledtasks":{"href":"http://localhost:8080/actuator/scheduledtasks","templated":false},
"mappings":{"href":"http://localhost:8080/actuator/mappings","templated":false}}}
```

As you can see the actuator provides information about beans, env, heapdump, threaddump, metrics etc. In order to get any detailed information you can click on the link. In case of threaddump click: http://localhost:8080/actuator/threaddump

![Thread Dump Info](https://user-images.githubusercontent.com/7569031/233135743-6ba4d395-574b-49a1-880e-f1a52a5d896e.png)










