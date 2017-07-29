#### AngularJs控制器

* 控制器的基本写法

```
angular.module('app',[]).controller('myController',function($scope) { $scope.name = 'hello world' })
```

* 控制器应该尽量精简，复杂业务逻辑交给服务，通过依赖注入与服务交互，复杂的DOM操作交给指令去操作

* 这里的控制器相当于mvvm中的viewmodel，作为视图与数据的桥梁

> Angular2+中剔除了控制器controller还有$scope，完全组件化



