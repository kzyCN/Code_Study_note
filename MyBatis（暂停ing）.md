# MyBatis

[官方文档（支持中文）](https://mybatis.org/mybatis-3/zh/index.html)

## 环境配置（基于Maven）

配置jar包

```XML
<!-- junity依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

<!-- mybatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.5</version>
        </dependency>
<!-- mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
```

在`src/main/rescources`处配置**全局配置文件**`mybatis-config.xml`

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--mybatis全局配置文件-->
<configuration>
    <environments default="development">

        <environment id="development">

            <transactionManager type="JDBC"/>

            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

<!--    加载映射文件-->
    <mappers>
        <mapper resource="HouseMapper.xml"/>
    </mappers>
</configuration>
```

![image-20200810123653787](https://gitee.com/kzycn/picCloud/raw/master/20200810123700.png)

### 加载全局配置文件

```JAVA
public class SqlSessionFactoryTest {

    @Test
    public void test() throws IOException {

        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new     SqlSessionFactoryBuilder().build(in);

        //3.获取会话

        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4.通过sql会话，向数据库发起增删改查的请求
//        House house = sqlSession.selectOne("HouseMapper.getbyId",1);
//        System.out.println(house);


        //5.关闭会话
        sqlSession.close();

    }
```

### House类

> 尽量使用包装类

```JAVA
public class House {

    private Integer id;
    private String title;
    private String logo;
    private Double price;
    private Double area;
    private String city;
    private String district;
    private Date createTime;
    private Date updateTime;
    
    ....
    //geter and setter
    //to String
}
```

### 编写映射文件

在`src/main/resource`目录下，创建`HouseMapper.xml`

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace命名空间，作用就是对sql进行分类化管理，理解sql隔离-->
<mapper namespace="com.imcode.mybatis.mapper.HouseMapper"> 
    <!--查询所有的酒店信息-->
<!--
1.id属性： 标识映射文件中的sql，将sql语句封装到mapped statement对象中，所以称为 statement的id 

2.parameterType：指定输入参数类型 

3.#{}：表示一个占位符 

4.#{id}：其中的id表示接收的输入参数，如果输入参数是简单类型 

5.#{}中的参数名可以任意，可以为value或者其他 

6.resultType：指定sql输出结果的所映射的java对象类型， select指定resultType表示将单条记录映射成的java对象
-->
<select id="getById" parameterType="java.lang.Integer" resultType="com.imcode.mybatis.model.House"> 
    </select> 
</mapper>
```

### 在全局中加载映射文件

![image-20200810124641736](https://gitee.com/kzycn/picCloud/raw/master/20200810124641.png)

### 接口

```JAVA
/**
 * mapper 接口的全限定名要和 mapper 映射文件的 namespace 值一致。
 * mapper 接口的方法名称要和 mapper 映射文件的 statement 的 id 一致。
 * mapper 接口的方法参数类型要和 mapper 映射文件的 statement 的 parameterType 的值一
 * 致。
 * mapper 接口的方法返回值类型要和 mapper 映射文件的 statement 的 resultType 的值一
 * 致。
 */
public interface HouseMapper {

    House  getbyId(Integer id);

    /**
     * 对象作为输入条件 通过房间名称和小区名称模糊查询
     *多个参数需要在参数前面加上@Param注解
     * @return
     */
    List<House> findByTitleAndDistrict1(@Param("title") String title, @Param("district") String district);

    List<House> findByTitleAndDistrict2(House house);

    /**
     *根据id删除房源
     * @param id
     */
    void deleteById(Integer id);

    /**
     * 修改房屋信息
     */
    void update(House houseUpdate);

}
```

### 在HouseMapper.xml注册

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--命名空间，用于分类,对应dao全路径-->
<mapper namespace="top.floatlife.mapper.HouseMapper">

<select id="getbyId" parameterType="java.lang.Integer" resultType="top.floatlife.mybatis.House">
    SELECT
    id,
    title,
    logo,
    price,
    area,
    city,
    district,
    create_time createTime,
    update_time updateTime
     FROM house WHERE id = #{id}
  </select>

<select id="findByTitleAndDistrict1" resultType="top.floatlife.mybatis.House">
    SELECT
    id,
    title,
    logo,
    price,
    area,
    city,
    district,
    create_time createTime,
    update_time updateTime
    FROM house WHERE title LIKE CONCAT('%',#{title},'%') AND district LIKE CONCAT('%',#{district},'%')
</select>

<!--    对象类型映射-->
    <select id="findByTitleAndDistrict2"
            resultType="top.floatlife.mybatis.House"
            parameterType="top.floatlife.mybatis.House">
    SELECT
    id,
    title,
    logo,
    price,
    area,
    city,
    district,
    create_time createTime,
    update_time updateTime
    FROM house WHERE title LIKE CONCAT('%',#{title},'%') AND district LIKE CONCAT('%',#{district},'%')
</select>



<delete id="deleteById" parameterType="java.lang.Integer">
        DELETE FROM house WHERE id = #{id}
    </delete>

    <update id="update" parameterType="top.floatlife.mybatis.House" >
        UPDATE house
        SET create_time = #{createTime},update_time = #{updateTime}
        WHERE id=#{id}
    </update>
</mapper>
```

### 测试类

```JAVA
public class SqlSessionFactoryTest {

    @Test
    //基础配置
    public void test() throws IOException {

        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);

        //3.获取会话

        SqlSession sqlSession = sqlSessionFactory.openSession();

        //4.通过sql会话，向数据库发起增删改查的请求
//        House house = sqlSession.selectOne("HouseMapper.getbyId",1);
//        System.out.println(house);

        //mybatis框架使用反射技术，在程序的运行期，通过jdk的动态代理机制，帮我们生成了HouseMapper接口的实现类（代理类）
        HouseMapper houseMapper =
                sqlSession.getMapper(HouseMapper.class);
        House house = houseMapper.getbyId(1);
        System.out.println(house);

        //5.关闭会话
        sqlSession.close();

    }

    @Test
    //多条件
    public void findByTitleAndDistrict1() throws IOException {

        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);

        //3.获取会话
        SqlSession sqlSession = sqlSessionFactory.openSession();

        HouseMapper houseMapper =
                sqlSession.getMapper(HouseMapper.class);
        List<House> list = houseMapper.findByTitleAndDistrict1("办公", "东风路");

        //增强for循环，遍历
        for (House house : list) {
            System.out.println(house);
        }

        //5.关闭会话
        sqlSession.close();

    }

    @Test
    //对象映射
    public void findByTitleAndDistrict2() throws IOException {
        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);

        //3.获取会话
        SqlSession sqlSession = sqlSessionFactory.openSession();

        HouseMapper houseMapper = sqlSession.getMapper(HouseMapper.class);

        //通过创建House对象来构造查询条件
        House hous = new House();

        hous.setTitle("办公");
        hous.setDistrict("东风路");
        List<House> list = houseMapper.findByTitleAndDistrict2(hous);

        //增强for循环，遍历
        for (House house : list) {
            System.out.println(house);
        }

        //5.关闭会话
        sqlSession.close();
    }

    @Test
    //修改
    public void deleteById() throws IOException {
        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);

        //3.获取会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
        try {
            HouseMapper houseMapper = sqlSession.getMapper(HouseMapper.class);
            houseMapper.deleteById(10);
            // 提交事务
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
            //回滚
            sqlSession.rollback();
        } finally {
            sqlSession.close();
        }
    }

    @Test
    //修改信息
    public void update() throws IOException {
        //1.读取mybatis的配置文件
        String s = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(s);

        //2.构建SQL会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);

        //3.获取会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
        try {
            HouseMapper houseMapper = sqlSession.getMapper(HouseMapper.class);

            House house = houseMapper.getbyId(7);

            house.setCreateTime(new Date());
            house.setUpdateTime(new Date());

            houseMapper.update(house);

            // 提交事务
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
            //回滚
            sqlSession.rollback();
        } finally {
            sqlSession.close();
        }
    }
}
```

