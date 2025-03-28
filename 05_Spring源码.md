# Spring

![image-20250121004820056](/Users/zhangzizhao/Library/Application Support/typora-user-images/image-20250121004820056.png)

| Spring组件                         |                                                              |
| ---------------------------------- | ------------------------------------------------------------ |
| Bean                               | @Component 标记自定义类; 第三方类上没有@Component, 所以用@Configuration + @Bean |
| BeanDefinition(BD)                 | 一个BD描述一个Bean(Id, Class, 属性, 单例原型, 懒加载), 根据BD创建Bean. GenericBeanDefinition是通用的Bean信息; RootBeanDefinition包含了父类Bean信息. |
| AbstractAutowireCapableBeanFactory |                                                              |
| DefaultListableBeanFactory         | HashMap<String, BD>                                          |



## Spring启动过程

### 核心流程

1. 初始化BeanFactory, 包括BFPP, BPP.
2. 扩展点1: 执行所有BFPP的实现, 修改BeanDefinition, 包括注解的解析; 对${jdbc.url}进行占位符替换.
3. 根据BeanDefinition创建Bean. 为什么要用反射? 可以获取到类的属性, 方法, 构造器, 注解
   1. 实例化: 堆内开辟一块空间
   2. 属性注入
   3. 扩展点2: invokeAwareMethods()执行所有Aware的实现, 可完成容器对象属性注入
   4. 初始化
      1. 扩展点3-1: BPP.before()
      2. @PostConstruct
      3. 扩展点4-2: BPP.after() 实现AOP



### refresh()之前

```java
PathMatchingResourcePatternResolver() 用来读取Class
GenericApplicationContext.beanFactory = new DefaultListableBeanFactory();
ConfigurableApplicationContext SpringApplication.context = new AnnotationConfigServletWebServerApplicationContext();
```



### ApplicationContext.refresh()

| AbstractApplicationContext.refresh() |                                                              |
| ------------------------------------ | ------------------------------------------------------------ |
| preprareRefresh()                    | 初始化Environment; initPropertySources(): Spring为空; getEnvironment().validateRequiredProperties(); 初始化Listeners; 初始化Events; 初始化监听器和事件的集合对象 |
| obtainFreshFactory()                 | 创建并获取到ConfigurableListableBeanFactory类型的beanFactory |
| prepareBeanFactory()                 | 给当前的beanFactory设置属性值. 包括设置SpringEL解析器; 属性编辑器(可添加自定义); 添加BPP; 忽略一部分Aware实现类, 使用@Autowired时忽略这些Aware实现类, 留给BPP来处理; 织入?; 系统环境Bean放到一级缓存中; |
| postProcessBeanFactory()             | Spring是空                                                   |
| invokeBeanFactoryPostProcessors()    | ConfigurationClassPostProcessor.processConfigBeanDefinitions()解析配置类, 然后放入BeanDefinitions中; 此时BeanDefinition已经注册完成, 通过getBean()拿到所有BFPP的实现并且执行. |
| registerBeanPostProcessors()         | 实例化并且<u>注册</u>所有的BPP的实现, 包括ACAwareProcessor, AbstractAutoProxyCreator, AutowiredAnnotationBeanPostProcessor |
| initMessageSource()                  | 国际化                                                       |
| initApplicationEventMulticaster()    | 初始化事件多播器ApplicationEventMulticaster                  |
| onRefresh()                          | Spring是空, SpringBoot中启动Web容器                          |
| registerListener()                   |                                                              |
| finishBeanFactoryInitialization()    | 类型转换服务; 设置内置的值处理器; 创建Bean                   |
| finishRefresh()                      | 发布ContextRefreshedEvent事件, 监听该事件的监听器执行对应方法. |

创建Bean的顺序:

1. @Configuration
2. @Component
3. @Import 的类
4. @Bean
5. @Import的ImportBeanDefinitionRegistar
6. BeanDefinitionRegistryPostProcessor



### 解析配置类的过程

doProcessConfigurationClass()

1. @Component
2. @PropertySources, @PropertySource
3. @ComponentScans, @ComponentScan
4. proceessImports() 解析@Import
5. @ImportResource



### 创建Bean的过程

判断是否是FactoryBean, 如果是则getBean(&+beanName);

