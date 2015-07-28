title: "Angular的factory/service/provider"
date: 2015-05-27 15:55:13
tags: 
- angular
- javascript
---



#Angular的factory/service/provider
SERVICE是为了实现多个controller之间共享的数据和方法。
根据不同的使用场景，angular提供了三种SERVICE, SERVICE的实现都是singleton的。
SERVICE包括factory, service, provider ,value, constant都是基于provider实现的。

<!-- more -->
#factory

``` javascript
//factory
myApp.factory('myFactory', function(){

        var services = {};
        services.doStuff = function()
        {
            return "factory call";
        }
        return services;

});
```

#service
是的，这个SERVICE就叫service;        
需要注意的是servic中的变量需要用this的方式定义。

``` javascript
//service
myApp.service('myService', function(){

    this.doStuff = function()
    {
        return "service call";
    }
});
```


#provider

provider是其他service的基础，是最复杂、最可配置的。`Providers are the only service you can pass into your .config() function.`

``` javascript
//provider
myApp.provider('myProvider',function(){
 
    var services = {};
    var configValue = "Default Value";
    this.SetValue = function(value){
        configValue = value;
    }
    services.doStuff = function()
    {
        return "provider call with config value: " + configValue;
    }
    this.$get = function()
    {
        return services;
    }
});
 
//config for provider
myApp.config(['myProviderProvider',function(myProvider) {
    myProvider.SetValue("hello provider");
}]);
```
#使用SERVICE

在controller中注入：

``` javascript

        myApp.controller('serviceExampleController', ['$scope','myProvider','myFactory','myService', function($scope,myProvider,myFactory,myService){
 
        $scope.value = "hi there";
        $scope.service =  myService.doStuff();
        $scope.factory = myFactory.doStuff();
        $scope.provider = myProvider.doStuff();
        }]);
```

#factory/service/provider区别

``` javascript

var fn = function(){
    var value = "value created without this";  //will not in service  
    this.value = "this is return from service"; //in service
    this.$get = function(){
        return "this return from provider";
    }
    return "this is return from factory";
}
app.factory('factory', fn);
app.service('service', fn);
app.provider('provider', fn);
```

factory, service, provider中存储的内容都是什么呢： 
- factory 返回执行fn的结果
- service 返回实例化(new fn())fn的结果
- provider 返回实例化fn, 然后执行$get方法的返回结果


``` javascript

console.log(new fn());
console.log(myService);

console.log(fn());
console.log(myFactory);

console.log(new fn().$get());
console.log(myProvider);
```

由于在Angular中，SERVICE都是Singleton的，因此SERVICE只会被实例化一次，并且缓存起来，假设这个缓存叫做cache, 那么在SERVICE被第一次注入时

``` javascript

cache.factory = fn()
cache.service = new fn()
cache.provider = (new fn()).$get()

```



#参考
http://angular-tips.com/blog/2013/08/understanding-service-types/
https://docs.angularjs.org/guide/providers




