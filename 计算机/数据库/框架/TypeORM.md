## TypeORM

- **`TypeScript` 的 ORM 数据库框架,支持 MySQL、PostgreSQL、SQLite、MSSQL 等**

### 初始化

- **下载依赖包**

```shell
npm install typeorm reflect-metadata -S
npm install mysql2 -S
```

- **数据源配置(data-source.ts)**

```typescript
import { DataSource } from "typeorm";

export const AppDataSource = new DataSource({
    type: "mysql",                    //数据库类型
    host: "127.0.0.1",                //数据库地址
    port: 3306,                       //端口号
    username: "root",                 //用户名
    password: "000000",               //密码
    database: "my_project",           //数据库名
    synchronize: true,                //自动同步表结构(仅开发环境)
    logging: false,                   //关闭日志
    entities: ["src/entity/*.ts"],    //实体文件路径
    extra: {                          //连接池配置
        max: 5,                       //最大连接数
        min: 0,                       //最小连接数
        connectionTimeoutMillis: 30000
    }
});
```

- **初始化连接**

```typescript
import { AppDataSource } from "./data-source";

AppDataSource.initialize()
    .then(() => console.log("数据库连接成功"))
    .catch(err => console.log("数据库连接失败", err));
```

- **定义实体(entity/User.ts)**

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity("user")  //指定表名
export class User {
    @PrimaryGeneratedColumn()  //主键,自增
    id: number;

    @Column({ length: 100, unique: true })  //字段配置
    name: string;

    @Column({ nullable: true })  //允许为空
    email: string;

    @Column({ default: 1, comment: "状态" })  //默认值+备注
    status: number;
}
```

- **常用列装饰器**

| 装饰器                    | 说明                     |
| ------------------------- | ------------------------ |
| @Column()                 | 普通字段                 |
| @PrimaryColumn()          | 主键(不自增)             |
| @PrimaryGeneratedColumn() | 主键(自增)               |
| @CreateDateColumn()       | 自动创建时间             |
| @UpdateDateColumn()       | 自动更新时间             |
| @DeleteDateColumn()       | 软删除时间               |
| @VersionColumn()          | 乐观锁版本号             |

- **Column 配置项**

| 配置项           | 说明                     |
| ---------------- | ------------------------ |
| type             | 数据库列类型             |
| length           | 字段长度                 |
| nullable         | 是否允许为空(默认false)  |
| unique           | 是否唯一                 |
| default          | 默认值                   |
| primary          | 是否为主键               |
| comment          | 字段备注                 |
| select           | 查询时是否默认返回       |

### 数据类型

| TypeScript 类型 | 装饰器 type 值   | MySQL 类型  | 说明           |
| --------------- | ---------------- | ----------- | -------------- |
| string          | "varchar"        | VARCHAR     | 字符串(255)    |
| string          | "text"           | TEXT        | 长文本         |
| number          | "int"            | INT         | 整数           |
| number          | "bigint"         | BIGINT      | 长整数         |
| number          | "float"          | FLOAT       | 单精度浮点     |
| number          | "double"          | DOUBLE      | 双精度浮点     |
| number          | "decimal"        | DECIMAL     | 精确小数       |
| boolean         | "tinyint"        | TINYINT(1)  | 布尔值         |
| Date            | "datetime"       | DATETIME    | 日期时间       |
| Date            | "date"           | DATE        | 仅日期         |
| object          | "json"           | JSON        | JSON数据       |
| string          | "enum"           | ENUM        | 枚举           |

```typescript
@Column({ type: "varchar", length: 64 })
name: string;

@Column({ type: "enum", enum: ["admin", "user"] })
role: string;
```

### 添加数据

```typescript
const userRepo = AppDataSource.getRepository(User);

//添加单条
// INSERT INTO user (name, email) VALUES ("Tom", "tom@mail.com");
const user = new User();
user.name = "Tom";
user.email = "tom@mail.com";
await userRepo.save(user);

//添加多条
await userRepo.save([
    { name: "Tom", email: "tom@mail.com" },
    { name: "Jerry", email: "jerry@mail.com" }
]);

//使用insert(不触发级联和监听器)
await userRepo.insert([
    { name: "Tom" },
    { name: "Jerry" }
]);
```

### 修改数据

```typescript
const userRepo = AppDataSource.getRepository(User);

//按条件修改
// UPDATE user SET name="Jerry" WHERE id=1;
await userRepo.update({ id: 1 }, { name: "Jerry" });

