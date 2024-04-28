## MyBatis

### 配置文件

- **src/main/resources/application.properties**

```properties
mybatis.type-aliases-package=com.test.springtest.entity
mybatis.mapper-locations=classpath:com/test/springtest/mapper/*.xml
mybatis.config-location=classpath:mybatis/mybatis-config.xml
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/my_project?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=000000
```

### `mybatis-config.xml`

```xml
<configuration>
	<typeAliases>
		<typeAlias alias="Integer" type="java.lang.Integer" />
		<typeAlias alias="Long" type="java.lang.Long" />
		<typeAlias alias="HashMap" type="java.util.HashMap" />
		<typeAlias alias="LinkedHashMap" type="java.util.LinkedHashMap" />
		<typeAlias alias="ArrayList" type="java.util.ArrayList" />
		<typeAlias alias="LinkedList" type="java.util.LinkedList" />
	</typeAliases>
</configuration>
```

### XML映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.springtest.mapper.WordMapper">
	<sql id="id"> <!-- 定义可重用的 SQL 代码段 -->
		id, name
	</sql>
    <select>
        SELECT 
        	<include refid="id" /> <!-- sql 标签使用 -->
        FROM 表名
    </select>
    <trim 
       prefix="前缀"
       suffix="后缀"
       prefixOverrides="前缀判断的条件"
       suffixOverrides="后缀判断的条件"> <!-- 自定义去除关键字 -->
    </trim>
</mapper>
```

#### \<select>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.springtest.mapper.WordMapper">
	<select id="id" resultType="">
        SELECT * FROM 表名
        <if test="value != null"> <!-- if判断 -->
			WHERE name = #{value} 
		</if>
        <choose> <!-- switch语句 -->
    		<when test="title != null"> <!-- case语 -->
    			AND title L #{title}
    		</when>
    		<when test="value != null">
    			AND value = #{value}
    		</when>
    		<otherwise> <!-- default语句 -->
    			AND value = 1
    		</otherwise>
  		</choose>
    </select>
    <select id="id" resultType="">
        SELECT * FROM 表名
        <where> 
    		<if test="title != null">
    			title = #{title}
    		</if> 
    		<if test="value != null">
    		    AND value like #{value}
    		</if>
  		</where>
    </select>
</mapper>
```

#### 其他标签

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.springtest.mapper.WordMapper">
	<insert id="id" resultType="">
        INSERT INTO 表名 (id，name) VALUES (#{id}，#{name})
    </insert>
    <insert id="id" useGeneratedKeys="true" keyProperty="id" parameterType="Article">
        <!-- useGeneratedKeys: 返回添加数据后的主键值 -->
        <!-- keyProperty: 返回添加数据后的主键名 -->
		INSERT INTO 表名 (name) VALUES (#{name})
	</insert>
    <update id="id" resultType="">
        UPDATE 表名 SET id=#{id} <where></where>
    </update>
    <delete id="id" resultType="">
        DELETE FROM 表名 <where></where>
    </delete>
</mapper>
```

| \<select>属性 | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| id            | 唯一的标识符                                                 |
| parameterType | 传入语句的参数类的完全限定名或别名, 默认值为 unset           |
| resultType    | 语句中返回的期望类型的类的完全限定名或别名                   |
| resultMap     | 外部 resultMap 的命名引用                                    |
| flushCache    | 任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值: false |
| useCache      | 将会导致本条语句的结果被二级缓存，默认值: 对 select 元素为 true |
| timeout       | 抛出异常之前，驱动程序等待数据库返回请求结果的秒数 默认值为 unset |

| \<insert>，\<update>, \<delete> 属性 | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| id                                   | 唯一标识符                                                   |
| parameterType                        | 传入语句的参数的完全限定类名或别名                           |
| flushCache                           | 任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：true |
| timeout                              | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset |