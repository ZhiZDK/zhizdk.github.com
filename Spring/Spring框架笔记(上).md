### Spring
#### Spring框架概述
1. Spring是轻量级的开源的JavaEE框架
2. Spring可以解决企业应用开发的复杂性
3. Spring有两个核心部分：IOC和Aop
    1. IOC：控制反转，把创建对象过程交给Spring进行管理
    2. Aop：面向切面，不修改源代码进行功能增强
4. Spring特点
    1. 方便解耦，简化开发
    2. Aop编程支持
    3. 方便程序测试
    4. 方便和其他框架进行整合
    5. 方便进行事务操作
    6. 降低API开发难度

##### IOC(接口)
1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
2. Spring提供IOC容器实现两种方式：（两个接口）
    1. BeanFactory：IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用
        * 加载配置文件的时候不会创建对象，在获取对象(使用)才去创建对象
    2. ApplicationContext：BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用
        * 加载配置文件的时候就会把在配置文件中的对象进行创建
3. ApplicationContext接口有实现类
    * FileSystemXmlApplicationContext（写文件在硬盘中的路径）
    * ClassPathXmlApplicationContext（写文件在项目中的路径/名称()在src中）

#### IOC操作Bean管理
1. **什么是Bean管理**
    * Bean管理指的是两个操作
        1. Spring创建对象
        2. Spring注入属性

2. Bean管理操作有两种方式
    1. 基于xml配置文件方式实现
        1. 在spring配置文件中，使用bean标签，标签里面添加对应属性就可以实现对象创建
        2. 在bean标签有很多属性，介绍常用的属性
            * id属性：唯一标识
            * class属性：类全路径（包类路径）
        3. 创建对象的时候，默认也是执行无参数构造方法完成对象创建
    2. 基于xml方式注入属性
        1. DI：依赖注入，就是注入属性
            * 第一种注入方式：使用set方法进行注入
                1. 配置对象创建
                2. set方法注入属性
                     * 使用property完成属性注入
                         * name：类里面属性名称
                         * value：向属性中注入的值
            * 第二种注入方式：使用有参数构造方法进行注入
                1. 创建类，定义属性，创建属性对应有参数构造方法
                2. 在spring配置文件中进行配置
                    1. 使用有参构造注入属性
                        * 使用标签<constructor-arg>进行配置

    3. IOC操作Bean管理（xml注入其他类型属性）
        1. 字面量
            1. null值
                * `<property name="address">
                    <null>
                    </property>`
            2. 属性值包含特殊符号
                1. 把<>进行转义&lt;&gt;
                2. 把特殊符号内容写到CDATA
                    * `<property name="address">
                        <value><![CDATA[<<南京>>]]>
                        </property>`
        2. 注入属性-外部bean
            1.  创建两个类service类和dao类
            2.  在service调用dao里面的方法
            3.  在spring配置文件中进行配置
                1.  创建UserDao类型属性，生成set方法
                2.  service和dao对象创建
                3.  注入userDao对象
                    * name属性：类里面属性名称
                    * ref属性：创建userDao对象bean标签id值

        3. 如何设置单实例还是多实例
            1. 在spring配置文件bean标签里面有属性(scope)用于设置单实例还是多实例
            2. scope属性值
                1. 第一个值，默认值，singleton，表示是单实例对象
                2. 第二个值，peototype，表示是多实例对象

            3. singleton和prototype区别
                1. 第一，singleton是单实例，prototype是多实例
                2. 第二，设置scope值是singleton的时候，加载spring配置文件的时候就会创建单实例对象，而当设置scope值是prototype的时候，不是在加载spring配置文件的时候创建对象，而是在调用getBean方法时创建多实例对象
        4. xml自动装配
            1. 什么是自动装配
                1. 根据指定装配规则（属性名称或者属性类型），Spring自动将匹配的属性值进行注入
            2. 实现自动装配
                * bean标签属性autowire，配置自动装配
                * autowire属性常用两个值：
                    * byName：根据属性名称注入，注入值bean的id值和类属性名称一样
                    * byType：根据属性类型注入
        5. 外部属性文件
            1. 直接配置数据库信息
                1. 配置德鲁伊连接池
                2. 引入德鲁伊连接池依赖jar包
            2. 引入外部属性文件配置数据库连接池
                1. 创建外部属性文件，properties格式文件，写数据库信息
                2. 把外部properties属性文件引入到spring配置文件中
                3. 在spring配置文件使用标签引入外部属性文件
    4. 基于注解方式实现
        1. 什么是注解
            1. 注解是代码特殊标记，格式：@注解名称(属性名称=属性值,属性名称=属性值..)
            2. 使用注解，注解作用在类上面，方法上面，属性上面
            3. 使用注解目的：简化xml配置
        2. Spring针对Bean管理中创建对象提供注解
            1. @Component
            2. @Service
            3. @Controller
            4. @Repository
            * 上面四个注解功能是一样的，都可以用来创建bean实例
        3. 基于注解方式实现对象创建
            1. 第一步：引入依赖
                * spring-aop-5.2.3.RELEASE.jar
            2. 第二步：开启组件扫描
                1. 如果扫描多个包，多个包使用逗号隔开
                2. 扫描包上层目录
            3. 创建类，在类上面添加创建对象注解
                1. 在注解里面value属性值可以省略不写
                2. 默认值就是类名称，首字母小写  UserService-->userService

    5. 基于注解方式实现属性注入
        1. @Autowired：根据属性类型进行自动装配
        2. @Qualifier：根据属性名称进行注入
        3. @Resource：可以根据类型注入，可以根据名称注入
        4. @Value：注入普通类型属性
