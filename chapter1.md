## 如何构建AngularJs的开发目录

### Angular项目初始化

* 这里我们使用的是AngularJs1 不像Angular2以上有Angular-cli这个强大的脚手架工具 所以这里我推荐 Yeoman和Angular-seed ;  为了方便我们使用Angular-seed可以快速的搭建一个基于Angular的WebApp工程。 打开我们的git命令或者bower工具切换到我们的根目录cd /ts-blog 然后输入：

```
  $ git clone https://github.com/angular/angular-seed.git
```

* 在你的ts-blog文件加下面会生成![](/assets/import.png)

* 这里的app文件夹是你项目代码核心，这里我们不用seed自动生成的目录，自定义自己的目录，设置directive，controller，service，routes，interfaces作为文件夹存放对应的js文件，views存放对应的视图HTML文件，我们的目录看起来就像下面这个样子:



