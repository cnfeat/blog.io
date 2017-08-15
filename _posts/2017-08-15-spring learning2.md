- spring实际上是一个容器框架，可以配置各种bean（action/service/domain/dao），并且可以维护各种bean间的关系(通过ref)。当需要使用某个bean时候，我们可以getBean(id)，使用即可。

- ioc是什么？

  答：ios(inverse of control)**控制反转**，就是把创建对象(bean)和维护对象(bean)的关系的权利从程序中转移到spring的容器(applicationContext.mxl)中，而程序本身不再维护。

  学习框架最重要的就是学习各个配置。

- DI是什么？

  答：di(dependency injection)依赖注入：实际上di和ioc是同一概念，spring的设计者认为di更能准确表示spring核心

- 通常把Applicationcontext做成一个单例

- web层（管理JSP、action、表单、数据输入、数据处理、数据输出）、model层（业务层、数据访问层、持久层）、数据库

- web层：action[解决action单例问题]，框架：Struts

- 业务层：service/domain/dao

- 持久层：数据源。框架：Hibernate

- spring框架，可以管理web层，业务层，dao层，持久层。该spring可以配置各个层的组件（bean），并且维护各个bean之间的关系。

### ioc

ioc或di还有解耦的作用。

- spring开发提倡接口编程，配合技术可以实现程序间的解耦。

举例说明：

现在我们体验一下spring的di配合接口编程，完成一个字母大小写转换的案例。

思路：

- 创建一个接口ChangeLetter

- 两个类实现接口

- 把对象配置到spring容器中

- 使用

  通过上面案例，可以初步体会到di配合接口编程，的确可以减少层之间的耦合。

### 配置bean

- 从ApplicationContext应用上下文容器获取bean和从bean工厂获取bean区别：

  案例：

  ~~~java
  public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		 // 1.ApplicationContext方法：当我们去实例化beans.xml，该文件中配置的bean被实例化
    		 ApplicationContext ac = new
  		 ClassPathXmlApplicationContext("com/hsp/ioc/beans.xml");
  		 Student student = (Student) ac.getBean("Student");
  		 System.out.println(student.getName());;
  		
  		// 2.从BeanFactory方法：
  		// 如果我们使用BeanFactory去获取bean，当你只是实例化该容器，
  		// 那么容器的bean不被实例化，只有当你去使用getBean某个bean时，才会实时的创建
  		BeanFactory factory = new XmlBeanFactory(new ClassPathResource("com/hsp/ioc/beans.xml"));
  		factory.getBean("Student");
  	}
  ~~~

  ​

  - 如果使用ApplicationContext，则配置的bean如果是singleton不管你用不用，都被实例化（好处就是可以预先加载，缺点就是耗内存）
  - 如果是bean工厂，则当你实例化该对象时候，配置的bean不会马上被实例化，当你使用的时候，才会实例化（好处就是节约内存，缺点就是速度慢）。在移动设备里利用bean工厂，节约内存。
  - 一般没有特殊要求，应当使用ApplicationContext完成。

- bean的scope的细节

  Bean作用域，如下如:

  ![Bean的scope细节](http://otfc4cl9r.bkt.clouddn.com/Bean%E7%9A%84scope%E7%BB%86%E8%8A%82.png)

- 入门案例：

- request、session是在web开发时候才有意义

- 三种获取ApplicationContext对象引用的方法

  1、ClassPathXmlApplicationContext：从类路径中加载(常用)

  2、FileSystemXmlApplicationContext：从何文件系统中加载

  3、XmlWebApplicationContext：从web系统中加载

- bean的生命周期

  为什么总是把一个生命周期当做一个重点？

  Servlet->servlet生命周期：init() 、destroy()

  java对象生命周期

  面试笔试，喜欢问相关问题

  - 1、 实例化（当我们的程序加载beans.xml文件），把我们的bean(前提是scope=singleton)实例化到内存
  - 2、调用set方法设置属性
  - 3、如果你实现了bean名字关注接口（BeanNameAware）则，可以通过setName得到bean id号
  - 4、如果你实现了bean工厂关注接口（BeanFactoryAware），则可以获取BeanFactory
  - 5、如果实现了ApplicationContextAware，则调用方法：


  - 6、如果bean和后置处理器BeanPostProcessor关联，则调用BeanPostProcessor的预初始化方法postProcessBeforeInitialization方法：

    ~~~java
    public Object postProcessAfterInitialization(Object arg0, String arg1) throws BeansException {
    		// TODO Auto-generated method stub
    		System.out.println("postProcessAfterInitialization函数被调用");
    		System.out.println(arg0 + "被创建的时间是:"+ new java.util.Date());
    		return arg0;
    	}

    	@Override
    	public Object postProcessBeforeInitialization(Object arg0, String arg1) throws BeansException {
    		// TODO Auto-generated method stub
    		System.out.println("postProcessBeforeInitialization函数被调用");
    		return arg0;
    	}
    ~~~

    BeanPostProcessor类似于我们的web filter

    需求：

    a、记录每个对象，被实例化的时间

    b、过滤每个调用对象ip

    c、给所有对象添加属性，或者函数=>aop（面向切面编程，针对所有对象编程）

  - 7、如果你实现了InitializingBean接口，则在postProcessAfterInitialization函数被调用之前，调用afterPropertiesSet()函数

  - 8、如果自己在<bean init-method="init" />则可以在bean定义自己的初始方法。

  - 9、调用后初始化方法：BeanPostProcessor的后初始化方法postProcessAfterInitialization

  - 10、使用我们的bean

  - 11、容器关闭

  - 12、可以通过实现DisposableBean接口，调用destroy()方法，释放各种资源

  - 13、使用<bean destroy-method="mydestroy" />调用定制的销毁方法。

    小结：

    实际开发中，我们没有这么多的过程，常见的是：

    1->2->6->9->10->11->13

- aop初探

  - 针对很多对象编程

- 问题：通过BeanFactory获取ApplicationContext对象，一样吗？

  不一样，bean在工厂中创建的生命周期会简单些。

  ![BeanFactory获取的bean的生命周期](http://otfc4cl9r.bkt.clouddn.com/BeanFactory%E8%8E%B7%E5%8F%96%E7%9A%84bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
