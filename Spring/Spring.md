#### Spring
##### [[介绍一下Spring]]
Spring是一个基于IOC和AOP的框架。提供了丰富的工具，简化了开发。

##### [[Spring的启动流程]]
1. 创建 Spring 容器
2. 读取配置
3. 创建并注册 Bean
4. 初始化 Bean
5. 完成启动

#### IOC
##### [[介绍一下IOC]]
1. IOC就是控制反转，原先需要手动New对象进行创建，而现在将对象创建的过程交由Spring容器进行管理，Spring将对象注册成Bean存储到Spring容器中。当需要使用对象时，由Spring进行对象的注入，并且当对象创建需要依赖其他对象时，也交由Spring进行依赖的管理，开发者不需要感知下层的细节。
优点：
1. 减少了对象创建的数量，节省内存
2. 在注册Bean时可以对Bean的功能增强。
	1. Bean的生命周期
3. 使开发者对对象创建的过程无感知，使开发更加简单
	1. 三级缓存解决循环依赖

##### [[Bean的生命周期]]
1. Bean的生命周期主要依赖Spring中的两个类
	1. AbstractAutowireCapableBeanFactory负责创建
		1. 实例化
		2. 设置属性
		3. 是否有实现了Aware接口
		4. BeanPostProcessors前置处理
		5. 是否实现initializingBean
		6. 是否自定义init-methd
		7. BeanPostProcessors后置处理
		8. 注册Destruction回调
	2. 正常使用
	3. DisposableBeanAdapter负责销毁。
		1. 是否实现DisposableBean接口
		2. 是否自定义的destroy-method

##### [[Bean的作用范围]]
1. singleton
2. prototype
3. request
4. session
5. golbal-seesion

##### [[循环依赖问题]]
1. Spring引入了三级缓存解决循环依赖问题。
2. 假设有两个对象A、B互相依赖
3. 创建A对象时先检查A对象在三级缓存中是否存在，若不存在则实例化A，并将A的工厂加入到三级缓存中
4. 又因为A依赖B对象，则检查三级缓存中是否有B对象，若不存在，则将B实例化，并将B的工厂放入三级缓存中
5. B又依赖A，再检查缓存时就会发现一二级缓存中不存在A，但三级缓存中存在A的对象工厂，B就会利用A的工厂创建半成品A，并放入二级缓存
6. 同时向B注入A，B注入A后创建完成，将B放入一级缓存
7. 再向A注入B，将A放入一级缓存

##### [[为什么不用二级缓存解决循环依赖问题]]
1. 和AOP相关

##### [[@Autowire实现原理]]
1. 处理@Autowired注解： Spring会在初始化Bean的时候，检查类中的所有字段和方法。如果字段或方法上有`@Autowired`注解，Spring会尝试自动装配这些依赖。
2. 在Bean初始化阶段，会调用doCreateBean()方法，在doCreateBean()方法中会调用populateBean()，populateBean()会调用两次后置处理器，第一次判断是否需要属性填充，如果需要则第二次调用Bean后置处理器AutowiredAnnotationBeanPostProcessor的postProcessPropertyValues()方法，该方法会进行@Autowired注解的解析，实现自动装配。
#### AOP
##### [[介绍一下AOP]]
1. AOP即面向切面编程，将公共的逻辑抽取出来，复用能力，减少代码开发。
2. 核心是动态代理
	1. JDK动态代理，接口
	2. Cglib动态代理，继承
3. 实现是在Spring初始化Bean时(BeanPostProcessors后置处理)创建代理对象

##### [[IOC和AOP之间有什么联系]]
1. IOC实现对Bean的创建和管理，而AOP的实现在Bean的创建过程中创建代理对象。

##### [[讲一下约定优于配置]]
1. “约定优于配置”意味着框架会提供合理的默认行为，开发者只需要在必要时进行配置，而无需为每个细节进行显式配置。这种方式减少了配置文件的复杂度和冗余，提高了开发的效率和代码的可读性

#### 事务
##### [[介绍一下Spring的事务]]
1. REQUIRED，不存在事务则开启，存在事务加入，保证只有一个事务
2. REQUIREDS_NEW，每次执行新开一个事务，当前存在事务，则将当前事务挂起
3. SUPPORTS，有事务要加入事务，没事务普通执行
4. NOT_SUPPORTED，有事务暂停事务，没有普通执行
5. MANDATORY，强制有事务，没有事务报错
6. NEVER，有事务报异常
7. NESTED，之前有事务就创建嵌套事务，嵌套事务回滚不影响父事务，父事务影响嵌套事务



#### SpringMVC
#####  [[介绍一下SpringMVC]]
1. 是Spring框架的一部分，用于构建Web应用程序。它遵循MVC设计模式，将Web应用程序的不同职责分离开来，以便于管理和维护。
2. Spring MVC在启动的时候，会把带有@RequestMapping注解的方法和类封装成一个RequestMappingInfo和HandlerMethod，然后注册到MappingRegistry。当HttpServletRequest访问时，会通过AbstractHandlerMethodMapping#lookupHandlerMethod方法获取对应的HandlerMethod
3. 当http请求进入tomcat并在HttpServlet中处理的时候，首先会解析http request中的数据，以此来拿到对应的HandlerMethod（HandlerMethod封装了对应的Method和持有它的Bean）。明白了这一层，本问题就会从不同的request如何路由到不同的Controller变为不同的request如何拿到对应的HandlerMethod

##### [[SpringMVC的执行流程]]
1. 请求打到DispatcherServlet
2. DispatcherServlet收到请求，调用HandlerMapping
3. HandlerMapping返回一个执行链给DispatchServlet
4. DispatcherServlet调用HandlerAdapter
5. HandlerAdapter调用具体的处理器
6. 处理器返回ModelAndView给HanderAdapter，再返回给DispatcherServlet
7. DispatchServlet将ModelAndView传给ViewReslover
8. ViewResolver解析后返回具体的View
9. DippatcherServlet根据View进行渲染
10. DispatcherServlet响应response
#### SpringBoot
#####  [[SpringBoot的启动流程]]
1. 主要是两个方法
2. 一个是SpringBootApplication.run()
	1. 启动计时
	2. 获取和启动监听器
	3. 装配环境参数
	4. 打印Banner
	5. 创建应用上下文
	6. 准备上下文
	7. 刷新上下文
3. 一个是initialize()
	1. 添加源
	2. 设置Web环境
	3. 加载初始化器
	4. 设置监听器
	5. 确定主应用类

##### [[SpringBoot自动配置原理]]
1. 它会根据定义在classpath下的类，自动的给你生成一些Bean，并加载到Spring的Context中。
2. 核心是`EnableAutoConfiguration`注解，注解会引导Spring Boot根据项目中的依赖和配置，自动配置Spring应用程序
3. SpringBootApplication -> EnableAutoConfiguration ->EnableAutoConfigurationImportSelector -> Configuration
#### 杂
##### [[怎么使用spring连接多数据库，怎么管理这些数据库连接]]