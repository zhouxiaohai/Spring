## Spring注入全局的HttpServletRequest

<br/>

**问题:** *由于对HttpServletRequest机制加载不了解,盲目使用出现问题说不清原理o(╥﹏╥)o 尴尬*

<br/>

> **Spring加载HttpServletRequest原理**

文描:

```
Spring在容器初始化的时候就将HttpServletRequest注册到了容器中 (Spring官网文档),在WebApplicatonContextUtils类中注入HttpServletRequest等全局类,并在AbstractApplicationContext中refresh.
```

图解:

1.0

![361817-20160330101950816-1201136852.png](F:/开发设计文档/Spring/361817-20160330101950816-1201136852.png "")

2.0
![361817-20160330101932738-1728384835.png](F:/开发设计文档/Spring/361817-20160330101932738-1728384835.png "")

3.0
![361817-20160330102029832-879776485.png](F:/开发设计文档/Spring/361817-20160330102029832-879776485.png "")

4.0
![361817-20160330102820582-1440240323.png](F:/开发设计文档/Spring/361817-20160330102820582-1440240323.png "")

---------------------------------------------------------------------------------

<br/>

> JAVA中标配的引入HttpServletRequest

**分为四种:**

* @Autowired (*推荐使用*)

* public void Test(HttpServletRequest request1, HttpServletResponse resp,HttpSession session1) 
* ((ServletRequestAttributes)RequestContextHolder.getRequestAttributes()).getRequest(); 
* Global.getHttpServletRequest(); 该方式基于上面3封装。最灵活不需要每个action中都定义 HttpServletRequest 参数。


<br/>

> **结论**

    Spring巧妙地注入了一个装饰器代理了真实对象的操作，只在需要的时候获取真实的Request和Session对象，获取方法Servlet自身实现好了，和Servlet类获取Request和Session应该大同小异

* @Autowired获取到的是装饰器对象
* 方法参数获取到的是真实对象
* 方法和autowired都是线程安全 [https://my.oschina.net/sluggarddd/blog/678603]

<br/>

> **使用场景**

* 方法中携带:
    1. Spring未推出注解遗留下来的习惯
    2. Controller中方法使用在某一个(比如:KODController)

* Autowired
    1. Spring3注解化以后推荐自动注入
    2. 使用于多个方法中携带HttpServletRequest,避免代码冗余
    3. ServletRequest
    4. ServletResponse
    5. HttpSession
    6. Principal
    7. Locale
    8. InputStream
    9. Reader
    10. OutputStream
    11. Writer

--------------------------------------------------------------------

<br/>

> **业务隔离反思**

1.由于SpringMVC模式的形成@AutoWired HttpServletRequest中类似代理都应该在VIEW层中实现,由参数形式传入底层.

2.Session由统一模板管理

<br/>

> **为什么启动报错**

    
    springMVC 怎么在方法中注入HttpServletResponse对象
    
    https://segmentfault.com/q/1010000007495428

<br/>

> **其他**

1. Bean scopes的学习文章:

    https://waylau.gitbooks.io/spring-framework-4-reference/content/III.%20Core%20Technologies/Bean%20scopes.html

2. 在Spring 4.0 新功能和增强:

    https://waylau.gitbooks.io/spring-framework-4-reference/content/II.%20What%E2%80%99s%20New%20in%20Spring%20Framework%204.x/3.%20New%20Features%20and%20Enhancements%20in%20Spring%20Framework%204.0.html
    

<br/>

*....未完待续,学无止境*