- getBean()
  - transformedBeanName() 处理&
  - getSingleton() ==双重检查锁==避免并发时获取到不完整的Bean, 从一二三级缓存中判断是否有具体的实例
    - 判断一级缓存有没有
    - 如果没有, 判断当前要获取的对象是否正在被创建过程中
    - 如果正在被创建过程中, 判断二级缓存有没有
    - 如果对象为空且允许早期引用(提前暴露), 则单例双重检查, 依次从一二三级缓存获取
  - createBean()
    1. 获取Bean的Class
    2. resolveBeforeInstantiation(), 执行InstantiationAwareBPP的实现, 返回当前Bean的代理对象, 如果包含aop的相关处理(AbstractAutoProxyCreator), 会在此处生成Advisor对象, 方便后续进行调用.
    3. doCreateBean()
      - 创建Bean的包装对象
      - createBeanInstance(): 
        - 获取Bean的Class
        - 如果包含Supplier, obtainFromSupplier()通过Supplier创建对象.
        - 如果包含FactoryMethod, 通过FactoryMethod创建对象.
        - 通过BPP获取指定的构造方法.
        - instantiateBean() 获取默认构造方法创建对象, 并且将其包装为BeanWrapper.
        - applyMergedBeanDefinitionPostProcessors() 处理@Autowired, @Value, @Resource.
        - populateBean() 
          - autowireByName(), autowireByType()
          - applyPropertyValues() 属性值设置
            - setPropertyValues() 反射调用set()给Bean设置属性值
        - initializeBean()
          - invokeAwareMethods()
          - applyBeanPostProcessorsBeforeInitialization() BPP的before()
          - invokeInitMethods()
          - applyBeanPostProcessorsAfterInitialization() BPP的after()
  - applyBeanPostProcessorsBeforeInitialization()
  - applyBeanPostProcessorsAfterInitialization(): AOP创建代理对象
    - BPP.after() --> AutoProxyCreator.wrapIfNecessary(), 默认Cglib



## Spring的扩展点

| Spring的扩展点                   |                                                              |
| -------------------------------- | ------------------------------------------------------------ |
| BeanDefinitionRegistryPP         | BFPP的子接口, 传来一个BeanDefinitionRegistry, 通过这个注册器可以管理BeanDefinition |
| BFPP                             | 传来一个ConfigurableListableBeanFactory, 可以用它修改BeanDefinition, 或者添加没有配置的Bean的BeanDefinition. |
| Spring模板方法                   | 通过子类继承实现扩展                                         |
| BPP                              |                                                              |
| InitializingBean                 | afterProperties(), 属性赋值之后扩展                          |
| 自定义监听器监听Spring发布的事件 |                                                              |



## Bean的生命周期

1. 生
   1. 配置
   2. BeanDefinition, BeanFactoryPostProcessor.postProcessBeanFactory() 加载配置文件并且将其构造为BeanDefinition
   4. 实例化 DefaultListableBeanFactory.createBeanInstance()
   5. 属性(依赖)注入
      1. 自定义属性赋值(用BeanPostProcessor实现): AbstractAutowireCapableBeanFactory.populateBean()
      2. InitializingBean.afterPropertiesSet()
      3. 容器对象属性赋值: aware(), AbstractAutowireCapableBeanFactory.invokeAwareMethods(), 赋值BeanName, BeanClassLoader, BeanFactory
   6. 初始化: 
      1. BeanPostProcessor.postProcessBeforeInitialization(): 大部分Aware: Environment, ApplicationEventPublisher, ApplicationContext等
      2. @PostConstruct配置的回调(init)方法, AbstractAutowireCapableBeanFactory.invokeInitMethods()调用
      3. BeanPostProcessor.postProcessAfterInitialization() 生成代理对象
2. 死: 调用ApplicationContext.close()
   1. 销毁回调方法@PreDestroy



创建Bean的方式: 1.反射调用无参构造函数; 2.工厂方法, 就是配置factory-bean和factory-method. 配置类可以理解为一个工厂, FactoryBeanName就是有@Configuration配置类的名称, FactoryMethodName就是有@Bean的方法.



## 循环依赖问题

循环依赖: 有个A对象，它的属性是B对象，而B对象的属性也是A对象. 也就是A依赖B，而B又依赖A

1. ApplicationContext.getBean(A)获取不到
2. 实例化A
3. 对A的属性B进行注入, 调用ApplicationContext.getBean(B)获取不到
   1. 实例化B
   2. 对B的属性A进行注入, 调用ApplicationContext.getBean(A)获取不到.
      1. 实例化A
      2. ...一直循环



三级缓存, 也就是三个Map, DefaultSingletonBeanRegistry中的三个属性, 

