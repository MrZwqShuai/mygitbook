# 上面带AngularJs中如何创建自定义指令

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

  代码: [https://codepen.io/MrZwqShuai/pen/MvaEjz](https://codepen.io/MrZwqShuai/pen/MvaEjz)

* 上述代码中我们可以看到一个简单的指令（这里和指令的思想有点不同，指令是为了分离dom操作，这里为了举例就没有依照指令的思路了），我们写了一个控制器myController然后紧接着在后面注册了一个指令myDirective，这里也可以也在module的后面，每当一个指令创建的时候都会生成一个作用域是可以选择的：

* * 1.继承父级作用域（一般是嵌套在外面的控制器：这里是myController），或者继承最外层的根作用域（$rootScope\)
* * 2.创建一个新的子作用域（子作用域原型继承该组件使用位置所在的作用域）
* * 3.创建一个独立的作用域（该作用域没有原型继承，也称隔离作用域，这里是复用组件最重要的设置）

```
angular.module("MyApp", [])
    .controller("MyController", function ($scope) {
    $scope.myName='周杰伦' ;
    $scope.myAge = 18 ;
}).directive('myDirective',function(){
  return {
    restrict:'AE',
    scope:false,//默认是false
    template:'<p>我是指令生成的<p>'+
    "<h3>你的年龄{{myAge}}</h3><input type='text' ng-model='myAge'>"
  }
})
```

上面代码中由于字段scope是默认的false值，所以该指令的作用域就是具体使用所在的作用域，所以可以看到当你在输入内容的时候 ，上面的内容会一起改变，如果你将这里的scope字段设置成true，再做同样的事看看有什么不一样的地方，你会发现你上面的输入框并没有一起改变，这是因为scope字段设置成true的时候此时的指令创建一个新的子作用域，又因为子作用域是根据原型继承来继承父作用域 （会在scope作用域继承讲到），这里的myAge不是对象的引用，所以就会造成父级作用域的数据不会因为子作用域的改变而改变。

#### 隔离作用域（独立作用域）

1. 这里单独讲解指令生成作用域的第三种情况是因为这里才是复用组件最强大的地方，隔离作用域使我们在不知道外部环境的情况下正常工作，不依赖外部环境，达到复用的效果。

> 虽然独立作用域和父作用域不存在原型继承关系，但是还是可以通过$parent属性来访问父作用域，不推荐这么做，这样破坏了指令与周围环境的隔离效果

2.还是贴出我们的代码:[https://codepen.io/MrZwqShuai/pen/MvaEjz](https://codepen.io/MrZwqShuai/pen/MvaEjz)

> 当我们输入指令18的框的时候父作用域对应的年龄属性也跟着改变，当我们输入指令第二个框的时候只有指令对应的属性发生相应变化，当我们点击改变姓名，指令相应的属性也会改变。

* 虽然这里是独立的作用域但是可以通过scope的{...}传入特殊的前缀标识符如：@，=，&lt;，& 来进行数据绑定
* 创建独立作用域之后，我们可以用这些标识符来引用指令元素的属性就像上面代码那样
* ```
  <myDirective my-directive my-age="myAge" my-name="{{myName}}" change-my-Name="changeName()"></myDirective>
  ```
* 前缀标识符接口的描述



