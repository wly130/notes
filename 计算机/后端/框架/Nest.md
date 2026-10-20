## NestJS框架

- **基于 [Node.js](https://nodejs.org/en/) 平台，使用 TypeScript 构建的高效、可扩展的企业级 Web 开发框架**

### 创建项目

- **安装依赖包**

```shell
npm install -g @nestjs/cli  #全局安装CLI工具
```

- **创建项目**

```shell
nest new 项目名称
cd 项目名称
npm install  #安装依赖
```

### 目录结构

| 文件名                    | 功能                                           |
| ------------------------- | ---------------------------------------------- |
| **src/main.ts**           | 启动文件,应用入口,创建Nest应用实例并监听端口   |
| **src/app.module.ts**     | 根模块,组织所有模块、控制器、服务              |
| **src/app.controller.ts** | 根控制器,处理HTTP请求                          |
| **src/app.service.ts**    | 根服务,处理业务逻辑                            |
| **config**                | 配置文件目录                                   |
| **src/modules**           | 业务模块目录                                   |
| **src/common**            | 公共模块(守卫、管道、拦截器、装饰器等)         |
| **src/entities**          | 数据库实体目录                                 |
| **src/dto**               | 数据传输对象目录                               |
| **test**                  | 测试目录                                       |
| **nest-cli.json**         | CLI配置文件                                    |
| **tsconfig.json**         | TypeScript配置                                 |
| **package.json**          | 项目依赖配置                                   |

- **启动项目**

```shell
npm run start        #普通启动
npm run start:dev    #开发模式(热重载)
```

- **监听端口(main.ts)**

```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    await app.listen(3000, () => console.log('服务已开启,端口号:3000'));
}
bootstrap();
```

**默认访问地址为 `http://localhost:3000/`**

### 请求头

#### 设置请求头

```ts
import { Controller, Get, Res, Headers } from '@nestjs/common';
import { Response } from 'express';

@Controller()
export class AppController {
    @Get('/url')
    getData(@Res() res: Response, @Headers() headers) {
        res.set('Content-Type', 'application/json');
        res.json({ message: 'success' });
    }
}
```

#### 设置 `Token`

- **下载依赖**

```shell
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
```

- **配置 `Token`(auth.module.ts)**

```ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { AuthService } from './auth.service';

@Module({
    imports: [
        JwtModule.register({
            secret: 'KEY',           //密钥
            signOptions: {
                expiresIn: '24h',    //token有效期
                algorithm: 'HS256'   //加密算法
            }
        })
    ],
    providers: [AuthService],
    exports: [AuthService]
})
export class AuthModule {}
```

- **生成与验证 `Token`(auth.service.ts)**

```ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';

@Injectable()
export class AuthService {
    constructor(private jwtService: JwtService) {}

    //生成token
    async setToken(data: any): Promise<string> {
        return this.jwtService.sign(data);
    }

    //验证token
    async getToken(token: string): Promise<any> {
        if (!token) {
            throw { code: 404, error: 'token空' };
        }
        try {
            const info = this.jwtService.verify(token.split(' ')[1]);
            return info; //解析返回值
        } catch (e) {
            throw { code: 401, error: '无效token' };
        }
    }
}
```

- **验证 `Token`(Guard守卫)**

```ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { Request } from 'express';

@Injectable()
export class JwtGuard implements CanActivate {
    constructor(private jwtService: JwtService) {}

    async canActivate(context: ExecutionContext): Promise<boolean> {
        const request = context.switchToHttp().getRequest<Request>();
        const token = request.headers['authorization'];
        if (!token) return false;
        try {
            const payload = this.jwtService.verify(token.split(' ')[1]);
            request['user'] = payload;
            return true;
        } catch {
            return false;
        }
    }
}
```

### 网络请求

#### GET请求

```ts
import { Controller, Get, Query, Param } from '@nestjs/common';

@Controller('url')
export class AppController {
    @Get(':name')
    getData(@Query() query, @Param('name') name: string) {
        //query: 请求参数(URL参数)
        //name: 路径参数
        return { message: 'success' }; //响应参数(自动转为JSON)
    }
}
```

#### POST请求

- **`application/json` 格式(默认支持)**

```ts
import { Controller, Post, Body } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';

@Controller()
export class AppController {
    @Post('/url')
    createData(@Body() body: CreateUserDto) {
        //body: 请求参数
        return { message: '创建成功' }; //响应参数
    }
}
```

- **`www-form-urlencoded` 格式**

```ts
import { Body, Controller, Post, UsePipes } from '@nestjs/common';

@Controller()
export class AppController {
    @Post('/url')
    @UsePipes(new UrlencodedBodyParser())
    createData(@Body() body) {
        return { message: 'success' };
    }
}

//main.ts 中配置
//app.use(urlencoded({ extended: true }));
```

- **`form-data` 文件上传**

```shell
npm install @nestjs/platform-express multer
npm install -D @types/multer
```

```ts
import { Controller, Post, UseInterceptors, UploadedFile } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { diskStorage } from 'multer';

@Controller()
export class AppController {
    @Post('/uploadFile')
    @UseInterceptors(FileInterceptor('file', {
        storage: diskStorage({
            destination: './assets/',   //上传文件目录
            filename: (req, file, cb) => {
                //自定义 时间戳+文件名
                cb(null, Date.now() + '-' + file.originalname);
            }
        })
    }))
    uploadFile(@UploadedFile() file: Express.Multer.File) {
        return { file }; //文件详情
    }
}
```

- **DTO 数据传输对象(dto/create-user.dto.ts)**

```ts
import { IsString, IsNotEmpty, IsEmail } from 'class-validator';

export class CreateUserDto {
    @IsString()
    @IsNotEmpty()
    name: string;

    @IsEmail()
    email: string;
}
```

- **请求参数**

| 装饰器/参数              | 功能                                |
| ------------------------ | ----------------------------------- |
| @Request() / @Req()      | 获取原始请求对象                    |
| @Response() / @Res()     | 获取原始响应对象                    |
| @Param(key?)             | 获取路由路径参数                    |
| @Query(key?)             | 获取URL查询参数                     |
| @Body(key?)              | 获取请求体参数                      |
| @Headers(name?)          | 获取请求头                          |
| @Ip()                    | 获取客户端IP地址                    |
| @HostParam()             | 获取主机参数                        |

- **响应参数**

| 方式                     | 功能                                |
| ------------------------ | ----------------------------------- |
| return 对象              | 自动序列化为JSON响应                |
| @Res() res.json()        | 手动发送JSON响应                    |
| @Res() res.send()        | 手动发送HTTP响应                    |
| @Res() res.status(404)   | 设置HTTP状态码                      |
| @Header('key', 'value')  | 设置响应头                          |
| @Redirect(url, statusCode)| 重定向                              |
| @HttpCode(code)          | 自定义HTTP状态码                    |

### 数据库操作(TypeORM)

- **下载依赖包**

```shell
npm install @nestjs/typeorm typeorm mysql2
```

- **数据库连接(app.module.ts)**

```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
    imports: [
        TypeOrmModule.forRoot({
            type: 'mysql',            //数据库类型
            host: '127.0.0.1',        //数据库地址
            port: 3306,               //端口号
            username: 'root',         //用户名
            password: '000000',       //密码
            database: 'my_project',   //数据库名
            entities: [__dirname + '/**/*.entity{.ts,.js}'],
            synchronize: true,        //自动同步(仅开发环境)
            extra: {
                max: 5,              //连接池最大连接数
                min: 0,              //连接池最小连接数
                acquire: 3000
            }
        })
    ]
})
export class AppModule {}
```

- **定义实体(entities/m-type.entity.ts)**

```ts
import { Entity, Column, PrimaryColumn } from 'typeorm';

@Entity('m_type')  //数据表名
export class MType {
    @PrimaryColumn()
    id: number;    //字段名,主键

    @Column({ nullable: false })
    type: string;  //字段名,是否为空
}
```

- **注册实体到模块(user.module.ts)**

```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { MType } from './entities/m-type.entity';
import { UserService } from './user.service';
import { UserController } from './user.controller';

@Module({
    imports: [TypeOrmModule.forFeature([MType])],
    controllers: [UserController],
    providers: [UserService],
    exports: [UserService]
})
export class UserModule {}
```

- **服务层CRUD操作(user.service.ts)**

```ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { MType } from './entities/m-type.entity';

@Injectable()
export class UserService {
    constructor(
        @InjectRepository(MType)
        private readonly mTypeRepo: Repository<MType>
    ) {}

    //添加
    async create(data: Partial<MType>) {
        await this.mTypeRepo.save(data);
        return '添加成功';
    }
    //删除
    async delete(id: number) {
        await this.mTypeRepo.delete({ id });
        return '删除成功';
    }
    //修改
    async update(id: number, data: Partial<MType>) {
        await this.mTypeRepo.update({ id }, data);
        return '修改成功';
    }
    //查询
    async findAll(type: string) {
        return await this.mTypeRepo.find({ where: { type } });
    }
}
```

- **控制器层(user.controller.ts)**

```ts
import { Controller, Get, Post, Body, Query } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('user')
export class UserController {
    constructor(private readonly userService: UserService) {}

    @Post('/add')
    async add(@Body() body) {
        return this.userService.create(body);
    }

    @Post('/del')
    async del(@Body() body) {
        return this.userService.delete(body.id);
    }

    @Post('/update')
    async update(@Body() body) {
        return this.userService.update(body.id, body);
    }

    @Get('/select')
    async select(@Query('type') type: string) {
        const data = await this.userService.findAll(type);
        return { data };
    }
}
```

### 模块与路由

- **根模块(app.module.ts)**

```ts
import { Module } from '@nestjs/common';
import { UserModule } from './modules/user/user.module';
import { AuthModule } from './modules/auth/auth.module';

@Module({
    imports: [UserModule, AuthModule]  //导入子模块
})
export class AppModule {}
```

- **业务模块(modules/user/user.module.ts)**

```ts
import { Module } from '@nestjs/common';
import { UserController } from './user.controller';
import { UserService } from './user.service';

@Module({
    controllers: [UserController],  //注册控制器
    providers: [UserService],       //注册服务(提供者)
    exports: [UserService]          //导出服务供其他模块使用
})
export class UserModule {}
```

- **控制器路由(user.controller.ts)**

```ts
import { Controller, Get, Post, Body, Param, Query } from '@nestjs/common';

@Controller('xxx')  //路由前缀
export class UserController {
    @Get('/url')  //GET  /xxx/url
    getData() {
        return { message: 'success' };
    }

    @Post('/url')  //POST /xxx/url
    postData(@Body() body) {
        return { message: 'success' };
    }

    @Get('/url/:id')  //GET  /xxx/url/:id
    getById(@Param('id') id: string) {
        return { id };
    }
}
```

- **访问接口: `http://localhost:3000/xxx/url`**

### 常用概念

| 概念                 | 功能                                                       |
| -------------------- | ---------------------------------------------------------- |
| **Module**           | 模块,组织代码的基本单元,使用@Module装饰器                  |
| **Controller**       | 控制器,处理HTTP请求,使用@Controller装饰器                  |
| **Provider/Service** | 提供者/服务,处理业务逻辑,使用@Injectable装饰器             |
| **DTO**              | 数据传输对象,定义接口输入数据结构                          |
| **Guard**            | 守卫,权限验证,使用@Injectable + CanActivate接口            |
| **Pipe**             | 管道,数据验证和转换,使用@Injectable + PipeTransform接口    |
| **Interceptor**      | 拦截器,AOP切面编程,用于日志、缓存、超时处理等              |
| **Middleware**       | 中间件,路由处理前/后执行的函数                             |
| **Exception Filter** | 异常过滤器,统一异常处理                                    |
| **Decorator**        | 装饰器,自定义参数或路由装饰                                |
