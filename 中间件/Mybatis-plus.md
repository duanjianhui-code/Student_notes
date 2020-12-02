# Mybatis-plus

> [MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生

## 开始使用：

1.新建数据库文件

```sql
CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
```

其对应的数据库 Data 脚本如下：

```sql
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

3、编写项目，初始化项目！使用SpringBoot初始化！

 4、导入依赖

```properties
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
</dependency>
```

说明：我们使用 mybatis-plus 可以节省我们大量的代码，尽量不要同时导入 mybatis 和 mybatisplus！版本的差异！

5、连接数据库！这一步和 mybatis 相同！

```properties
# mysql 5 驱动不同 com.mysql.jdbc.Driver
# mysql 8 驱动不同com.mysql.cj.jdbc.Driver、需要增加时区的配置
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis-plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

6、传统方式pojo-dao（连接mybatis，配置mapper.xml文件）-service-controller 

6、使用了mybatis-plus 之后

- 创建pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

- mapper接口

  ```java
  // 在对应的Mapper上面继承基本的类 BaseMapper
  @Repository // 代表持久层
  public interface UserMapper extends BaseMapper<User> {
      // 所有的CRUD操作都已经编写完成了
      // 你不需要像以前的配置一大堆文件了！
  }
  ```

- 注意点，我们需要在主启动类上去扫描我们的mapper包下的所有接口

  ```java
  @MapperScan("com.mybatisplus.mapper")
  @SpringBootApplication
  ```

- 测试类中测试

  ```java
  class MybatisPlusApplicationTests {
      // 继承了BaseMapper，所有的方法都来自己父类
      // 我们也可以编写自己的扩展方法！
      @Autowired
      UserMapper userMapper;
  
      @Test
      void contextLoads() {
          // 参数是一个 Wrapper ，条件构造器，这里我们先不用 null
          // 查询全部用户
          List<User> users = userMapper.selectList(null);
          users.forEach(System.out::println);
      }
  
  }
  ```

  

- 结果

  ![image-20201202205553049](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202205553049.png)



## 配置日志

我们所有的sql现在是不可见的，我们希望知道它是怎么执行的，所以我们必须要看日志！

```properties
#配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![image-20201202210300228](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202210300228.png)



## CRUD扩展

### 插入

```java
//测试插入
    @Test
    void insertTest() {
        User user = new User();
        user.setName("段建辉");
        user.setAge(23);
        user.setEmail("126591321@qq.com");
        userMapper.insert(user);
    }
```

结果：

![image-20201202211923456](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202211923456.png)



### 主键生成策略

>默认 ID_WORKER 全局唯一id

雪花算法： snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为 毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味 着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯 一！

> 主键自增

我们需要配置主键自增：

 1、实体类字段上 [@TableId(type = IdType.AUTO)]() 

2、数据库字段一定要是自增



### 更新

```java
 //测试更新
    @Test
    void updateTest(){
        User user=new User();
        user.setId(1l);
        user.setName("段建辉");
        user.setAge(3);
        user.setEmail("126591321@qq.com");
        userMapper.updateById(user);
    }
```

![image-20201202213356963](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202213356963.png)



### 自动填充

 创建时间、修改时间！这些个操作一遍都是自动化完成的，我们不希望手动更新！ 阿里巴巴开发手册：所有的数据库表：gmt_create、gmt_modified几乎所有的表都要配置上！而且需 要自动化！

> 方式一：数据库级别（工作中不允许你修改数据库）

1、在表中新增字段 create_time, update_time

![image-20201202214411320](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202214411320.png)

2、再次测试插入方法，我们需要先把实体类同步！

```java
private Date createTime;
private Date updateTime;
```

注意：在日常开发中，不允许对数据库直接进行修改，建议使用代码

> 方式二：代码级别

1、删除数据库的默认值、更新操作！

![image-20201202215140178](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202215140178.png)

2、实体类字段属性上需要增加注解

```java
 //字段填充内容
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.UPDATE)
    private Date updateTime;
```

3、编写处理器来处理这个注解即可！

```java
@Slf4j
@Component // 一定不要忘记把处理器加到IOC容器中！
public class MyMetaObjectHandle implements MetaObjectHandler {
    //插入时的填充策略
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("Star........");
        //default MetaObjectHandler setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject) 
        this.setFieldValByName("create_time",new Date(),metaObject);
        this.setFieldValByName("update_time",new Date(),metaObject);
    }
    //更新时的填充策略
    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("update_time",new Date(),metaObject);
    }
}
```

结果：

![image-20201202220746663](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202220746663.png)



### 乐观锁

>乐观锁 : 故名思意十分乐观，它总是认为不会出现问题，无论干什么不去上锁！如果出现了问题， 再次更新值测试 悲观锁：故名思意十分悲观，它总是认为总是出现问题，无论干什么都会上锁！再去操作！

我们这里主要讲解 乐观锁机制！ 乐观锁实现方式： 

- 取出记录时，获取当前 version 
- 更新时，带上这个version 
- 执行更新时， set version = newVersion where version =oldVersion
- 如果version不对，就更新失败

```sql
乐观锁：1、先查询，获得版本号 version = 1
-- A
update user set name = "kuangshen", version = version + 1
where id = 2 and version = 1
-- B 线程抢先完成，这个时候 version = 2，会导致 A 修改失败！
update user set name = "kuangshen", version = version + 1
where id = 2 and version = 1
```

>测试一下MP的乐观锁插件

1、给数据库中增加version字段！

![image-20201202223628667](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202223628667.png)

2、我们实体类加对应的字段

```java
@Version //乐观锁Version注解
private Integer version;
```

3、注册组件

```java
@MapperScan("com.mybatisplus.mapper")
@Configuration
@EnableTransactionManagement

public class MybatisPlusConfig {
    // 注册乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

4、测试

```java
  // 测试乐观锁成功！
    @Test
    public void testOptimisticLocker() {
        // 1、查询用户信息
        User user = userMapper.selectById(1L);
        // 2、修改用户信息
        user.setName("kuangshen");
        user.setEmail("24736743@qq.com");
        // 3、执行更新操作
        userMapper.updateById(user);
    }
```

![image-20201202224914089](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202224914089.png)

```java
// 测试乐观锁失败！多线程下
    @Test
    public void testOptimisticLocker2(){
        // 线程 1
        User user = userMapper.selectById(1L);
        user.setName("kuangshen111");
        user.setEmail("24736743@qq.com");
        // 模拟另外一个线程执行了插队操作
        User user2 = userMapper.selectById(1L);
        user2.setName("kuangshen222");
        user2.setEmail("24736743@qq.com");
        userMapper.updateById(user2);
        // 自旋锁来多次尝试提交！
        userMapper.updateById(user); // 如果没有乐观锁就会覆盖插队线程的值！
    }
```

![image-20201202225209624](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202225209624.png)



### 查询

> 单个查询

```java
// 测试查询
@Test
public void testSelectById(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```

![image-20201202225546599](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202225546599.png)

> 查询批量

```java
  // 测试批量查询！
    @Test
    public void testSelectByBatchId(){
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
        users.forEach(System.out::println);
    }
```

![image-20201202225836782](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202225836782.png)

> 条件查询

```java
 // 按条件查询之一使用map操作
    @Test
    public void testSelectByBatchIds(){
        Map<String, Object> map = new HashMap<>();
        //自定义查询条件
        map.put("id",1);
        List<User> users = userMapper.selectByMap(map);
        users.forEach(System.out::println);
    }
```

![image-20201202230410696](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202230410696.png)



### 分页

1、原始的 limit 进行分页

