#### AngularJs控制器

* 控制器的基本写法

```
angular.module('app',[]).controller('myController',function($scope) { $scope.name = 'hello world' })
```

* \*控制器应该尽可能短小精悍，在控制器中进行DOM操作和数据操作则是一个不好的实践，通过项目中的实践，我们应用一般会将复杂的逻辑放到指令和服务中心。轻量且更易维护的控制器看起来像这样:

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

#### AngularJs服务

1. 什么是服务？

* 控制器只是关心视图是如何同$scope绑定子啊一起的，以及如何管理数据的。出于内存占用和性能的考虑，控制器只会在需要时被实例化，并且不再需要就会被销毁。这就意味着每次切换路由或重新加载视图时，当前控制器会被AngularJs清除掉。

* 服务就不一样，它在AngularJs中是一个单例对象，每个应用中只会被实例化一次，这样就为我们提供能在应用的整个生命周期内保持数据的方法，它能够在控制器切换时进行通信并保持数据的一致性。

* 上面在控制器中就用到了MyService这个单例服务，通过AngularJs依赖注入的方式提供给控制器，这样控制器就可以使用MyService的所有属性，方法。

* 如何注册一个服务  

> 在AngularJs应用中，factory\(\)方法是用来注册服务的最常规方式。

-

