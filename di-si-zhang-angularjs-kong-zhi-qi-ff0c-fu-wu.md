#### AngularJs控制器

* 控制器的基本写法

```
angular.module('app',[]).controller('myController',function($scope) { $scope.name = 'hello world' })
```

* 控制器应该尽可能短小精悍，在控制器中进行DOM操作和数据操作则是一个不好的实践，通过项目中的实践，我们应用一般会将复杂的逻辑放到指令和服务中心。轻量且更易维护的控制器看起来像这样:

```
angular.module('MyApp', [])
    .controller('MyController', function($scope, MyService) {
        $scope.login = function(user) {
            MyService.login(user);
        };
    });
```

* 这里的控制器相当于mvvm中的viewmodel，作为视图与数据的桥梁

> Angular2+中剔除了控制器controller还有$scope，完全组件化

* 