 2、pageHelper 第三方插件 

3、MP 其实也内置了分页插件！

> 开始使用

1、配置拦截器组件即可

```java
  // 分页插件
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
```

2、直接使用Page对象即可！

```java
 // 测试分页查询
    @Test
    public void testPage(){
        // 参数一：当前页
        // 参数二：页面大小
        // 使用了分页插件之后，所有的分页操作也变得简单的！
        Page<User> page = new Page<>(2,5);
        userMapper.selectPage(page,null);
        page.getRecords().forEach(System.out::println);
        System.out.println(page.getTotal());
    }
```

![image-20201202232134148](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202232134148.png)



### 删除操作

1、根据 id 删除记录

```java
// 测试删除
    @Test
    public void testDeleteById() {
        userMapper.deleteById(1334124845154004993l);
    }
```

![image-20201202233114678](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202233114678.png)

2、通过id批量删除

```java
// 通过id批量删除
    @Test
    public void testDeleteBatchId() {
        userMapper.deleteBatchIds(Arrays.asList(1334124845154004994l, 1334124845154004995l));
    }
```

![image-20201202233423038](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202233423038.png)

3、通过map删除

```java
 // 通过map删除
    @Test
    public void testDeleteMap() {
        HashMap<String, Object> map = new HashMap<>();
        map.put("id", 1334124845154004996l);
        userMapper.deleteByMap(map);
    }
```

![image-20201202233503371](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202233503371.png)



### 逻辑删除

> 物理删除 ：从数据库中直接移除 
>
> 逻辑删除 ：再数据库中没有被移除，而是通过一个变量来让他失效！ deleted = 0 => deleted = 1

1、在数据表中增加一个 deleted 字段

2、实体类中增加属性

```java
 @TableLogic//逻辑删除
 private Integer deletedl;
```

3、配置！

```java
 // 逻辑删除组件！
    @Bean
    public ISqlInjector sqlInjector() {
        return new LogicSqlInjector();
    }
```

```properties
#配置逻辑删除
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

4、测试一下删除！

![image-20201202235654186](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202235654186.png)

![image-20201202235800466](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201202235800466.png)



### 性能分析插件

我们在平时的开发中，会遇到一些慢sql。测试！ druid

作用：性能分析拦截器，用于输出每条 SQL 语句及其执行时间

 MP也提供性能分析插件，如果超过这个时间就停止运行！

 1、导入插件

```java
     /**
     * SQL执行效率插件
     */
    @Bean
    @Profile({"dev", "test"})// 设置 dev test 环境开启，保证我们的效率
    public PerformanceInterceptor performanceInterceptor() {
        PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
        performanceInterceptor.setMaxTime(100); // ms设置sql执行的最大时间，如果超过了则不执行
        performanceInterceptor.setFormat(true); // 是否格式化代码
        return performanceInterceptor;
    }
```

记住，要在SpringBoot中配置环境为dev或者 test 环境！

```properties
#设置为开发环境
spring.profiles.active=dev
```

2、测试使用！

```java
 @Test
    void contextLoads() {
        // 参数是一个 Wrapper ，条件构造器，这里我们先不用 null
        // 查询全部用户
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }
```

![image-20201203001016901](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203001016901.png)



### 条件构造器

我们写一些复杂的sql就可以使用它来替代

官方：[条件构造器 | MyBatis-Plus (baomidou.com)](https://baomidou.com/guide/wrapper.html#abstractwrapper)

1、测试一，记住查看输出的SQL进行分析

```java
@Test
    void contextLoads() {
        // 查询name不为空的用户，并且邮箱不为空的用户，年龄大于等于12
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.isNotNull("name")
                .isNotNull("email")
                .ge("age",12);
        userMapper.selectList(wrapper).forEach(System.out::println);

    }
```

![image-20201203002529171](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203002529171.png)

2、测试二，记住查看输出的SQL进行分析

```java
@Test
    void test2() {
        // 查询名字段建辉
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("name", "段建辉");
        User user = userMapper.selectOne(wrapper);// 查询一个数据，出现多个结果使用List或者 Map
        System.out.println(user);
    }
```

![image-20201203002918328](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203002918328.png)

3、测试三，记住查看输出的SQL进行分析

```java
@Test
    void test3() {
        // 查询年龄在 20 ~ 30 岁之间的用户
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.between("age", 20, 30); // 区间
        Integer count = userMapper.selectCount(wrapper);// 查询结果数
        System.out.println(count);
    }
```

![image-20201203003317455](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203003317455.png)

4、测试四，记住查看输出的SQL进行分析

```java
 // 模糊查询
    @Test
    void test4() {
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        // 左和右 t%
        wrapper
                .notLike("name", "e")
                .likeRight("email", "t");
        List<Map<String, Object>> maps = userMapper.selectMaps(wrapper);
        maps.forEach(System.out::println);
    }
```

![image-20201203003642131](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203003642131.png)

5、测试五（）

```java
 @Test
    void test5() {
        // 子查询
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.inSql("id","Select id from user where id <3");
        List<Object> objects = userMapper.selectObjs(wrapper);
        objects.forEach(System.out::println);
    }
```

![image-20201203004155841](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203004155841.png)

6、测试六

```java
@Test
    void test6() {
        // 通过id进行排序
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.orderByAsc("id");
        List<User> users = userMapper.selectList(wrapper);
        users.forEach(System.out::println);
    }
```

![image-20201203004618210](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201203004618210.png)



### 代码自动生成器

dao、pojo、service、controller都给我自己去编写完成！

 AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、 Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

源码：

```java
package com.mybatisplus;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.po.TableFill;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;


import java.util.ArrayList;

/**
 * @author Duanjianhui
 * @create 2020-12-03 0:52
 */

public class DuanCode {
    public static void main(String[] args) {
        // 需要构建一个 代码自动生成器 对象
        AutoGenerator mpg = new AutoGenerator();
        // 配置策略
        // 1、全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("段建辉");
        gc.setOpen(false);
        gc.setFileOverride(false); // 是否覆盖
        gc.setServiceName("%sService"); // 去Service的I前缀
        gc.setIdType(IdType.ID_WORKER);
        gc.setDateType(DateType.ONLY_DATE);
        gc.setSwagger2(true);
        mpg.setGlobalConfig(gc);
        //2、设置数据源
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/mybatis-plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);
        //3、包的配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("blog");
        pc.setParent("com.mybatisplus");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setController("controller");
        mpg.setPackageInfo(pc);
        //4、策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setInclude("user"); // 设置要映射的表名
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true); // 自动lombok；
        strategy.setLogicDeleteFieldName("deleted");
        // 自动填充配置
        TableFill gmtCreate = new TableFill("create_time", FieldFill.INSERT);
        TableFill gmtModified = new TableFill("update_time", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(gmtCreate);
        tableFills.add(gmtModified);
        strategy.setTableFillList(tableFills);
        // 乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true); //localhost:8080/hello_id_2
        mpg.setStrategy(strategy);
        mpg.execute(); //执行
    }
}

```

