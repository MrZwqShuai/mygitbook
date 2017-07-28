## AngularJs的作用域 $scope

1. 1作用域概念：AngularJs中的$scope对象是模板的域模型，也叫作作用域实例。

2. 2 作用域scope可以定义方法，以封装UI交互逻辑供模板使用。举个简单的例子:

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
<h1>hello,getName()</h1>
```



