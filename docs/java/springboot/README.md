# Spring Boot

[文章来源JavaGuide](https://github.com/Snailclimb/springboot-guide)

> Spring Boot 是基于Spring框架，对Spring框架一些烦人的配置做了封装，开发人员不用再去面对这些繁杂的配置

**Spring Boot项目**

以 Application为后缀名的 Java 类一般就是 Spring Boot 的启动类，比如本项目的启动项目就是HelloWorldApplication 。我们直接像运行普通 Java 程序一样运行它，由于 Spring Boot 本身就嵌入servlet容器的缘故，我们的 web 项目就运行成功了， 非常方便。

需要注意的一点是 Spring Boot 的启动类是需要最外层的，不然可能导致一些类无法被正确扫描到，导致一些奇怪的问题。 一般情况下 Spring Boot 项目结构类似下面这样

使用idea创建Spring Boot项目的目录分级：
```
com
  +- example
    +- myproject
      +- Application.java
      |
      +- domain
      |  +- Customer.java
      |  +- CustomerRepository.java
      |
      +- Service
      |  +- CustomerService.java
      |
      +- controller
      |  +- CustomerController.java
      |
      +- config
      |  +- swagerConfig.java
```
1. Application.java是项目的启动类
2. domain目录主要用于实体（Entity）与数据访问层（Repository）
3. service 层主要是业务类代码
4. controller 负责页面访问控制
5. config 目录主要放一些配置类

**@SpringBootApplication 注解分析**

HelloWorld项目启动类
```java
@SpringBootApplication
public class HelloWorldApplication {

	public static void main(String[] args) {
		SpringApplication.run(HelloWorldApplication.class, args);
	}

}
```

@SpringBootApplication注解源码
```java
package org.springframework.boot.autoconfigure;
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
   ......
}


package org.springframework.boot;
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
```

可以看出大概可以把 @SpringBootApplication 看作是 @Configuration、@EnableAutoConfiguration、@ComponentScan 注解的集合。根据 SpringBoot官网，这三个注解的作用分别是：

1. @EnableAutoConfiguration：启用 SpringBoot 的自动配置机制
2. @ComponentScan： 扫描被@Component (@Service,@Controller)注解的bean，注解默认会扫描该类所在的包下所有的类。
3. @Configuration：允许在上下文中注册额外的bean或导入其他配置类。
所以说 @SpringBootApplication 就是几个重要的注解的组合，为什么要有它？当然是为了省事，避免了我们每次开发 Spring Boot 项目都要写一些必备的注解。这一点在我们平时开发中也经常用到，比如我们通常会提一个测试基类，这个基类包含了我们写测试所需要的一些基本的注解和一些依赖。

新建一个 controller 文件夹，并在这个文件夹下新建一个名字叫做 HelloWorldController 的类。

> @RestController是Spring 4 之后新加的注解，如果在Spring4之前开发 RESTful Web服务的话，你需要使用@Controller 并结合@ResponseBody注解，也就是说@Controller +@ResponseBody= @RestController

```java
package com.example.helloworld.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("test")
public class HelloWorldController {
    @GetMapping("hello")
    public String sayHello() {
        return "Hello World";
    }
}
```

默认情况下，Spring Boot 项目会使用 8080 作为项目的端口号。如果我们修改端口号的话，非常简单，直接修改

application.properties配置文件即可。

src/main/resources/application.properties
```java
server.port=8333
```

## RESTful Web 服务开发

假如我们有一个书架，上面放了很多书。为此，我们需要新建一个 Book 实体类。   
`com.example.helloworld.entity`   
```java
/**
 * @author shuang.kou
 */
@Data
public class Book {
    private String name;
    private String description;
}
```
我们还需要一个控制器对书架上进行添加、查找以及查看。为此，我们需要新建一个 BookController 。   
```java
import com.example.helloworld.entity.Book;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

@RestController
@RequestMapping("/api")
public class BookController {

    private List<Book> books = new ArrayList<>();

    @PostMapping("/book")
    public ResponseEntity<List<Book>> addBook(@RequestBody Book book) {
        books.add(book);
        return ResponseEntity.ok(books);
    }

    @DeleteMapping("/book/{id}")
    public ResponseEntity deleteBookById(@PathVariable("id") int id) {
        books.remove(id);
        return ResponseEntity.ok(books);
    }

    @GetMapping("/book")
    public ResponseEntity getBookByName(@RequestParam("name") String name) {
        List<Book> results = books.stream().filter(book -> book.getName().equals(name)).collect(Collectors.toList());
        return ResponseEntity.ok(results);
    }
}
```

1. @RestController **将返回的对象数据直接以 JSON 或 XML 形式写入 HTTP 响应(Response)中。**绝大部分情况下都是直接以 JSON 形式返回给客户端，很少的情况下才会以 XML 形式返回。转换成 XML 形式还需要额为的工作，上面代码中演示的直接就是将对象数据直接以 JSON 形式写入 HTTP 响应(Response)中。
2. @RequestMapping :上面的示例中没有指定 GET 与 PUT、POST 等，因为**@RequestMapping默认映射所有HTTP Action**，你可以使用@RequestMapping(method=ActionType)来缩小这个映射。
3. @PostMapping实际上就等价于 @RequestMapping(method = RequestMethod.POST)，同样的 @DeleteMapping ,@GetMapping也都一样，常用的 HTTP Action 都有一个这种形式的注解所对应。
4. @PathVariable :取url地址中的参数。@RequestParam url的查询参数值。
5. @RequestBody:可以将 HttpRequest body 中的 JSON 类型数据反序列化为合适的 Java 类型。
6. ResponseEntity: 表示整个HTTP Response：状态码，标头和正文内容。我们可以使用它来自定义HTTP Response 的内容。