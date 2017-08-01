### AngularJs中$watch,$apply,$digest的理解

* 我们都知道AngularJs的数据绑定过程叫做:dirty-checking\(脏检查\)，他们是如何工作的呢

##### $watch队列

* 每次在UI上绑定一些东西的时候就会往$watch队列中插入一条$watch。$watch可以想象成可以检测它监视的model变化的东西。比如

```
User: <input type="text" ng-model="user" />
Password: <input type="password" ng-model="password" />
```

> 这里我们就在$watch队列里面加入了两个$watch，$scope.user和$scope.password,但是在使用ng-repeat的时候要格外注意，看下面：

```
<ul>
  <li ng-repeat="person in people">
      {{person.name}} - {{person.age}}
  </li>
</ul>
app.controller('MainCtrl', function($scope) {
  $scope.people = [...];
});
```

> UI上用了ng-repeat，每个person绑定了两个$watch\(一个是name,一个是age\)，ng-repeat也是一个watch，如果10个person就会有2\*10+1个$watch，这里处理不好就会导致性能的下降，毕竟每次$digest的时候会遍历所有的$watch的。

##### $digest循环

* angular context处理事件的时候，$digest循环就会触发，这个循环是由两个更小的循环组合起来的，其中一个会处理我们的$watch队列，$digest将会遍历我们的$watch。然后更新绑定的UI实现DOM刷新。
* 这就是所谓的dirty-checking，会遍历直到所有的$watch都没有变化。

##### $apply决定什么事件进入angular context

* 如果当事件触发时，你调用$apply就会进入angular context，比如ng-click，ng-model操作中事件就已经被封装到一个$apply调用，所以你在这些类似操作中不需要手动调用$apply来进入angular context

* 一种很常见的情况，刚入门的新手在使用jquery时候说，为啥不会更新我绑定的东西呢，这就是因为jQuery没有调用$apply，事件没有进入angular context，脏检查就永远不会执行，但是你所绑定的数据已经改变了，只是脏检查没有调用，DOM没有更新罢了。例子：[https://codepen.io/MrZwqShuai/pen/oexaYO](https://codepen.io/MrZwqShuai/pen/oexaYO)

> 我们在点击蓝色区域的时候，并没有像我们期望的那样改变数字0，当我们点击changehello按钮的时候就会发现蓝色区域的数字发生改变了，改变后的数据是你之前点击蓝色区域时积累的。这下应该明白了吧。