ApplicationContext.getBean()从一级缓存singletonObjects中获取Bean. 如果Bean进行了AOP, 则放入singletonObjects中的是生成的代理对象.

1. 一级缓存: singletonObjects<BeanName, 最终对象>, 
2. 二级earlySingletonObjects<BeanName, Bean(还没进行属性注入)>, 由三级缓存放进来
3. 三级singletonFactories<BeanName, ObjectFactory<?>>, 这个ObjectFactory是一个函数式接口, 或者说lambda表达式. (lambda表达式作为参数放到方法的实参中, 在方法执行的时候不会调用lambda, 只有在调用getObject()的时候才会调用lambda). ObjectFactory<?\>永远是getEarlyBeanReference(beanName, rootBeanDefinition, bean) 



循环依赖问题的步骤:

1. A实例化, 放在三级缓存
2. A属性注入时, 发现依赖B, 又去实例化B
   1. 实例化B
   2. B属性注入时, 需要获取A对象, 又去实例化A
      1. 实例化A但<u>发现A对象正在创建过程中</u>, 于是从三级缓存里拿出ObjectFactory, 执行ObjectFactory.getObject()得到A(提前暴露A). 将A移入二级缓存(此时A是半成品)
      2. 得到A, B属性注入成功, 将B移入一级缓存.
   3. 实例化B成功, 返回
3. A属性注入成功, 将A放入一级缓存
4. A实例化成功

getEarlyBeanReference(beanName, rootBeanDefinition, bean): 如果bean需要代理, 则创建Bean的代理对象并且返回; 如果不需要, 则直接返回bean.



第三级缓存可以不要吗? 如果不需要代理, 就可以不要. 

属性注入是在生成代理对象之前执行的, 那怎么将一级缓存中的原对象替换? 需要在属性注入的时候判断是否生成代理对象.

为什么非要用lambda? <u>对象什么时候需要提前暴露是没办法确定的</u>, 所以只有在该对象被调用的时候才可以判断是否需要代理, 使用lambda可以实现这样的类似回调机制.

在给B赋属性A的值时, 调用lambda, 判断赋的是A的原始对象, 还是A的代理对象.



## @Import

- 导入其他配置类, 实现配置的模块化. 
- @EnableXXX本质就是一个@Import(XXConfig.class), 加上@EnableXXX才会导入指定配置类, 从而创建相应类的Bean.
- 导入实现ImportSelector接口的类, 为指定的类创建Bean
- 导入实现ImportBeanDefinitionRegistrar接口的类, 直接操作注册器, 可以为指定的类创建Bean



## @Configuration

不加@Configuration也可以配置@Bean.

@Configuration用来代替xml, 加了@Configuration会为配置类创建cglib==动态代理==, 保证配置类中@Bean的单例.

```java
@Configuration
public class Config {
  	@Bean
    public AService aService() {
        AService aService1 = bService();
        AService aService2 = bService(); // 加了@Configuration才能用代理来保证每次调用时都是getBean()操作
        ...
    }
  
  	@Bean
    public BService bService() {
        return new BService();
    }
}
```

启动Spring时注册ConfigurationClassPostProcessor(实现了==BFPP(创建动态代理)==和BDRegistryPostProcessor(解析注解))



## AOP

| AOP的实际应用                                                |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 我们要对某些方法进行监控(QPS、RT、ERROR), 上报给监控系统的代码是非业务代码, 我们不希望这些方法里有这些非业务代码. | 自定义注解 + AOP, 只要方法上有我自定义的注解, 这个方法前后就会执行监控的代码 |
| 日志                                                         |                                                              |
| Spring事务                                                   |                                                              |



<u>要实现AOP需要配置的三个信息: 1.额外的非业务逻辑; 2.执行额外非业务逻辑的位置; 3.怎么筛选这个具体位置</u>

| AOP的一些概念    |                                                              |
| ---------------- | ------------------------------------------------------------ |
| Aspect切面       | 额外的逻辑类                                                 |
| Pointcut切点     | 哪些 要增强的业务方法(连接点)会被通知拦截(增强). 通常是==注解==, 通配符, 切点表达式 |
| JointPoint连接点 | 具体 要增强的业务方法                                        |
| Advice通知:      | 增强的具体行为 @Around, @Before, @After, @AfterThrowing, @AfterReturning. |
| Advisor          | Advice的包装, 负责连结Pointcut和Advice.                      |



在创建第一个Bean之前, 必须先把AOP需要的相关对象提前准备好, 因为无法预估哪些对象需要动态代理. 

- 准备BeanDefinition: AspectJExpressionPointcut, Advisor, AspectJAutoProxyCreator.

在哪个步骤可以提前实例化并且生成对应的对象?

BeanFactoryPostProcessor是用来对BeanFactory进行修改操作的, 一般修改BeanDefinition, 不会影响到后续实例化过程. 

BeanPostProcessor的子类AbstractAutowireCapableBeanFactory.resolveBeforeInstantiatoin() --> applyBeanPostProcessorsBeforeInstantiation() --> postProcessBeforeInstantiation() --> AbstractAutoProxyCreator.createProxy()



Aop的实现过程:

@EnableAspectJAutoProxy会通过@Import注册一个BPP处理AOP

1. 解析切面: 创建Bean之前的第一个BPP解析切面@Aspect(就是解析@Aspect, @Before, @After, @PointCut成一个一个advisor, advisor管理advice以及切点表达式, 不同的advice可以设置成不同的切点表达式, 所以一个通知就会解析成一个advisor, 比如一个前置通知对应一个advisor, 一个后置通知对应一个advisor), 将切面中所有通知解析为advisor(该对象包含通知, 切点, 通知方法)排好序放入List.

2. 创建动态代理: 初始化后的BPP.after(), getAdvicesAndAdvisorsForBean()拿到之前缓存的advisor的切点PointCut, 判断当前Bean是否被切点命中, 如果匹配则为其创建动态代理

3. 调用: 拿到动态代理对象, 调用方法就会判断当前方法是否是目标方法, 将advisor转换为Interceptor, 通过==责任链模式递归执行==通知(环绕, 前置, 后置, 异常, 返回).

   1. CglibAopProxy.DynamicAdvisorInterceptor.intercept() 开始执行具体流程

   2. getInterceptorsAndDynamicInterceptionAdvice() 获取消息通知的责任链

   3. MethodInvocation有个List\<MethodInterceoptor\>, MethodInvocation.proceed() 执行责任链

      - 每个结点: MethodInterceptor.invoke()

        - 第一个结点ExposeInvocationInterceptor把MethodInvocation设置到ThreadLocal中, 方便在链中直接进行获取, 并且根据原来的下标依次获取元素. 再递归调用MethodInvocation.proceed()
        - 每个结点递归调用ThreadLocal中的MethodInvocation.proceed()

        


## Spring事务

实现原理是AOP.

| 事务的传播                           | Spring中 一个事务方法A 调用了 另一个事务方法B, 就会有传播行为 |
| ------------------------------------ | ------------------------------------------------------------ |
| PROPAGATION_REQUIRED(融入)           | 已经在事务中, 则不新建事务. 也就是A和B算一个事务             |
| PROPAGATION_SUPPORTS                 | 无论如何都不新建事务                                         |
| PROPAGATION_MANDATORY                | 必须<u>在</u>事务中, 如果当前<u>不在</u>事务中, 就抛出异常   |
| PROPAGATION_REQUIRES_NEW(创建新事物) | 无论如何都新建事务. 也就是A和B算两个事务, 各干各的           |
| PROPAGATION_NOT_SUPPORTED            |                                                              |
| PROPAGATION_NEVER                    | 必须<u>不在</u>事务中, 如果当前<u>在</u>事务中, 就抛出异常   |
| PROPAGATION_NESTED                   |                                                              |

事务挂起会怎样?



Spring事务的AOP过程:

1. 解析切面: @EnableTransactionManagement注册一个配置类, 该配置类配置了advisor; Bean的创建前第一个BPP解析advisor, 切点是@Transactional
2. 创建动态代理: BPP创建动态代理, 如果有@Transactional就创建动态代理
3. 调用动态代理 TransactionInterceptor.invoke() --> TransactionAspectSupport.invokeWithinTransaction() 
   1. createTransactionIfNecessary() 
      1. getTransaction() --> DataSourceTransactionManager.doBegin() ==开启事务==: 获取数据库连接, 设置事务属性, 设置事务自动提交关闭
      2. prepareTransactionInfo() 创建事务信息对象存在ThreadLocal中(一个线程只有一个事务, 所以Spring声明式事务如果用多线程不能达到一致性), 包含事务处理所需的属性.
   2. invocation.proceedWithInvocation() 执行业务方法
   3. 如果出现异常则 completeTransactionAfterThrowing() ==回滚事务==
   4. cleanupTransactionInfo() 清除事务信息对象
   5. commitTransactionAfterReturning() 提交事务



事务传播:

融入: 拿到ThreadLocal中的Connection, 共享一个连接, 共同提交和回滚.

