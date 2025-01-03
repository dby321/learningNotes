

# Mybatis笔记

[Mybatis中文网](https://mybatis.net.cn/)

[Mybatis-generator/通用Mapper/Mybatis-Plus对比](https://blog.csdn.net/m0_37524586/article/details/88351833)

[黑马-MyBatis教程](https://www.bilibili.com/video/BV1Db411s7F5/?from=search&seid=14757358402794430637&vd_source=f58f2e2144be4e99a8cf800afeecbbcb)

[Github-MyBatis 通用 Mapper4](https://github.com/abel533/Mapper)

[MyBatis-Plus](https://baomidou.com/) 

## Mybatis入门

1. 下载Mybatis的jar包

2. 业务需求

   1. 根据用户id查询一个用户
   2. 根据用户名称模糊查询用户列表、
   3. 添加用户
   4. 更新用户
   5. 删除用户

3. log4j.properties

   ```properties
   # Global logging configuration
   log4j.rootLogger=DEBUG, stdout
   # Console output...
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
   ```

4. SqlMapConfig.xml(和spring整合后，<environments>消失)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 和spring整合后 environments配置将废除 -->
	<environments default="development">
		<environment id="development">
			<!-- 使用jdbc事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
    <mappers>
        <mapper resource="sqlmap/User.xml"></mapper>
    </mappers>
    
</configuration>
```

5. User.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，后面会讲 -->
<mapper namespace="test">
    <!-- id:statement的id 或者叫做sql的id-->
	<!-- parameterType:声明输入参数的类型 -->
	<!-- resultType:声明输出结果的类型，应该填写pojo的全路径 -->
	<!-- #{}：输入参数的占位符，相当于jdbc的？ -->
	<select id="queryUserById" parameterType="Integer"
		resultType="cn.itcast.mybatis.pojo.User">
		SELECT * FROM `user` WHERE id  = #{id}
	</select>
    <select id="queryUserByUsername1" parameterType="String"
		resultType="cn.itcast.mybatis.pojo.User">
		SELECT * FROM `user` WHERE username LIKE #{username}
	</select>
    
</mapper>
```

6. 测试程序

```java
public class MybatisTest {
	private SqlSessionFactory sqlSessionFactory = null;

	@Before
	public void init() throws Exception {
		// 1. 创建SqlSessionFactoryBuilder对象
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();

		// 2. 加载SqlMapConfig.xml配置文件
		InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");

		// 3. 创建SqlSessionFactory对象
		this.sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
	}

	@Test
	public void testQueryUserById() throws Exception {
		// 4. 创建SqlSession对象
		SqlSession sqlSession = sqlSessionFactory.openSession();

		// 5. 执行SqlSession对象执行查询，获取结果User
		// 第一个参数是User.xml的statement的id，第二个参数是执行sql需要的参数；
		Object user = sqlSession.selectOne("queryUserById", 1);

		// 6. 打印结果
		System.out.println(user);

		// 7. 释放资源
		sqlSession.close();
	}
}
```

### Mybatis解决jdbc编程的问题

1. 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库连接池可解决此问题。

解决：在SqlMapConfig.xml中配置数据连接池，使用连接池管理数据库链接。

2. Sql语句写在代码中造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码。

解决：将Sql语句配置在XXXmapper.xml文件中与java代码分离。

3. 向sql语句传参数麻烦，因为sql语句的where条件不一定，可能多也可能少，占位符需要和参数一一对应。

解决：Mybatis自动将java对象映射至sql语句，通过statement中的parameterType定义输入参数的类型。

4. 对结果集解析麻烦，sql变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成pojo对象解析比较方便。

解决：Mybatis自动将sql执行结果映射至java对象，通过statement中的resultType定义输出结果的类型。

### Mybatis和Hibernate的区别

Mybatis和hibernate不同，它不完全是一个ORM（对象关系映射）框架，**因为MyBatis需要程序员自己编写Sql语句**。mybatis可以通过XML或注解方式灵活配置要运行的sql语句，并将java对象和sql语句映射生成最终执行的sql，最后将sql执行的结果再映射生成java对象。

 

Mybatis学习门槛低，简单易学，程序员直接编写原生态sql，可严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，例如互联网软件、企业运营类软件等，因为这类软件需求变化频繁，一但需求变化要求成果输出迅速。但是灵活的前提是**mybatis无法做到数据库无关性**，如果需要实现支持多种数据库的软件则需要自定义多套sql映射文件，工作量大。

 

Hibernate对象/关系映射能力强，**数据库无关性好**，对于关系模型要求高的软件（例如需求固定的定制化软件）如果用hibernate开发可以节省很多代码，提高效率。但是Hibernate的学习门槛高，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡，以及怎样用好Hibernate需要具有很强的经验和能力才行。

总之，按照用户的需求在有限的资源环境下只要能做出维护性、扩展性良好的软件架构都是好架构，所以框架只有适合才是最好。 

## Mapper开发

1. Mapper.xml文件中的**namespace**与mapper接口的**类路径相同**。

2. Mapper**接口方法名**和Mapper.xml中定义的每个statement的**id相同** 

3. Mapper接口方法的**输入参数类型**和mapper.xml中定义的每个sql 的**parameterType的类型相同**

4. Mapper接口方法的**输出参数类型**和mapper.xml中定义的每个sql的**resultType的类型相同**

### sqlMapConfig.xml

[Mybatis中文网-配置](https://mybatis.net.cn/configuration.html)

### XxxMapper.xml

[Mybatis中文网-XML映射器](https://mybatis.net.cn/sqlmap-xml.html)

[Mybatis中文网-动态 SQL](https://mybatis.net.cn/dynamic-sql.html)

#### 手动映射`<resultMap>`

OrderMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，Mapper动态代理开发的时候使用，需要指定Mapper的类路径 -->
<mapper namespace="cn.itcast.mybatis.mapper.OrderMapper">

	<!-- resultMap最终还是要将结果映射到pojo上，type就是指定映射到哪一个pojo -->
	<!-- id：设置ResultMap的id -->
	<resultMap type="Orders" id="orderResultMap">
		<!-- 定义主键 ,非常重要。如果是多个字段,则定义多个id -->
		<!-- property：主键在pojo中的属性名 -->
		<!-- column：主键在数据库中的列名 -->
		<id property="id" column="id" />

		<!-- 定义普通属性 -->
		<result property="userId" column="user_id" />
		<result property="number" column="number" />
		<result property="createtime" column="createtime" />
		<result property="note" column="note" />
	</resultMap>

	<!-- 查询所有的订单数据 -->
	<select id="queryOrderAll" resultMap="orderResultMap">
		SELECT id, user_id,
		number,
		createtime, note FROM `order`
	</select>

</mapper>
```

##### 一对一`<association>`

```xml
<resultMap type="order" id="orderUserResultMap">
	<id property="id" column="id" />
	<result property="userId" column="user_id" />
	<result property="number" column="number" />
	<result property="createtime" column="createtime" />
	<result property="note" column="note" />

	<!-- association ：配置一对一属性 -->
	<!-- property:order里面的User属性名 -->
	<!-- javaType:属性类型 -->
	<association property="user" javaType="User">
		<!-- id:声明主键，表示user_id是关联查询对象的唯一标识-->
		<id property="id" column="user_id" />
		<result property="username" column="username" />
		<result property="address" column="address" />
	</association>

</resultMap>

<!-- 一对一关联，查询订单，订单内部包含用户属性 -->
<select id="queryOrderUserResultMap" resultMap="orderUserResultMap">
	SELECT
	o.id,
	o.user_id,
	o.number,
	o.createtime,
	o.note,
	u.username,
	u.address
	FROM
	`order` o
	LEFT JOIN `user` u ON o.user_id = u.id
</select>
```

##### 一对多`<collection>`

> 一对一和多对多查询时，记得关联两张表

```xml
<resultMap type="user" id="userOrderResultMap">
	<id property="id" column="id" />
	<result property="username" column="username" />
	<result property="birthday" column="birthday" />
	<result property="sex" column="sex" />
	<result property="address" column="address" />

	<!-- 配置一对多的关系 -->
	<collection property="orders" javaType="list" ofType="order">
		<!-- 配置主键，是关联Order的唯一标识 -->
		<id property="id" column="oid" />
		<result property="number" column="number" />
		<result property="createtime" column="createtime" />
		<result property="note" column="note" />
	</collection>
</resultMap>

<!-- 一对多关联，查询订单同时查询该用户下的订单 -->
<select id="queryUserOrder" resultMap="userOrderResultMap">
	SELECT
	u.id,
	u.username,
	u.birthday,
	u.sex,
	u.address,
	o.id oid,
	o.number,
	o.createtime,
	o.note
	FROM
	`user` u
	LEFT JOIN `order` o ON u.id = o.user_id
</select>
```

##### 多对多

> 必须要加入中间表，其实写映射的时候只能写一对多。
>
> 例如：角色和用户是多对多，每次查询left join只能写一对多，即一个角色的所有用户，或者一个用户的所有角色。

## 输入映射和输出映射

### 传递简单类型

使用`#{}`占位符，或者`${}`进行sql拼接。

### 传递pojo对象

Mybatis使用ognl表达式解析对象字段的值，`#{}`或者`${}`括号中的值为pojo属性名称。

### 传递pojo包装对象

- QueryVo

```java
public class QueryVo {
	// 包含其他的pojo
	private User user;

	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
}
```



- Sql语句

![1577858257312](.\images\1577858257312.png)



## PageHelper

```xml
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="typeAliasesPackage" value="com.binyu.domain"></property>
       
        <!--配置PageHelper-->
        <property name="plugins">
            <array>
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">mysql</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>
```



## Mybatis的注解开发

注解	说明
@Insert	实现新增
@Delete	实现删除
@Update	实现更新
@Select	实现查询
@Result	实现结果集封装
@Results	可以与@Result 一起使用，封装多个结果集
@ResultMap	实现引用@Results 定义的封装
@One	实现一对一结果集封装
@Many	实现一对多结果集封装
@SelectProvider	实现动态 SQL 映射
@CacheNamespace	实现注解二级缓存的使用
