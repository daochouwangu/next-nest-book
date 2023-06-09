# 01-项目搭建

## monorepo 架构

这个项目我们使用`monorepo`的架构，`monorepo` 是最近比较流行的架构，他的优点是包和包之间即独立又在一个库中，不用发布也能用最新的版本，现在比较流行的 `next.js/vite/react` 都采用了这种架构。

我们采用`pnpm`来搭建

[官方文档：](https://pnpm.io/workspaces)

1.  先建立个项目文件夹，名字随便起，我这里叫 aligiegie

    ```bash
    mkdir aligiegie
    ```
2.  然后初始化

    ```bash
    git init 
    pnpm init
    ```

    可以改下 .gitignore 把常用的node项目的拷贝过来
3.  创建对应的目录

    ```bash
    mkdir packages # 一般是用packages
    cd packages # 切换到目录下
    ```

## 使用Next创建前端项目

我们项目使用 next 来做前端，[打开官网看看怎么开始新项目](https://nextjs.org/docs/getting-started/installation)

```bash
npx create-next-app@latest
```

因为我们使用pnpm 所以可以用

```
pnpx create-next-app@latest
```

> 这里科普下，npx是直接下载包（不安装依赖）然后直接运行。然后还有俩个指令
>
> 1. pnpm dlx 这个和 npx 是一样的
> 2. pnpm create 这个是直接用 create-xxx的包
>
> 所以还可以用:
>
> ```bash
> pnpm dlx create-next-app@latest
> pnpm create next-app@latest
> ```

## 使用Nest创建后端项目

同理，使用 Nest 来做后端，[官网](https://docs.nestjs.com/#installation)

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```

因为我们用的pnpm 所以理论上

```bash
$ pnpm add -g @nestjs/cli
$ nest new project-name
```

当然，我也推荐这么做，但是你也可以用前面说的不安装的方式

```bash
$ pnpm dlx @nestjs/cli new project-name # 参数直接跟在后面
```

## 建立`monorepo`关系

在根目录下的 `package.json` 中增加：

```json
"workspaces": [
  "packages/*"
],
```

然后我们去改下子项目的package.json 把名字统一下

```json
"name": "@aligiegie/web",
```

## 命令：一键运行前后的

一般来说我们可以分别运行每个项目，但是你也可以在根目录下的 package.json中创建新的脚本，一键运行前后端。 这里我用了 concurrently 包，你要 pnpm add 一下，这个包可以让你一次性同时运行多个脚本。

```bash
pnpm add concurrently
```

使用 `pnpm --filter` 可以指定在哪个子包下 在根`package.json`中的`scripts`中增加：

```json
"dev:web": "pnpm --filter @aligiegie/web dev",
"dev:server": "pnpm --filter @aligiegie/server start",
"dev": "concurrently \"pnpm dev:web\" \"pnpm dev:server\""
```

这里注意，`nest` 和 `next` 都默认用了`3000`端口，一起开会冲突，我们改下端口， 这里我改的是next端口，你也可以改其他的。

```
"dev": "next dev -p 3002",
```

然后打开 http://localhost:3002/

第一步完成！
