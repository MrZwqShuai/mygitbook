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

* 出现上面的原因并不是AngularJs做的事情，这是JavaScript的原型继承起作用了，我们之前说过AngularJs作用域继承完全遵循JavaScript原型继承，只要弄懂了Javascript的原型继承就理解上面差异的原因了，所以接下来我们有必要深入一下JavaScript的原型继承。

##### Javascript原型继承

这里的ParentScope和ChildScope就相当于父子控制器，我只拿出其中可以解释父子作用域继承出现差异的原因部分，原文链接在这里[https://segmentfault.com/a/1190000004358393](https://segmentfault.com/a/1190000004358393)，讲的很详细了。

```
function ParentScope(){
    this.aString = "parent string";
    this.aNumber = 100;
    this.anArray = [10,20,30];
    this.anObject = {
    'property1': 'parent prop1',
    'property2': 'parent prop2' 
    };
    this.aFunction = function(){ 
      return 'parent output'; 
    }
}

function ChildScope(){    
}

ChildScope.prototype = new ParentScope();

var childScope = new ChildScope();
```

ChildScope 原型继承自 ParentScope

![](/assets/2810593304-56a4ca9accb89_articlex.png)

如果我们要在 childScope 上查询一个定义在 parentScope 的属性, JavaScript 会先在 childScope 上查找, 如果没有查到, 那么会顺着原型链去查找. 所以以下判别式均为 true

```
childScope.aString === 'parent string'
childScope.anArray[1] === 20
childScope.anObject.property1 === 'parent prop1'
childScope.aFunction() === 'parent output'
```

如果我们做如下操作:

```
childScope.aString = 'child string'
```

原型链并没有被访问, 一个新的astring会被加入到 childScope 的属性中去, 新的属性会隐藏 parentScope 中的同名属性.

![](/assets/3584488447-56a4cab6b7f25_articlex.png)

假设我们做出如下操作:

```
childScope.anArray[1] = 22
childScope.anObject.property1 = 'child prop1'
```

原型链被访问了. 因为 anArray, anObject 没有在 childScope 中找到. 新的赋值操作均在 parentScope 上进行. childScope 上没有添加任何新的属性.

![](/assets/2529754630-56a4cacdaf2ea_articlex.png)

如果我们做出如下操作

```
childScope.anArray = [100, 555]
childScope.anObject = { name: 'Mark', country: 'USA' }
```

![](/assets/1674980122-56a4cadf44380_articlex.png)



原型链没有被访问, childScope 会获得两个新的属性, 并且会隐藏 parentScope 上的同名属性.

仔细体会上面的三次操作. 第一第三次均是对某个属性进行赋值, 原型链并不会被访问, 由于属性并不存在, 所以新的属性将会被添加. 而第二次其实是先访问, childScope.anArray, childScope.anObject, 再对其访问的对象的某个属性进行复制.

##### 总结：

* 如果我们读取 childScope.propertyX, 而 childScope 拥有 propertyX, 那么原型链不会被访问

* 如果我们读取 childScope.propertyX, 而 childScope 并没有 propertyX, 那么原型链会被访问.

* 如果对 childScope.propertyX 进行赋值, 那么原型链并不会被访问.