创建新事务: 将新事务存入ThreadLocal, 老事务暂存, 新事务结束后, 老事务恢复到ThreadLocal.



Spring事务失效的原因就是AOP失效的原因.



## Spring事件监听

用于和别的框架集成. 观察者模式, 就是用来解耦.

事件Event

监听器Listener

发布器EventMulticaster: ApplicationContext.publishEvent()



## FactoryBean

工厂方法设计模式.

实现BeanFactory的时候必须遵循Spring Bean完整的生命周期, 实例化, 初始化, Aware, Init, before, after, 如果需要更加便捷简单的方式, 就需要用FactoryBean, 不需要遵循这个流程.

而使用FactoryBean只需要调用getObject(), 整个过程的创建过程是由程序员自己控制, 更加灵活



例如, 要用FactoryBean创建A对象, 定义一个实现了FactoryBean的类AFactoryBean, getObject()相当于一个工厂方法, 创建并返回A对象的实例. 只有调用 getObject()的时候才会创建A的对象, 并且是<u>懒加载</u>的; 但AFactoryBean不是懒加载的.

Application.get("aFactoryBean")返回的是A; Application.get("&aFactoryBean")创建并返回的是AFactoryBean

A和AFactoryBean都还是交给Spring管理的, new ApplicationContext()会创建AFactoryBean的Bean, 通过他的BeanDefinition发现它是否实现BeanFactory接口. 创建出来的AFactoryBean放在一级缓存, 但是不会立马调用getObject()创建A. 

在Application.get("aFactoryBean")获取A的时候, 才会调用getObject()创建A, 放在另一个缓存Map FactoryBeanObjectCache中. 如果A不是单例, 则每次都重新创建, 缓存中不会保存.



## Spring整合Mybatis

Spring会忽略接口, Spring如何整合Mybatis?

1. MybatisAutoConfiguration配置了SqlSessionFactory和SqlSessionTemplate的Bean.
2. 创建SqlSessionFactoryBean时, InitializingBean.afterProperties()创建SqlSessionFactory赋值给他的属性.
3. Mybatis 通过继承Spring的扫描器 实现了一个 自定义扫描器ClassPathMapperScanner, 重写排除接口的方法, 扫描到接口的信息;
4. Mybatis 通过实现BeanDefinitionRegistryPostProcessor 实现一个自定义BFPP: 用扫描器扫描到的信息为<u>接口注册BeanDefinition</u>; <u>将BeanDefinition的BeanClass改为MapperFactoryBean</u>(因为Spring不能将接口实例化);
5. <u>Mapper接口的实例化</u>过程: MapperFactoryBean.getObject() --> SqlSessionTemplate.getMapper() --> MapperRegistry.getMapper() --> MapperProxyFactory.newInstance() 创建代理对象.
   1. 配置接口XXXMapper, 用来对表XXX进行操作, 每个方法上用注解配置对应的SQL
   2. 为XXXMapper创建代理对象, 代理对象方法执行流程:
      1. 获取方法上的注解, 解析注解的value
      2. 解析并替换占位符
      3. 数据库操作
      4. 处理结果集



# Mybatis

| JDBC的使用: SQL在代码中, 维护性差; 频繁创建和关闭数据库连接, 资源消耗大; 参数设置麻烦, 结果集处理麻烦; 没有提供**缓存**. | Mybatis的使用                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 加载驱动: Class.forName("com.mysql.cj.jdbc.Driver"); Driver中的静态代码块中会执行 DriverManager.registerDriver(new Driver()) 将Driver在DriverManager中注册. | Reader reader = Resources.getResourceAsReader("mybatis-config.xml"); |
| 获取连接: DriverManager.getConnection(url, username, password); 会通过SPI机制加载com.mysql.cj.jdbc.Driver. | 加载解析配置 SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader); |
| 创建Statement: 创建SQL的包装类 PreparedStatement对象, connection.prepareStatement(sql); | 开启会话 SqlSession sqlSession = sqlSessionFactory.openSession(); |
| 执行: preparedStatement.executeQuery();                      | 执行: Object result = sqlSession.selectOne("", 1);           |



## Mybatis SQL执行过程

