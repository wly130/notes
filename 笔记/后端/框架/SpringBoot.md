## SpringBoot

### 项目结构

<img src="E:\GitProject\notes\笔记\img\SpringBoot目录结构.png" alt="SpringBoot目录结构" style="margin:0" />

| 目录/文件名                | 作用                |
| -------------------------- | ------------------- |
| **controller**             | **控制层**          |
| **entity**                 | **实体类层**        |
| **service**                | **业务层**          |
| **mapper**                 | **数据层**          |
| **App.java**               | **启动文件**        |
| **TestMapper.xml**         | **数据XML映射文件** |
| **application.properties** | **数据库配置文件**  |
| **test**                   | **测试目录**        |
| **pom.xml**                | **`maven` 配置**    |

### pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.5</version>
	</parent>
	<groupId>com.test</groupId>
	<artifactId>springtest</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springtest</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jdbc</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
			<version>2.5.0</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.2.0</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>

```

### Controller

```java
package com.test.springtest.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.test.springtest.entity.Test;
import com.test.springtest.mapper.TestMapper;

@RestController
@RequestMapping("/test")
public class TestController {
	@Autowired
	private TestMapper testMapper;

	@GetMapping("/getInfo")
	public List<Test> getInfo(int id) {
		List<Test> list = testMapper.getInfo(id);
		return list;
	}
}
```

### Entity

```java
package com.test.springtest.entity;

public class Test {
	private int id;
	private String type;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
	}
}
```

### Service

```java
package com.test.springtest.service;

import java.util.List;

import org.apache.ibatis.annotations.Param;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.test.springtest.entity.Test;
import com.test.springtest.mapper.TestMapper;

@Service
public class TestService {
	@Autowired
	TestMapper testMapper;

	public List<Word> getInfo(int id) {
		List<Word> list = testMapper.getInfo(id);
		return list;
	}
}
```

### Mapper

```java
package com.test.springtest.mapper;

import com.test.springtest.entity.Test;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface TestMapper { 
	List<Test> getInfo(@Param("id") int id);
}
```

### application.properties

```properties
mybatis.type-aliases-package=com.test.springtest.entity
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/my_project?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=000000
```

### TestMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.springtest.mapper.TestMapper">
	<resultMap id="test" type="Test">
		<id column="id" property="id" javaType="int" />
		<result column="type" property="type" javaType="String" />
	</resultMap>
    <!-- 全局字段 -->
    <sql id="list">
		id, w_name, w_kana, w_mean
	</sql>
	<select id="queryAllUser" resultType="Test">
        <!-- 传参 -->
        select 
        	<include refid="list" />
        from
        	m_type 
        where 
        	id=#{id}
    </select>
</mapper>
```

- **默认接口地址: `http://localhost:8080/test/getInfo`**

### 接口传参

#### 路径传参

```java
package com.test.springtest.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/word")
public class TestController {
    // /test?id=10
	@GetMapping("/test")
	public int Test(@RequestParam int id) {
		return id;
	}
    // /test/10
    @GetMapping("/test/{id}")
	public int Test(@PathVariable int id) {
		return id;
	}
}
```

#### `JSON` 传参

```java
package com.test.springtest.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/word")
public class TestController {
	@PostMapping("/test")
	public Object Test(@RequestBody Object id) {
		return id;
	}
}
```

#### `form-data` 传参

```java
package com.test.springtest.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/word")
public class TestController {
	@PostMapping("/test")
	public int Test(@RequestParam int id) {
		return id;
	}
}
```

