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

2. 控制器只是关心视图是如何同$scope绑定子啊一起的，以及如何管理数据的。出于内存占用和性能的考虑，控制器只会在需要时被实例化，并且不再需要就会被销毁。这就意味着每次切换路由或重新加载视图时，当前控制器会被AngularJs清除掉。

3. 服务就不一样，它在AngularJs中是一个单例对象，每个应用中只会被实例化一次，这样就为我们提供能在应用的整个生命周期内保持数据的方法，它能够在控制器切换时进行通信并保持数据的一致性。

4. 上面在控制器中就用到了MyService这个单例服务，通过AngularJs依赖注入的方式提供给控制器，这样控制器就可以使用MyService的所有属性，方法。

5. 如何注册服务

> 在AngularJs应用中，factory\(\)方法是用来注册服务的最常规方式。
>
> 一共有5种方法来创建服务：1.factory\(\) 2.service\(\) 3.constant\(\) 4.value\(\) 5.provider\(\)

常用三种创建服务的方法

##### 1.factory\(\)

```
factory()方法是创建和配置服务最快捷方式。factory()函数接受两个参数：name（字符串） Fn（函数） 这个函数会在AngularJs
创建服务实例时被调用。Fn这个函数是可以返回简单类型，函数，乃至对象等任意类型的数据
```

```
angular.module('MyApp', [])
    .factory('myService', ['$http', function($http) {
        return {
            getData: function(url) {
                return $http.get(url).then();
                 //.....
            }
        }
    }]);
```

##### 2.service\(\)

> 使用service\(\)可以注册一个支持构造函数的服务，它允许我们为服务对象注册一个构造函数。
>
> service\(\)方法和factory\(\)方法区别不大，接受两个参数:name\(字符串，要注册的服务名称\)、constructor\(构造函数，我们调用它来实例化服务对象\)。我们用service重写上面的myService

```
angular.module('MyApp', [])
    .service('myService', ['$http', function($http) {
            this.getData = function(url) {
                return $http.get(url).then();
                 //.....
        }
    }]);
```

##### 3.provider\(\)

所有服务工厂都是由$provide服务创建的，$provide服务在运行时初始化这些提供者。

\***提供者是一个具有$get\(\)方法的对象**\*，$inject通过挑用$get方法创建服务实例。所有创建服务的方法都构建在provider方法智商。provider\(\)方法负责在$providerCache中注册服务。

```
angular.module('myApp', [])
    .provider('myService', {
        $get: function() {
            return {
                'name': 'zwq'
            }
        }
    });
```

factory版本

```
angular.module('myApp', [])
    .factory('myService',function() {
       return {
          'name' : 'zwq'
       }
    });
```

那什么时候使用provider\(\)方法

* 当我们需要用AngularJs的.config\(\)函数来对.provider\(\)方法返回的服务进行额外的扩展配置。同其他创建服务的方法不同，config\(\)方法可以被注入特殊的参数。

* 当我们希望在应用开始前就对服务进行配置的时候而不是service/factory是第一次注入时才会初始化时就需要使用到provider\(\)。

```
var myApp = angular.module('myApp', []);
myApp.config(function($provide) {
    $provide.provider('greeting', function() {
        this.$get = function() {
            return function(name) {
                alert('hello', +name);
            }
        }
    })
})
```

这里即使我们发现不注入这个provider，但是它也是会进行实例化的。

#### 控制器和服务之间的交互

* 一般我们控制器总是通过service获取数据，而在conteoller中出现的逻辑都是和view相关的逻辑，比如loading，控制显示隐藏等等

* 最佳实现，直接建一个service，不要用什么事件，controller积累多的话很难维护，\***直接在service中提供getter和setter存取的方法**

* 每个controller依赖service，统一的service.setter\(...\)在改完数据后可以$emit\('upDated'\)，每个controller里$on\('upDated',function\(\) { $scope.data = service.getData\(\) }\)



