# spring boot 记录

@RequestMapping注解提供了“路由”信息。它告诉Spring任何带有路径“/”的HTTP请求应该映射到home方法。
@RestController注解告诉Spring将生成的字符串直接返回给调用者。


用 @Component 对那些比较中立的类进行注释
1、@controller 控制器（注入服务）
2、@service 服务（注入dao）
3、@repository dao（实现dao访问）
4、@component （把普通pojo实例化到spring容器中，相当于配置文件中的）
@Component,@Service,@Controller,@Repository注解的类，并把这些类纳入进spring容器中管理。

@Autowired与@Resource都可以用来装配bean.


@RequestMapping:
RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中 
的所有响应请求的方法都是以该地址作为父路径。 
该注解有六个属性: 
params:指定request中必须包含某些参数值是，才让该方法处理。 
headers:指定request中必须包含某些指定的header值，才能让该方法处理请求。 
value:指定请求的实际地址，指定的地址可以是URI Template 模式 
method:指定请求的method类型， GET、POST、PUT、DELETE等 
consumes:指定处理请求的提交内容类型（Content-Type），如application/json,text/html; 
produces:指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回


（2）@ResponseBody
@Responsebody表示该方法的返回结果直接写入HTTP response body中
一般在异步获取数据时使用，在使用@RequestMapping后，返回值通常解析为跳转路径，
加上@Responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。
比如异步获取json数据，加上@Responsebody后，会直接返回json数据。
 
（3）@RequestBody将HTTP请求正文插入方法中,使用适合的HttpMessageConverter将请求体写入某个对象。

@GetMapping、@PostMapping等:
相当于@RequestMapping（value=”/”，method=RequestMethod.Get\Post\Put\Delete等） 。是个组合注解


@RequestParam:
用在方法的参数前面。相当于 request.getParameter()。

@PathVariable:
路径变量。如 RequestMapping(“user/get/mac/{macAddress}”) 
public String getByMacAddress(@PathVariable(“macAddress”) String macAddress){ 
//do something; 
} 
参数与大括号里的名字相同的话，注解后括号里的内容可以不填


@SpringBootApplication
由于大量项目都会在主要的配置类上添加@Configuration,@EnableAutoConfiguration,@ComponentScan三个注解。
因此Spring Boot提供了@SpringBootApplication注解，该注解可以替代上面三个注解（使用Spring注解继承实现）。


***


# Spring Boot 集成 MyBatis, 分页插件 PageHelper, 通用 Mapper 

CSDN博客：http://blog.csdn.net/isea533/article/details/73555400
GitHub项目：https://github.com/mybatis-book/book

在 `src/main/resources` 中创建 META-INF 目录，在此目录下添加 spring-devtools.properties 配置，内容如下：
```properties
restart.include.mapper=/mapper-[\\w-\\.]+jar
restart.include.pagehelper=/pagehelper-[\\w-\\.]+jar
```
使用这个配置后，就会使用 restart 类加载加载 include 进去的 jar 包。

## 集成 MyBatis Generator
通过 Maven 插件集成的，所以运行插件使用下面的命令：
>mvn mybatis-generator:generate

Mybatis Geneator 详解:
>http://blog.csdn.net/isea533/article/details/42102297

## application.properties 配置
```properties
#mybatis
mybatis.type-aliases-package=tk.mybatis.springboot.model
mybatis.mapper-locations=classpath:mapper/*.xml

#mapper
#mappers 多个接口时逗号隔开
mapper.mappers=tk.mybatis.springboot.util.MyMapper
mapper.not-empty=false
mapper.identity=MYSQL

#pagehelper
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

## application.yml 配置

完整配置可以参考 [src/main/resources/application-old.yml](https://github.com/abel533/MyBatis-Spring-Boot/blob/master/src/main/resources/application-old.yml) ，和 MyBatis 相关的部分配置如下：

```yaml
mybatis:
    type-aliases-package: tk.mybatis.springboot.model
    mapper-locations: classpath:mapper/*.xml

mapper:
    mappers:
        - tk.mybatis.springboot.util.MyMapper
    not-empty: false
    identity: MYSQL

pagehelper:
    helperDialect: mysql
    reasonable: true
    supportMethodsArguments: true
    params: count=countSql
```

注意 mapper 配置，因为参数名固定，所以接收参数使用的对象，按照 Spring Boot 配置规则，大写字母都变了带横线的小写字母。针对如 IDENTITY（对应i-d-e-n-t-i-t-y）提供了全小写的 identity 配置，如果 IDE 能自动提示，看自动提示即可。

## SSM集成的基础项目 
>https://github.com/abel533/Mybatis-Spring

## MyBatis工具 http://www.mybatis.tk

- 推荐使用 Mybatis 通用 Mapper3 https://github.com/abel533/Mapper

- 推荐使用 Mybatis 分页插件 PageHelper https://github.com/pagehelper/Mybatis-PageHelper

## 作者信息

- 作者博客：http://blog.csdn.net/isea533

- 作者邮箱：abel533@gmail.com

- 如需加群，请通过 http://mybatis.tk 首页按钮加群。





































