#### Express 是一个基于 Node.js 平台的极简、灵活的 web 应用开发框架，它提供一系列强大的特性，帮助你创建各种 Web 和移动设备应用。

##### 1.安装NodeJs，这应该没什么可说的吧，基本上玩前端的都应该安装了吧，环境配置也不需要手动配置，安装的时候都给你配置好了

##### 2.进入express官网，入门那里有个express生成器，通过应用生成器工具 express 可以快速创建一个应用的骨架。通过如下命令安装

```
$ npm install express-generator -g
```

##### 安装指南链接[http://www.expressjs.com.cn/starter/generator.html](http://www.expressjs.com.cn/starter/generator.html)

例如，下面的示例就是在当前工作目录下创建一个命名为 [**Micro-agency-Demo**](https://github.com/MrZwqShuai/Micro-agency-Demo)的应用。

    $ express Micro-agency-Demo

      warning: the default view engine will not be jade in future releases
      warning: use `--view=jade' or `--help' for additional options


       create : Micro-agency-Demo
       create : Micro-agency-Demo/package.json
       create : Micro-agency-Demo/app.js
       create : Micro-agency-Demo/routes
       create : Micro-agency-Demo/routes/index.js
       create : Micro-agency-Demo/routes/users.js
       create : Micro-agency-Demo/views
       create : Micro-agency-Demo/views/index.jade
       create : Micro-agency-Demo/views/layout.jade
       create : Micro-agency-Demo/views/error.jade
       create : Micro-agency-Demo/public
       create : Micro-agency-Demo/bin
       create : Micro-agency-Demo/bin/www
       create : Micro-agency-Demo/public/javascripts
       create : Micro-agency-Demo/public/images
       create : Micro-agency-Demo/public/stylesheets
       create : Micro-agency-Demo/public/stylesheets/style.css

       install dependencies:
         $ cd Micro-agency-Demo && npm install

       run the app:
         $ DEBUG=micro-agency-demo:* npm start

然后安装所有依赖包：

```
$ cd Micro-agency-Demo
$ npm install
```

启动这个应用（MacOS 或 Linux 平台）：

```
$ DEBUG=Micro-agency-Demo npm start
```

Windows 平台使用如下命令：

```
> set DEBUG=Micro-agency-Demo & npm start
```

然后在浏览器中打开 [http://localhost:3000/](http://localhost:3000/)网址就可以看到这个应用了。

通过 Express 应用生成器创建的应用一般都有如下目录结构：

```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.jade
    ├── index.jade
    └── layout.jade

7 directories, 9 files
```

> 通过 Express 应用生长期创建应用只是众多方法中的一种。你可以不使用它，也可以修改它让它符合你的需求!
