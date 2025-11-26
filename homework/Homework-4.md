# ECS160-HW4 (Extra credit - 5 points)

## Due date: 12/12/2025 (No late days permitted)
## Problem: 
Reimplement HW2 using Spring Boot framework. 

_Learning objectives:_
1. Spring Boot microservices.
  
_Problem statement:_

The goal of this HW is to "reimplement" HW2 using state of the art microservice framework instead of the one we developed in Part B of HW2. 

### Part A

We will use [Spring Boot](https://spring.io/projects/spring-boot) as our microservices framework. Please clone the repository [here](https://github.com/davsec-teaching/spring-boot-handout). This repository comes with
a `pom.xml` file that already contains the library dependencies for Spring Boot. (If you're interested in setting this up yourself, check out https://start.spring.io/). The provided repository contains a `ModerationService.java` source code file that you can use to get started. 

Spring Boot uses Java Annotations to annotate the services. Check out this
[tutorial](https://codecrunch.org/creating-a-post-and-get-request-springboot-ff6e82a5d46b) for how to use Spring Boot 
with HTTP GET (and POST) requests. 
We will create REST API endpoints for these microservices. For more information on REST API check out [this](https://www.redhat.com/en/topics/api/what-is-a-rest-api) link. 
REST APIs can support any HTTP methods including `GET`, `POST`, `PUT`, `DELETE` and so on. In our case we will use the HTTP `GET` method to send the request to the microservice.

A Spring Boot microservice should at least consist of a `SpringBootApplication` and a `Controller`. The `SpringBootApplication` is a Java class that is annotated with `@SpringBootApplication.` 
This class will implement the `public static void main(..)` function as shown below. 

Sample code for a Spring Boot service application is as follows. The class must be annotated with `@SpringBootApplication`. 
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyMicroService {

	public static void main(String[] args) {
		SpringApplication.run(MyMicroService.class, args);
	}

}
```

The Controller specifies the REST endpoint (the url the service is bound to). Sample code for a Controller is as follows. The controller class must be annotated with the `RestController` annotation. We will be using the `HTTP Post` method to send the request to the microservice, so we annotate the function that implements the REST endpoint
as `@GetMapping("/<endpoint_url>")`. See: https://howtodoinjava.com/spring-mvc/controller-getmapping-postmapping/ for more examples.

Edit the `application.properties` file to add `server.port=30000` for the microservice. 

Build the application with `mvn clean install`.

Run the application with `mvn spring-boot:run`. 

On a successful launch, you should see log messages similar to the following on the terminal.

````
2024-12-11T20:51:18.604-08:00  INFO 20432 --- [HW3] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 30000 (http)
2024-12-11T20:51:18.625-08:00  INFO 20432 --- [HW3] [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2024-12-11T20:51:18.626-08:00  INFO 20432 --- [HW3] [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.33]
2024-12-11T20:51:18.751-08:00  INFO 20432 --- [HW3] [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2024-12-11T20:51:18.757-08:00  INFO 20432 --- [HW3] [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1175 ms
2024-12-11T20:51:19.188-08:00  INFO 20432 --- [HW3] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 30000 (http) with context path '/'
2024-12-11T20:51:19.194-08:00  INFO 20432 --- [HW3] [           main] com.ecs160.HW3.ModerationService         : Started ModerationService in 2.082 seconds (process running for 2.372)
````

### Part B

1. Make any adjustments to HW2 Part C necessary to invoke the Spring Boot microservices.
2. Run HW2 Part D. You can use HW2 Part A to load from Redis.
3. Report the results in `ANALYSIS.md`.

### Submission instructions
1. Please follow the same submission instructions as HW2.
