## AngularJs作用域的原型继承

1. 作用域概念：AngularJs中的$scope对象是模板的域模型，也叫作作用域实例。

2. 作用域scope可以定义方法，以封装UI交互逻辑供模板使用。举个简单的例子:

```
var myController = function($scope) {
    $scope.name = 'yourname' ;
    $scope.getName = function() {
        return $scope.name ;
    }
}
```

然后在模板中调用它:

```
<h1>hello,{{getName()}}</h1>
```

h1标签中会显示$scope.name的值，yourname。

3.我们知道scope是可以继承的，AngularJs中的作用域继承和Javascript中对象的原型继承遵循同样的规则（沿着继承树向上查找属性，直至找到为止）。

#### 深入作用域继承

* 在AngularJs中，子作用域一般会继承其父作用域（当指令使用scope:{...}来定义的时候是个例外），比如你在子作用域中使用了双向数据绑定（类似ng-model），你的父作用域上原始类型（例如：number，string，boolean）和子作用域同名的属性并不会像你期望的那样工作，而是子作用域得到了它自己的属性，从而覆盖了父作用域上面的同名属性就像下面:

* 例子[https://codepen.io/MrZwqShuai/pen/qXZWPe](https://codepen.io/MrZwqShuai/pen/qXZWPe)

> ParentController控制器中的原始类型并不会随着ChildrenController控制器（子作用域）的改变而改变，如果我们把$scope.myName ="小头爸爸" ;换成注释的代码，就会像你期望的那样工作了，所以我们推荐在你的模型中始终使用' . '，就是将变量绑定为对象的属性，而不是直接绑定作用域的属性。



