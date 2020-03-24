
## Spring Boot整合Servlet（两种方式）

 1. 新建一个maven项目
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200324235403375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
创建完成后的结构图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200324235544882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
2. 引入pom.xml依赖

```java
	<!--引入父项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>
    <dependencies>
        <!--SpringBoot web启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### 第一种方式（通过注解扫描方式完成Servlet组件的注册）：
1. 通过注解扫描方式完成Servlet组件的注册
	1.1. 创建一个Servlet

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325001713401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
1.2. 编写Servlet代码：

```java
@WebServlet(name = "firstServlet",urlPatterns = "/firstServlet") //urlPatterns：访问路径
public class firstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("进来了firstServlet");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    doPost(request,response);
    }
```
1.3. 编写启动类
创建springboot启动类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325001838857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
代码：

```java
@SpringBootApplication
//在spring boot启动时会扫描@WebServlet注解，并创建该类的实例
@ServletComponentScan
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}

```

*注意：在启动类上需要加上@ServletComponentScan注解 意思是：在启动时扫描@WebServlet注解 ,创建Servlet的实例*

1.4. 运行启动类 ，在浏览器输入localhost:8080/firstServlet
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325002003926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
控制台输出信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325002042208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

### 第二种方式（通过方法完成Servlet组件的注册）

 1. 创建一个Servlet 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325002417820.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
2. 创建springboot启动类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325002804629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
在main方法下新建一个注册Servlet组件的方法

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    //添加一个方法，方法名无要求，必须返回ServletRegistrationBean。注册Servlet对象
    @Bean     //主键等价于<bean>标签
    public ServletRegistrationBean<SecondServlet> getServletRegistrationBean(){
        ServletRegistrationBean<SecondServlet> bean=
                new ServletRegistrationBean<SecondServlet>(new SecondServlet(),"/SecondServlet");
        return bean;
    }
}
```
3. 运行启动类 在浏览器输入 localhost:8080/SecondServlet
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325003033827.png)
4. 控制台打印信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325003104768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

## Springboot整合Filter （和整合Servlet方式差不多）
### 第一种方式（通过注解扫描方式完成Fliter组件的注册）
1. 新建一个Filter类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325003449636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
2. 继承Filter父类 实现接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325003713917.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
代码如下：

```java
@WebFilter(filterName = "firstFilter", urlPatterns = "/firstFilter")
public class firstFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        System.out.println("----进入FirstFilter-----");
        chain.doFilter(request, response);//放行
        System.out.println("----离开FirstFilter-----");
    }
}
```
3. 创建启动类
![](https://img-blog.csdnimg.cn/20200325003935133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
代码如下：

```java
@SpringBootApplication
//在spring boot启动时会扫描@WebServlet @WebFilter @WebListener注解，并创建该类的实例
@ServletComponentScan
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

}
```
运行启动类，在浏览器输入 localhost:8080/firstFilter
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325004155220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
*这里报404是因为没有写放行后的路径；*

 控制台打印信息：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325004256186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
 ### 第二种方式（通过方法方式完成Filter组件的注册）
 1. 创建Filter类 不用写@WebFilter注解
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325004526695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
 2. 启动类
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032500482515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
 代码如下：
 

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    //添加一个方法
    @Bean
    public FilterRegistrationBean<SecondFilter> getFilterRegistrationBean(){
        FilterRegistrationBean<SecondFilter> bean=
                new FilterRegistrationBean<SecondFilter>(new SecondFilter());
        bean.addUrlPatterns("*.do","*.jsp","/SecondFilter"); //以.do , .jsp ， SecondFilter结尾路径的都会进到过滤器
        return bean;
    }
}

```
3. 运行启动类 在浏览器输入 localhost:8080/SecondFilter
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032500501467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
控制台打印信息：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032500504038.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

## Springboot整合Listener (同上)
### 通过注解扫描方式完成Fliter组件的注册
1. 创建Listener类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325005605573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325005802938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
Listener代码:

```java
@WebListener()
public class firstListener implements ServletContextListener{
    //监听application对象的创建
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("-----------application对象创建-----------------");
    }
}

```
2. 创建启动类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325010014938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
代码：

```java
@SpringBootApplication
@ServletComponentScan  //在spring boot启动时会扫描@WebServlet @WebFilter @WebListener注解，并创建该类的实例
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```
3. 运行启动类 看控制台打印信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325010134181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
### 第二种方式（通过方法完成Listener组件注册）
代码一样 省略代码 直接上代码
1. 创建Listener类
2. 启动类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325010522767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
代码：

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
    @Bean
    public ServletListenerRegistrationBean<firstListener> getServletListenerRegistrationBean(){
        ServletListenerRegistrationBean<firstListener> bean=
                new ServletListenerRegistrationBean<firstListener>(new firstListener());
        return bean;
    }
}
```

3. 运行启动类 看控制台打印信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325010559439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

## Springboot访问静态资源（两种方式）
### 第一种方式（通过ServletContext的根目录下寻找静态资源）
1.在src/main 下创建一个webapp的目录（目录名称必须为webapp）
   在webapp下创建不同目录存放不同的静态资源，如：images 放图片 .
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011000989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
2. 运行启动类访问 直接访问资源路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011054971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011147630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
### 第二种方式（从classpath/static的目录下寻找静态资源）
在src/main/resources下创建一个static的目录（目录名称必须为static）
在static下创建不同目录存放不同的静态资源，如：images 放图片 .  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011406159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
2. 运行启动类访问浏览器 直接访问资源路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011503681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200325011147630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

以上就是本教程的相关内容，感谢观看，转载请注明出处