//批量修改
// UPDATE user SET status=0 WHERE age > 30;
await userRepo.update(
    { age: MoreThan(30) },
    { status: 0 }
);

//查找后修改
const user = await userRepo.findOneBy({ id: 1 });
user.name = "NewName";
await userRepo.save(user);
```

### 删除数据

```typescript
const userRepo = AppDataSource.getRepository(User);

//按条件删除
// DELETE FROM user WHERE id=1;
await userRepo.delete({ id: 1 });

//按ID数组删除
await userRepo.delete([1, 2, 3]);

//软删除(设置deletedAt字段)
await userRepo.softDelete({ id: 1 });

//清空表
await userRepo.clear();
```

### 单表查询

#### 查询方法

| 方法                    | 说明                       |
| ----------------------- | -------------------------- |
| find()                  | 查询所有数据               |
| findOne()               | 查询第一条匹配数据         |
| findOneBy()             | 按条件查询第一条           |
| findOneOrFail()         | 查询不到则抛异常           |
| findAndCount()          | 查询数据+返回总数          |
| count()                 | 返回总数                   |
| countBy()               | 按条件返回总数             |
| exists()                | 判断是否存在               |
| query()                 | 原始SQL查询                |

#### 查询特定字段

```typescript
// SELECT id, name FROM user;
await userRepo.find({ select: { id: true, name: true } });

//使用QueryBuilder
await userRepo.createQueryBuilder("u")
    .select(["u.id", "u.name"])
    .getMany();
```

#### WHERE 条件(FindOperator)

```typescript
import {
    Equal, Not, MoreThan, MoreThanOrEqual,
    LessThan, LessThanOrEqual, Like, In,
    Between, IsNull, NotEqual, Any
} from "typeorm";

await userRepo.find({
    where: {
        // 简单等值查询
        status: 1,                         // status = 1
        name: Equal("Tom"),                // name = "Tom"
        // 比较运算
        age: MoreThan(18),                 // age > 18
        age: MoreThanOrEqual(18),          // age >= 18
        age: LessThan(60),                 // age < 60
        age: LessThanOrEqual(60),          // age <= 60
        age: Between(18, 60),             // age BETWEEN 18 AND 60
        // 集合运算
        id: In([1, 2, 3]),                // id IN (1, 2, 3)
        id: Not(In([1, 2])),              // id NOT IN (1, 2)
        // 字符串匹配
        name: Like("%Tom%"),              // name LIKE '%Tom%'
        name: Not(Like("%Tom%")),         // name NOT LIKE '%Tom%'
        // NULL判断
        email: IsNull(),                  // email IS NULL
        email: Not(IsNull()),             // email IS NOT NULL
        // 否定
        status: Not(0)                    // status != 0
    }
});

//多条件OR查询
await userRepo.find({
    where: [
        { name: "Tom" },       //条件1
        { name: "Jerry" }      //条件2 (OR关系)
    ]
});
```

#### 排序

```typescript
// SELECT * FROM user ORDER BY id DESC, name ASC;
await userRepo.find({
    order: { id: "DESC", name: "ASC" }
});
```

#### 分页

```typescript
// SELECT * FROM user LIMIT 10 OFFSET 20;
let page = 3, pageSize = 10;
await userRepo.find({
    skip: (page - 1) * pageSize,
    take: pageSize
});

//带总数的分页查询
const [rows, count] = await userRepo.findAndCount({
    skip: (page - 1) * pageSize,
    take: pageSize
});
```

### 关联查询

#### 关联装饰器

| 装饰器                | 说明                 | 外键位置       |
| --------------------- | -------------------- | -------------- |
| @OneToOne()           | 一对一               | @JoinColumn方  |
| @ManyToOne()          | 多对一               | 当前表         |
| @OneToMany()          | 一对多               | 对方表         |
| @ManyToMany()         | 多对多               | 中间表         |
| @JoinColumn()         | 指定外键列           | -              |
| @JoinTable()          | 多对多中间表         | -              |

#### 定义关联

```typescript
//一对一: User有一个Profile
@Entity()
export class User {
    @OneToOne(() => Profile, profile => profile.user)
    @JoinColumn({ name: "profile_id" })  //外键在User表
    profile: Profile;
}

@Entity()
export class Profile {
    @OneToOne(() => User, user => user.profile)
    user: User;
}

//一对多: User有多篇Post
@Entity()
export class User {
    @OneToMany(() => Post, post => post.user)
    posts: Post[];
}

