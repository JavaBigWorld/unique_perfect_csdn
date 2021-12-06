# Swagger
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210710164336218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
相信无论是前端还是后端开发，都或多或少地被接口文档折磨过。
前端经常抱怨后端给的接口文档和实际情况不一致。
后端又觉得编写及维护接口文档会耗费不少精力，
经常来不及更新。其实无论是前端调用后端，
还是后端调用前端，都期望有一个好的接口文档。
但是这个接口文档对于程序员来说，就跟注释一样，
经常还会抱怨别人写的代码没有写注释，然而自己写
起代码来，最讨厌的也是写注释。所以仅仅只听过强制
了来规范大家是不够的，随着时间推移，版本迭代，
接口文档往往很容易就跟不上代码了。
```


## 1 什么是 Swagger
```markdown
发现了痛点就要去找解决方案。解决方案用的人多了，
就成了标准的规范，这就是 Swagger 的由来。通过
这套规范，你只需要按照它的规范去定义接口及接口
相关信息。再通过 Swagger 衍生出来的一系列项目
和工具，就可以做到生成各种格式的接口文档，生
成多做语言的客户端和服务端的代码，以及在线接口
调试页面等等。这样，如果按照新的开发方式，在开
发新版本或者迭代版本的时候，只需要更新 Swagger 
描述文件，就可以自动生成接口文档和客户端代码，做到调用端代码、
服务端代码以及接口文档的一致性。
但即便如此，对于许多开发来说，编写这个 yml 或 json 
格式的描述文件，本身也是有一定负担的工作，特别是在
后面持续迭代开发的时候，往往会忽略更新这个描述文件，
直接更改代码。久而久之，这个描述文件也和实际项目渐
行渐远，基于该描述文件生成的接口文档也失去了参考意义。
所以作为 Java 界服务端的大一统框架 Spring，迅速将Swagger 
规范纳入自身的标准，建立了 Spring-swagger 项目，后面改
成了现在的 Springfox。通过在项目中引入 Springfox，可以扫
描相关的代码，生成该描述文件，进而生成与代码一致的接
口文档和客户端代码。这种通过代码生成接口文档的形式，
在后面需求持续迭代的项目中，显得尤为重要和高效。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210710161346361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
 总结：Swagger 就是一个用来定义接口标准，接口规范，同时
 能根据你的代码自动生成接口说明文档的一个工具。
```
## 2 官方提供的工具
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210710161427675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

```markdown
Swagger Codegen：通过Codegen 可以将描述文件生成 html 
格式和 cwiki 形式的接口文档，同时也能生成多种语言的服务
端和客户端代码。支持通过 jar 包、docker、node 等方式在
本地化执行生成。也可以在后面的 Swagger Editor 中在线生成。

Swagger UI：提供了一个可视化的 UI 页面展示描述文件。
接口的调用方、测试、项目经理等都可以在该页面中对相
关接口进行查阅和做一些简单的接口请求。该项目支持在
线导入描述文件和本地部署 UI 项目。

Swagger Editor：类似于 Markdown 编辑器的编辑 Swagger
描述文件的编辑器，该编辑器支持实时预览描述文件的更新
效果，也提供了在线编辑器和本地部署器俩种方式。

Swagger Inspector：感觉和 Postman 差不多，是一个可以对
接口进行测试的在线版的 postman。比如在 Swagger UI 里面
做接口请求，会返回更多的信息，也会保存你请求的实际请求
参数等数据。

Swagger Hub：集成了上面所有项目的各个功能，你可以以项目
和版本为单位，将你的描述文件上传到 Swagger Hub 中。在 
Swagger Hub 中可以完成上面项目的所有工作，需要注册账号，
分免费版和收费版。

Springfox Swagger：Spring 基于 Swagger 规范，可以将基于 
SpringMVC 和 Spring Boot 项目的项目代码，自动生成 JSON 
格式的描述文件。本身不是属于 Swagger 官网提供的，在这
里列出来做个说明，方便后面作一个使用的展开。
```
## 3 构建 Swagger 与 Spring Boot 环境
### 3.1 引入依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

### 3.2 编写 Swagger 配置类
```markdown
这个配置类基本都是不变的。
```

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2  
public class SwaggerConfig {

    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .pathMapping("/")
                .select()
                // 扫描哪个接口的包
                .apis(RequestHandlerSelectors.basePackage("com.baiyi.controller"))
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
                        .title("标题: SpringBoot 整合 Swagger 使用")
                        .description("详细信息: SpringBoot 整合 Swagger,详细信息......")
                        // 版本信息
                        .version("1.1")
                        // 开发文档的联系人
                        .contact(new Contact("baiyi", "http://www.baidu.com","1101293873@qq.com"))
                        .license("This Baidu License")
                        .licenseUrl("http://www.baidu.com")
                        .build());
    }
}
```

### 3.3 启动 SpringBoot 项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160315912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)


### 3.4 访问 Swagger 的 UI 界面
```markdown
访问 Swagger 提供的 UI 界面：http://localhost:8080/swagger-ui.html
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210710161539594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70)

## 4 使用 Swagger 构建

### 4.1 开发 Controller 接口

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/user")
public class HelloController {

    @GetMapping("/findAll")
    public Map<String,Object> findAll(){
        Map<String, Object> map = new HashMap<>();
        map.put("success", "查询所有数据成功");
        map.put("status", true);
        return map;
    }
}
```

### 4.2 重启项目访问接口界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160528547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 5 Swagger 注解
### 5.1 @Api
```markdown
作用：用来指定接口的描述文字
修饰范围：用在类上
```


```java
@RequestMapping("/user")
@Api(tags = "用户服务相关接口描叙")
public class HelloController {

		....
}
```

###  5.2 @ApiOperation
```markdown
作用：用来对接口中具体方法做描叙
修饰范围：用在方法上
```
```java
@GetMapping("/findAll")
@ApiOperation(value = "查询所有用户接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来查询所有用户信息的接口")
public Map<String,Object> findAll(){
    Map<String, Object> map = new HashMap<>();
    map.put("success", "查询所有数据成功");
    map.put("status", true);
    return map;
}
```

### 5.3 @ApiImplicitParams
```markdown
作用：用来接口中参数进行说明
修饰范围：用在方法上
```

#### 5.3.1 普通参数使用
```java
@PostMapping("save")
@ApiOperation(value = "保存用户信息接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来保存用户信息的接口")
@ApiImplicitParams({
        @ApiImplicitParam(name = "id", value = "用户 id", dataType = "String", defaultValue = "21"),
        @ApiImplicitParam(name = "name", value = "用户姓名", dataType = "String", defaultValue = "白衣")
})
public Map<String, Object> save(String id, String name) {
    System.out.println("id = " + id);
    System.out.println("name = " + name);
    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", name);
    return map;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160622895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 5.3.2 RestFul 风格使用
```markdown
如果使用的是 RestFul 风格进行传参，必须再添加一个
paramType="path"
```

```java
@PostMapping("save/{id}/{name}")
@ApiOperation(value = "保存用户信息接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来保存用户信息的接口")
@ApiImplicitParams({
        @ApiImplicitParam(name = "id", value = "用户 id", dataType = "String", defaultValue = "21", paramType = "path"),
        @ApiImplicitParam(name = "name", value = "用户姓名", dataType = "String", defaultValue = "白衣", paramType = "path")
})
public Map<String, Object> save(@PathVariable("id") String id,@PathVariable("name") String name) {
    System.out.println("id = " + id);
    System.out.println("name = " + name);
    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", name);
    return map;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160716695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

#### 5.3.3 JSON 格式使用
```markdown
如果是 RequestBody 的方式，需要定义一个对象进行接收。
```
```markdown
1. 定义一个 User 对象
```
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String id;
    private String name;
}
```
```markdown
2. 编写 Controller
```
```java
@PostMapping("save2")
public Map<String, Object> save2(@RequestBody User user) {
    System.out.println("id = " + user.getId());
    System.out.println("name = " + user.getName());
    Map<String, Object> map = new HashMap<>();
    map.put("id", user.getId());
    map.put("name", user.getName());
    return map;
}
```
```markdown
3. 重启项目，打开 UI 界面
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160752184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
测试：
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160817381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
### 5.4 @ApiResponses
```markdown
作用：用在请求的方法上，表示一组响应
修饰范围：用在方法上
```


```java
@PostMapping("save2")
@ApiResponses({
        @ApiResponse(code = 404, message = "请求路径不对"),
        @ApiResponse(code = 400, message = "程序不对")
})
public Map<String, Object> save2(@RequestBody User user) {
    System.out.println("id = " + user.getId());
    System.out.println("name = " + user.getName());
    Map<String, Object> map = new HashMap<>();
    map.put("id", user.getId());
    map.put("name", user.getName());
    return map;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201022160847125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)
```markdown
想要获取该该课程markdown笔记（脑图+笔记）。可以扫描以下
微信公众号二维码。或者搜索微信公众号-Java大世界。回复swagger
即可获取笔记获取方式。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070416020088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

