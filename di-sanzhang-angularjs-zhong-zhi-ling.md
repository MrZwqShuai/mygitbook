# AngularJs中如何创建自定义指令

#### 1.什么是AngularJs指令

> 指令是AngularJs中最强大的功能，是处理程序逻辑与DOM的桥梁

AngularJs中的内置指令不做描述，比如ng-click，ng-controller，ng-model，ng-show，ng-class，ng-if，ng-repeat等等，这些用法多看多用很快就能熟悉，值得一提的是Angular源码中内置指令所使用的的API与自定义指令完全一样。

#### 2. 定义指令

* 指令应该在模块上注册，方法是在module上调用directive方法，调用方法的时候需要传递指令的标准名称和一个返回指令定义的工厂函数，看起来就像下面这个样子:

```
angular.module('myApp', [ //依赖模块]),directive('myDirective',function() {
    return myDirectiveDefinition;
})
```



