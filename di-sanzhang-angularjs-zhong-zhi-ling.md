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

* 这里返回的指令定义是一个对象，它包含的字段告知编译器怎么做，如何操作dom，比如 replace: true，这个字段告诉编译器用模板替换原有的元素。下面罗列出指令定义的所有字段:
  | 字段 | 描述 |
  | :--- | :--- |
  | name | 指令名称 |
  | restrict | 指令可以使用哪种标签形式 |
  | priority | 提示编译器该指令之后继续编译其他指令 |
  | terminal | 编译器是否在该指令之后继续编译其他指令 |
  | link | 定义将指令与作用域连接起来的链接函数 |
  | template | 用于生成指令标签的字符串 |
  | replace | 是否使用模板内容替换指令的现有元素 |
  | transclude | 是否为指令模板和编译函数提供指令元素中的内容 |
  | scope | 为指令穿件一个新的子作用域，还是创建一个独立的作用域 |
  | controller | 一个作为指令控制器的函数 |
  | require | 设置要注入当前指令链接函数中的其他指令的控制器 |
  | compile | 定义编译函数，编译函数会操作原始DOM，而且会在没有提供link设置的情况下创建链接函数 |

> 大多数的指令只需要其中的一部分字段

* 下面是一个简单的例子

  代码: [https://codepen.io/MrZwqShuai/pen/jLbBwm](https://codepen.io/MrZwqShuai/pen/jLbBwm)

* 上述代码中我们可以看到一个简单的指令（这里和指令的思想有点不同，指令是为了分离dom操作，这里为了举例就没有依照指令的思路了），我们写了一个控制器myController然后紧接着在后面注册了一个指令myDirective，这里也可以也在module的后面，每当一个指令创建的时候都会生成一个作用域是可以选择的：

* * 1.继承父级作用域（一般是嵌套在外面的控制器：这里是myController），或者继承最外层的根作用域（$rootScope\)
* * 2.创建一个新的子作用域（子作用域原型继承该组件使用位置所在的作用域）

* * 3.创建一个独立的作用域（该作用域没有原型继承，也称隔离作用域，这里是复用组件最重要的设置）





