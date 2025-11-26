## Nuxt.js

**[Nuxt.js](https://nuxtjs.org/) 是 Vue 服务端渲染框架**

### 初始化

- **创建项目**

```shell
npx create-nuxt-app 项目名
```

- **启动**

```shell
npm run dev
```

- **编译应用**

```shell
npm run generate
```

### 目录结构

<img src="../../img/目录结构/Nuxt%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png" style="zoom:120%;margin:0;" />

| 文件               | 作用         |
| ------------------ | ------------ |
| .**nuxt**          | 打包目录     |
| **assets**         | 静态资源目录 |
| **components**     | Vue组件目录  |
| **layouts**        | 布局目录     |
| **middleware**     | 中间件目录   |
| **pages**          | 页面目录     |
| **plugins**        | 插件目录     |
| **static**         | 静态文件目录 |
| **store**          | Vuex目录     |
| **nuxt.config.js** | 个性化配置   |

### 配置

### 上传服务器

1. **`npm run build` 打包**

2. **`.nuxt`，`static`，`nuxt.config.js`，`package.json` 上传到服务器**

3. **`npm install` 下载依赖包**

4. **`npm run build` 启动项目**