1. MapperMethod 根据当前请求的方法解析对应的配置.
2. MapperProxy.invoke() --> SqlSessionTemplate.invoke() 
   1. SqlSessionUtils.getSqlSession() --> DefaultSqlSessionFactory.openSession() 创建会话对象
      1. 获取事务对象
      2. Configuration.newExecutor() 创建Executor, 初始化一级缓存, 二级缓存, 注册插件
      3. 创建并返回 一个DefaultSqlSession
   2. 用SqlSession执行SQL --> SimpleExecutor.doQuery()
      1. prepareStatement() 从数据源DataSource获取数据库连接, 创建Statement, 处理占位符
      2. PreparedStatementHandler.query() --> 
         1. ClientPreparedStatement.execute() 执行SQL
         2. 处理结果集 ResultSetHandler.handleResultSets()
   3. SqlSession.commit() 清除一级缓存



## Mybatis的缓存机制

| 缓存策略 |      |
| -------- | ---- |
|          |      |
|          |      |
|          |      |

1. BaseExecutor.query() 创建CacheKey: "接口名.方法名:SQL语句:参数";
2. PerpetualCache.getObject() 根据CacheKey查找一级缓存
3. 如果命中, 则直接返回; 如果未命中, 则去操作数据库.
4. 会话关闭, 清空一级缓存



# SpringMVC

Spring-MVC本质就是一个Servlet, 是对Servlet的封装, 屏蔽了Servlet的很多细节, 处理请求和生产响应.

- Servlet获取请求参数需要request.getParameter(); 有了SpringMVC, 我们将方法参数写成对象, SpringMVC会帮我们把参数封装成对象
- Servlet上传文件需要处理各种细节; SpringMVC的方法上定义出MultipartFile接口, 又可以屏蔽掉上传文件的细节了



## SpringMVC启动过程

1. 启动Tomcat, Tomcat启动过程加载 Web.xml的配置信息, 包含1.spring配置文件路径; 2.DispatcherServlet以及SpringMVC配置文件applicationContext.xml路径; 3.servlet和路径的映射; 4.监听器ContextLoaderListener
2. 监听器ContextLoaderListener监听到Tomcat启动后
   1. 根据 Web.xml 中配置的 Spring配置文件路径 加载 Spring配置文件.
   2. 创建Spring容器作为父容器
   3. 创建DispatcherServlet: 
      1. HttpServlet.init()读取Spring MVC的配置文件applicationContext.xml
      2. FrameworkServlet.initServletBean()创建SpringMVC容器作为子容器; 容器创建时会添加一个监听器, 刷新完成后发布事件ContextRefreshedEvent, 监听此事件的监听器会去调用 DispatcherServlet.initStrategies()容器并且初始化Spring MVC的核心内置组件, 包括HandlerMapping, 用来将request和Controller对应.
3. 创建HandlerMapping的实现类的Bean时, 执行InitializingBean.afterPropertiesSet(), 例如RequestMappingHandlerMapping.afterProperties() --> AbstractHandlerMethodMapping.afterProperties() --> initHandlerMethods() 
   1. 循环所有BeanName, 筛选出有@Controller的Bean.
   2. @Controller的Bean中找所有有@RequestMapping的方法(@GetMapping, @PostMapping等都包含@RequestMapping), 存在MappingRegistry中.





## SpringMVC处理请求

Filter

DispatcherServlet.doService()接受到请求 --> doDispatch(): 

1. getHandler() 根据request获取到对应的Controller和 拦截器, 封装在HandlerExecutionChain中.

   1. AbstractHandlerMapping.getHandlerInternal() 
      1. request中得到 /xxx
      2. 根据 /xxx 找到HandlerInterceptor拦截器和对应的方法Handler.

2. getHandlerAdapter() HandlerAdapter找到一个与当前handler适配的Adapter适配器, 用适配器来执行对应的Controller.

   (Controller有多种实现方式可能是@RestController+@RequestMapping, 也可能是实现了Contoller或者HttpRequestHandler接口. 适配器Adaptor就是去适配不同的方式, ==适配器模式==)

3. mappedHandler.applyPreHandle() 执行拦截器HandlerInterceptor.preHandle()

4. HandlerAdapter.handle()  --> ServletInvocableHandlerMethod.invokeAndHandle() 执行处理请求的方法

5. mappedHandler.applyPostHandle() 执行拦截器HandlerInterceptor.postHandle()

6. DispatcherServlet.processDispatchResult() 视图渲染



过滤器和拦截器:

过滤器依赖Servlet, 对所有请求起作用; 不能访问action的上下文.

拦截器依赖SpringMVC容器, 只对DispatcherServlet映射的action请求起作用; 可以访问Bean.



# SpringBoot

## SPI机制

Service Provider Interface

不同的数据库厂商根据JDK提供的接口写了自己的实现类, 还要在自己的META-INF/services下写一个配置文件, 文件名是接口名, 文件中写着实现类名, 这样Java程序员就不需要知道实现类是哪个, 也可以操作数据库.

没有SPI机制获取MySQL连接:

```java
Class.forName("com.mysql.cj.jdbc.Driver");
Connection conn = DriverManager.getConnection(url, user, password);
```

有了SPI机制获取MySQL连接:

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
```

```java
// SPI机制自动加载类, 不需要再手动加载
Connection conn = DriverManager.getConnection(url, user, password) --> ServiceLoader.nextProviderClass() 读取META-INF/services下的文件
```



## 自动装配原理与自定义Starter

> 在使用SpringBoot时, 有个autoconfigure包, 包里面有个spring.factories文件, 里面定义了很多第三方配置类. 比如redis, kafka的配置类名. 在启动SpringBoot的时候, 就会加载这个spring.factories文件, 进而去加载配置类, 因为配置类已经初始化了大部分配置信息, 所以我们使用相关组件非常方便. 我们在application配置文件写上对应的配置就能通过各种template类直接操作对应的组件了.

第三方依赖将配置类写在META-INF/spring.factories中, SpringBoot在启动的时候会读取所有依赖Jar包下的META-INF/spring.factories和META-INF/spring/aot.factories和org.springframework.boot.autoconfigure.AutoConfiguration.imports的文件内容, 读取到后加载这些文件配置的类.

@SpringBootApplication

- @SpringBootConfiguration

- @ComponentScan

- @EnableAutoConfiguration

  - @AutoConfigurationPackage

    - @Import({AutoConfigurationPackages.Registrar.class}

  - @Import({AutoConfigurationImportSelector.class}), 在执行所有BFPP时会执行 ConfigurationClassParser.parse() 先解析注解, 再执行this.deferredImportSelectorHandler.process() --> handler.processGroupImports() -->  DeferredImportSelector.Group.process():

    - AutoConfigurationImportSelector.getAutoConfigurationEntry()

    - getCandidateConfigurations() 读取所有Jar包中的/META-INFO/spring.factories文件, 结果全部是全类名

      > spring-autoconfigure-metadata.properties中配置了这些配置类被加载的条件, 比如RedisAutoConfiguration.ConditionOnClass=RedisOperations, 也就是当前项目有RedisOperations这个类时(Maven引入Redis依赖)才会加载RedisAutoConfuguration

    - getConfigurationClassFilter().filter(configurations) 循环3中ConditionalOnXX过滤策略: 如果Class.forName("全类名")抛出异常, 则说明没有这个类; 



**自定义Starter**

starter是为了方便调用放使用相关的组件, Spring框架也实现了好多好用的starter, 例如之前用Mybatis需要引入各种各样的包, 而starter就是做了一层封装, 用到的jar包都给包起来了, 并且整理好了版本.

现在很多组件都会提供springboot-starter包给我们用, 要做一个starter包, 参照Spring的内置实现, 1.引入starter打包相关的依赖; 2.创建spring.factories文件, 编写我们配置类的全类名.

Redis的配置类其实会有初始化RedisTemplate的操作, 如果我们没有引入redis-starter包, 是怎么通过编译的? autoconfigure包里会定义到相关的依赖, 但被标记为optional, 并且只在编译环境有效. 能通过编译但不会将其依赖传入我们的工程里.



## SpringBoot启动流程

1. new SpringApplication()

   - getSpringFactoriesInstances() 用<u>SpringFactoriesLoader</u>加载spring.factories文件, 将指定类型的实现类实例化

     调用了三次, 分别实例化BootstrapRegistryInitializer, ApplicationContextInitializer和ApplicationListener三种接口的所有实现类.

   - deduceMainApplicationClass() 获取(反推)main方法所在的Class对象

2. run()

   - createBootstrapContext()

   - listeners = getRunListeners() 这个listeners是发布器, 负责发布事件, 给所有的监听器发送通知
     - EventPublishingRunListener有个属性是SpringApplication, SpringApplication中存着ApplicationListener的所有实现类的实例, 可以通过访问属性SpringApplication application来发布事件, 通知所有监听器
   - listeners.starting() 发布starting事件, 所有监听starting事件的监听器会触发

   - prepareEnvironment()

   - createApplicationContext()

   - prepareContext()

     - refreshContext() --> <u>ApplicationContext:</u> refresh()

       - invokeBeanFactoryPostProcessors(beanFactory)

         - <u>PostProcessorRegistrationDelegate:</u> invokeBeanFactoryPostProcessors() invokeBeanDefinitionRegistryPostProcessors() invokeBeanDefinitionRegistryPostProcessors() 
           - <u>ConfigurationClassPostProcessor:</u> postProcessBeanDefinitionRegistry() processConfigBeanDefinitions() 
             - <u>ConfigurationClassParser(解析配置类, 解析@Component, @Import等):</u> parse()  --> process() --> processGroupImports() --> getImports()
               - A<u>utoConfigurationImportSelector:</u> process() --> getAutoConfigurationEntry()

       - onRefresh() --> <u>ServletWebServerApplicationContext</u>.onRefresh() --> createWebServer()

         - getWebServerFactory() 

         - factory.getWebServer() --> getTomcatWebServer() --> 

           - new TomcatWebServer() --> initialize() -->

             - tomcat.start() 启动Tomcat 

             - startDaemonAwaitThread(): 挂起Tomcat线程, 等待请求

               ```java
               new Thread("container-" + containerCounter.get()) {
                   public void run() {
                       TomcatWebServer.this.tomcat.getServer().await();
                   }
               };
               ```

   - afterRefresh()



SpringCloud为什么要有两个容器? 可以在系统容器创建之前做一些操作.

配置中心的配置加载, 就是在BootStrap容器创建的时候完成的: 

1. 构造方法中加载监听器BootstrapApplicationListener, ConfigFileApplicationListener;
2. run()发布ApplicationEnvironmentPreraredEvent事件
   1. BootstrapApplicationListener 再次执行run(), 创建了新的SpringApplication对象,  加载解析bootstrap.properties, 里面配置了配置中心的信息, 去远程配置中心(nacos, zookeeper)拉取配置信息
   2. ConfigFileApplicationListener, 加载解析application.yml



## SpringBoot内置Tomcat原理

Spring的 refresh().onrefresh()没有实现, SpringBoot将其实现了.

ServletWebServerApplicationContext.createWebServer()

- getWebServerFactory(): BeanFactory.getBeanNamesForType(ServletWebServerFactory.class)
- TomcatServletWebServerFactory.getWebServer() 创建Tomcat, 并且this.tomcat.start()启动, this.tomcat.await()挂起等待请求.



## 监听器作为扩展点

spring.fatories记录了所需要的监听器类, SpringBoot在启动的时候会将这些类实例化并存在SpringApplication的属性中.

当容器启动的某一步, 会发布事件, 比如prepareEnvironment()中, 有listeners.environmentPrepared(), prepareEnvironmentEvent发生了,  listeners通知给所有订阅了这个事件的监听器, EnvironmentPostProcessorApplicationListener收到通知就会去执行onApplicationEvent(), 去加载解析==属性文件==.



SpringBoot启动过程发布了以下事件:

1. Starting
2. EnvironmentPrepared: ConfigFileApplicationListener配置文件加载.
3. Initialized
4. Prepared
5. Started
6. Ready
7. Failed



## 为什么SpringBoot的jar可以直接运行

插件spring-boot-maven-plugin把程序打成可执行jar.

是个Fat jar, 包含依赖的jar

java -jar 回去找jar中的manifest文件, 里面记录着main所在的类.



# Tomcat

客户端发送一个请求Socket, 就是一堆字节输入流InputStream, Tomcat根据协议将这堆字节封装成一个ServletRequest对象, 传给Servlet, Servlet就可以使用这个request对象处理请求, 将要返回的响应信息封装成一个ServletResponse对象, 返回给Tomcat, Tomcat将ServletResponse对象转换为字节流OutputStream



一个请求http://red.com:8080/mall/loginServlet

1. http://red.com:8080定位到Connector
2. http://red.com定位到Host(已经定位到Connector后, 通过red.com找虚拟主机)
3. /mall 定位到Context
4. /loginServlet定位到Servlet



## Tomcat启动

- Connector.init() new了一个ServerSocket绑定端口
- ContextConfig.webConfig() 加载解析web.xml中配置的Servlet, 将这些Servlet包装为Wrapper对象
- StandardContext.loadOnStartup() 每个wrapper都wrapper.load() --> servlet.init()初始化Servlet





## Tomcat接收请求

1. 请求到达Connector
2. ProtocalHandler处理请求
   1. Endpoint监听请求
   2. Acceptor
   3. Executor线程池分配一个线程
   4. Processor将 Socket 解析为 TomcatRequest
3. Adaptor 将TomcatRequest 适配为 ServletRequest



## Tomcat的自定义类加载器











