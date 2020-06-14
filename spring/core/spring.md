## IOC

+ 注入方式

  1. 构造方法注入
  2. setter方法注入
  3. 接口注入

+ IOC容器职责

  + 业务对象的构建管理
  + 业务对象的依赖绑定

+ spring容器类型

  + BeanFactory

    最基础的容器，使用延迟加载机制，所以启动比较快

  + ApplicationContext

    继承了BeanFactory，在BeanFactory的基础上提供了类似事件发布、国际化等支持。启动的时候会把所有的bean都加载，并且构建好bean之间的关系，相对于BeanFactory，需要更多的资源。

+ bean作用域

  + singleton

    默认，一个IOC容器里面只有一个bean实例

  + prototype

    每次getBean都会返回一个新的实例

  + request

    每次http请求都会创建一个新的实例

  + session

    每个新的session创建一个新的实例

  + global-session

+ 容器启动

  1. 容器启动阶段

     1. 通过BeanDefinitionReader加载bean的配置，然后生成对应的BeanDefinition
     2. 把BeanDefinition注册到对应的BeanDefinitionRegistry
     3. 执行BeanFactoryPostProcessor。这个类会对一些符合要求的BeanDefinition进行修改。例如，PropertyPlaceholderConfigurer是用来把占位符设置为实际值。

  2. Bean实例化阶段

     1. 实例化bean对象

        使用策略模式来确定是应该用反射还是cglib来生成对应的bean对象，一般都是用cglib。生产的bean会被包裹在一个BeanWrapper当中。

     2. 通过BeanWrapper对bean进行属性设置

     3. 检查bean是否继承了各种Aware接口，如果实现了，就把对应的属性设置进去

     4. 执行BeanPostProcessor.postProcessBeforeInitialization方法

     5. 如果这个bean继承了InitializingBean，就执行afterPropertiesSet方法

     6. 执行配置的initMethod方法(注解是initMethod，xml配置是init-method)

     7. 执行BeanPostProcessor.postProcessAfterInitialization方法

     8. 检查bean是否实现了DisposableBean接口，或者配置了destroyMethod，如果有，就生产一个对应的对象用于销毁的时候进行回调。

+ Spring mvc工作流程

  ```mermaid
  graph LR
  	client[client]
  	DispatcherServlet[DispatcherServlet]
  	HandlerMapping[HandlerMapping]
  	HandlerAdapter[HandlerAdapter]
  	HttpMessageConverter[HttpMessageConverter]
  	Handler[Handler]
  	ModelAndView[ModelAndView]
  	ViewResolver[ViewResolver]
  	Model[Model]
  	View[View]
  	client-.1.->DispatcherServlet
  	DispatcherServlet-.2.->HandlerMapping
  	DispatcherServlet-.3.->HandlerAdapter
  	HandlerAdapter-.4.->HttpMessageConverter
  	HttpMessageConverter-.4.->Handler
  	HandlerAdapter-.5.->ModelAndView
  	ModelAndView-.5.->DispatcherServlet
  	DispatcherServlet-.6.->ViewResolver
  	DispatcherServlet-.7.->Model
  	Model-.7.->View
  	View-.8.->client
  ```

  1. DispatcherServlet接收来自客户端的请求
  2. DispatcherServlet通过HandlerMapping解析到对应的handler(我们写的controller)
  3. DispatcherServlet通过HandlerAdapter来调用Handler
  4. HttpMessageConverter会解析过来的请求参数，传递给Handler
  5. Handler处理完以后，会返回ModelAndView对象到HandlerAdapter和DispatcherServlet
  6. DispatcherServlet通过ViewResolver查找到实际的view
  7. DispatcherServlet把model传给对应的view
  8. view回传给客户端

+ 事务

  + spring事务管理方式

    + 编程式事务，在代码中硬编码
    + 声明式事务
      + 基于xml
      + 基于注解

  + spring事务隔离级别

    1. TransactionDefinition.ISOLATION_DEFAULT

       使用数据的的事务隔离级别

    2. TransactionDefinition.ISOLATION_READ_UNCOMMITTED

    3. 

  + spring事务传播

+ 