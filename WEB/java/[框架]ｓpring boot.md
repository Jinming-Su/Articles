# 包结构
* controller
* entity
* repository
* service

# 配置文件pom.xml
### parent
spring-boot-starter-parent是一个parent pom，目的是更加容易的管理版本依赖和使用默认配置，通常在spring boot项目中会先继承一个parent，通常会继承spring-boot-starter-parent  
spring-boot-starter-parent会为依赖指定最合适的版本进行添加，所以就不需要为依赖指定版本号
### starter
starter是可以包含到应用中的一个方便的依赖关系描述符集合，命名模式为spring-boot-starter-*

### spring-boot-starter-web
对全栈web开发的支持， 包括Tomcat和spring-webmvc等依赖包

# 注解大全
这里需要一部分Spring的知识
* `@SpringBootApplication`指定本程序是一个spring boot应用程序
* `@Service`用于标注业务层组件（注入Dao），`@Controller`用于标注控制层组件（注入Service），`@Repository`用于标注数据访问层组件，即DAO，`@Component`泛指组件，当不好归类时使用这个注解进行标注
* `@RestController`等价于`@Controller+@RequestBody`
* `@GetMapping`，将http请求映射到指定的方法，类似的还有Post/Put/Delete/RequestMapping
* `@Value("${xxx}")` 可以简单地把配置文件中的属性值注入
* `@ConfigurationProperties(prefix="xxx"` 将配置文件中的属性注入到一个配置对象中
* `@Valid`可以校验实体类属性

# spring boot 返回json
spring boot 默认使用的json解析框架是jackson，直接将返回的对象解析为json返回

# 热部署
使用spring loader可以进行热部署，但是只能更新已有的方法，不能新增方法  
spring-boot-devtools实现热部署，原理是使用两个类分别加载未改变的类和发生改变的类，当有代码更改的时候，每次只加载发生更新的类  
devtools的用法:  
```
//先添加配置
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
	<scope>true</scope>
</dependency>

<plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
		<fork>true</fork>
	</configuration>
</plugin>
```  
如果是在eclipse中，上述配置即可实现热部署，但是IDEA下默认不能在RUN下自动编译，因此需要额外设置一下，这完全是IDEA的问题，具体参考http://blog.csdn.net/wjc475869/article/details/52442484.  

# JPA
JPA即Java Persistence API.JPA通过JDK5.0注解或者XML描述对象-关系表的映射关系，并将运行期的实体对象持久化到数据库中  
Spring Data是一个开源框架，而Spring Data JPA是框架中的一个模块.如果单独使用JPA进行开发，代码会和JDBC开发一样烦人，而Spring Data JPA就是为了简化JPA的写法.  
### 配置:  
```
// 先添加配置pom.xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

// application.yml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/dbgirl
    username: root
    password:
  jpa:
    database: MYSQL
    hibernate:
      ddl-auto: update
      naming:
        strategy: org.hibernate.cfg.ImprovedNamingStrategy
```
### 使用
* 创建实体类:使用`@Entity`进行实体类的持久化操作，当JPA检测到我们的实体类当中有`@Entity`注解时，会在数据库中生成对应的表结构信息，使用`@Id`指定主键，使用`@GeneratedValue(strategy = GenerationType.AUTO)`指定主键的生成策略  
* `@Service`进行Service的注解，`@Resource`和`@Autowired`都可以进行注入Bean，`@Autowired`是spring提供的方法，`@Resource`是java提供的方法
* `@Transactional`进行事务的绑定，一般在`@Service`中使用
* CrudRepository接口继承自Repository接口，PagingAndSortingRepository接口继承CrudRepository，JpaRepository接口继承自PagingAndSortingRepository

# 全局异常捕获
1. 新建一个Class用于处理异常
2. 在此class上添加注解，`@ControllerAdvice`
3. 在class中添加方法，方法上添加注解`@ExceptionHandler(Exception.class)`，来拦截相应的异常信息
4. 如果返回View，方法的返回值为ModelAndView；如果返回String或者Json，需要为方法添加`@ResponseBody`注解

# thymeleaf模板
1. 添加依赖  
```
// pom.xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
2. 关闭thymeleaf缓存
```
// application.yml
spring.thymeleaf.cache: false
```
3. 在resources/templates下建立.html
4. 在controller中指定.html即可

# 表单验证
http://blog.csdn.net/a60782885/article/details/68488411 

# AOP
1. 导入依赖
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
2. 创建类
使用`@Aspect``@Component`注解类
3. 具体用法参考http://www.cnblogs.com/lic309/p/4079194.html

# 日志
```
protected static Logger logger=LoggerFactory.getLogger(HelloController.class);  
logger.info('xxx');
```

# 单元测试
主要测试Service和Controller.
1. 添加依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```
2. 在测试类头添加注解`@RunWith(SpringRunner.class)`和`@SpringBootTest`
3. 在测试的方法上添加`@Test`注解  
4. 使用MockMvc测试API