@Entity()
export class Post {
    @ManyToOne(() => User, user => user.posts)
    @JoinColumn({ name: "user_id" })
    user: User;
}

//多对多: User和Role通过中间表关联
@Entity()
export class User {
    @ManyToMany(() => Role, role => role.users)
    @JoinTable({ name: "user_role" })  //中间表
    roles: Role[];
}

@Entity()
export class Role {
    @ManyToMany(() => User, user => user.roles)
    users: User[];
}
```

#### 关联查询(relations)

```typescript
//查询User及其Profile(一对一)
await userRepo.find({ relations: { profile: true } });

//查询User及其所有Post(一对多)
await userRepo.find({ relations: { posts: true } });

//嵌套关联
await userRepo.find({
    relations: { posts: { comments: true } }
});

//使用QueryBuilder
await userRepo.createQueryBuilder("u")
    .leftJoinAndSelect("u.posts", "post")
    .where("u.status = :status", { status: 1 })
    .getMany();
```

### 聚合与分组

```typescript
//分组统计
await userRepo.createQueryBuilder("u")
    .select("u.status", "status")
    .addSelect("COUNT(u.id)", "count")
    .groupBy("u.status")
    .getRawMany();

//聚合函数
await userRepo.count({ where: { status: 1 } });

//QueryBuilder中使用
await userRepo.createQueryBuilder("u")
    .select("MAX(u.age)", "maxAge")
    .addSelect("SUM(u.score)", "totalScore")
    .getRawOne();
```

### 钩子(Subscriber)

```typescript
import { EntitySubscriberInterface, EventSubscriber, InsertEvent } from "typeorm";

@EventSubscriber()
export class UserSubscriber implements EntitySubscriberInterface<User> {
    listenTo() { return User; }

    beforeInsert(event: InsertEvent<User>) {
        //创建前执行,如加密密码
    }
    afterInsert(event: InsertEvent<User>) {
        //创建后执行
    }
}
```

| 钩子方法        | 触发时机     |
| --------------- | ------------ |
| beforeInsert    | 插入前       |
| afterInsert     | 插入后       |
| beforeUpdate    | 更新前       |
| afterUpdate     | 更新后       |
| beforeRemove    | 删除前       |
| afterRemove     | 删除后       |
| afterLoad       | 加载后       |

### 事务

```typescript
//方式一: 使用transaction方法
await AppDataSource.transaction(async (manager) => {
    await manager.save(User, { name: "Tom" });
    await manager.delete(User, { id: 1 });
    await manager.update(User, { id: 2 }, { status: 1 });
    //自动提交,抛异常自动回滚
});

//方式二: 手动控制
const queryRunner = AppDataSource.createQueryRunner();
await queryRunner.connect();
await queryRunner.startTransaction();
try {
    await queryRunner.manager.save(User, { name: "Tom" });
    await queryRunner.manager.delete(User, { id: 1 });
    await queryRunner.commitTransaction();  //提交
} catch (err) {
    await queryRunner.rollbackTransaction(); //回滚
} finally {
    await queryRunner.release();             //释放
}
```

### 原始SQL查询

```typescript
//查询
const users = await AppDataSource.query(
    "SELECT * FROM user WHERE id = ?", [1]
);

//使用QueryBuilder(推荐)
const users = await userRepo.createQueryBuilder("u")
    .where("u.id = :id", { id: 1 })
    .andWhere("u.status = :status", { status: 1 })
    .orderBy("u.id", "DESC")
    .skip(0)
    .take(10)
    .getMany();
```

### QueryBuilder 常用方法

| 方法                 | 说明               |
| -------------------- | ------------------ |
| select()             | 指定查询字段       |
| addSelect()          | 追加查询字段       |
| where()              | WHERE条件          |
| andWhere()           | AND条件            |
| orWhere()            | OR条件             |
| orderBy()            | 排序               |
| addOrderBy()         | 追加排序           |
| groupBy()            | 分组               |
| having()             | HAVING条件         |
| skip()               | 跳过条数(分页)     |
| take()               | 获取条数(分页)     |
| leftJoinAndSelect()  | 左连接并查询关联   |
| innerJoinAndSelect() | 内连接并查询关联   |
| getMany()            | 返回实体数组       |
| getOne()             | 返回单个实体       |
| getRawMany()         | 返回原始结果数组   |
| getRawOne()          | 返回单个原始结果   |
| getCount()           | 返回总数           |
