# Spring理解

## （1）为什么需要依赖注入

* ILogger
    * FileLogger
    * ConsoleLogger
    * ServerLogger
…

对项目添加日志打印功能

* Version1: 在支付、登录、鉴权、退款、退货接口的代码加入日志打印
    ```java
    public class PaymentAction {
        private ILogger logger = new FileLogger();

        public void pay(BigDecimal payValue) {
            logger.log("pay begin, payValue is " + payValue);

            // do otherthing
            // ...

            logger.log("pay end");
        }
    }
    ```  
    
改需求了，要把日志打印到一台日志服务器上ServerLogger

* Version2: 把logger对象的创建交给外部去做
    ```java
    public class PaymentAction {
        private ILogger logger = LoggerFactory.createLogger();

        public void pay(BigDecimal payValue) {
            logger.log("pay begin, payValue is " + payValue);

            // do otherthing
            // ...

            logger.log("pay end");
        }
    }
    
    public class LoggerFactory {
        public static ILogger createLogger() {
            return new ServerLogger();
        }
    }
    ```
    
控制反转(Inverse of Control)，依赖注入(Dependency Injection)

* Version3: 不再在类内部创建logger对象，同时给PaymentAction添加了一个构造函数，方便Spring进行注入
    ```java
    public class PaymentAction {
        private ILogger logger;

        public PaymentAction(ILogger logger) {
            super();
            this.logger = logger;
        }

        public void pay(BigDecimal payValue) {
            logger.log("pay begin, payValue is " + payValue);

            // do otherthing
            // ...

            logger.log("pay end");
        }
    }
    ```
    
    创建payment.xml,引入必要的XSD文件;并且配置了两个bean对象,使用了<constructor-arg>标签,指定了ServerLogger作为PaymentAction构造函数的入参:  
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

      <bean id="paymentAction" class="com.springnovel.paymentwithspringxml.PaymentAction">
        <constructor-arg ref="serverLogger" />
      </bean>

      <bean id="serverLogger" class="com.springnovel.perfectlogger.ServerLogger" />

    </beans>
    ```
    
    差不多了，现在测试一下:
    
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext("payment.xml");
    PaymentAction paymentAction = (PaymentAction) context.getBean("paymentAction");
    paymentAction.pay(new BigDecimal(2));
    ```
    
    > output:  
    ServerLogger: pay begin, payValue is 2  
    ServerLogger: pay end
    
    ServerLogger对象已经被注入到PaymentAction中了。
    使用Spring实现了工厂类的功能,修复了之前代码**耦合性过高**的问题。
    
* 为什么要使用依赖注入
    * 传统的代码,每个对象负责管理与自己需要依赖的对象,导致如果需要切换依赖对象的实现类时,需要修改多处地方.
    * 同时,过度耦合也使得对象**难以进行单元测试**.
    * 依赖注入把对象的创造交给外部去管理,很好的解决了代码紧耦合(tight couple)的问题,是一种让代码实现松耦合(loose couple)的机制.松耦合让代码**更具灵活性,能更好地应对需求变动,以及方便单元测试**.

* 为什么要使用Spring
    * 使用Spring框架主要是为了简化Java开发(大多数框架都是为了简化开发),它帮我们封装好了很多完善的功能,而且Spring的生态圈也非常庞大.
    * 基于XML的配置是Spring提供的**最原始的依赖注入配置方式**,从Spring诞生之时就有了,功能也是最完善的(但是貌似有更好的配置方法,明天看看！).