title: javascript继承机制
date: 2015-05-01 10:43:27
tags: javascript
---



Javascript是class-free的语言，靠原型链（prototype chain）模式，来实现继承。
使用new关键字来创建一个对象，和java语言不同的时，new命令后面跟的不是类，而是构造函数（其实就是一个普通的function）.

<!-- more -->


# 对象创建

```
//构造函数
function Fn(value){
　　this.value = value;
}
//new

var obj1 = new Fn('value1');
var obj2 = new Fn('value2');
alert(obj1.value); //
alert(obj2.value); //
```

如果有想共享的属性怎么办呢，就需要用到通过原型链(prototype chain)实现的原型继承了。

# prototype 和\_\_proto__

- 每个对象都有`__proto__`属性
- `__proto__`总是指向其构造函数的prototype方法，原型链就是是通过`__proto__`属性查找实现的
- 只有function有prototype属性, 这个属性是由Function的属性，而function可以理解为 

```
var fn = new Function("arg1", "arg2", "return arg1 * arg2;");
fn(2, 3);
```

假设有如下构造函数Foo和对象a
```
function Foo(value)
{
this.value = value;
}

var a = new Foo(20);


console.dir(a); //可以看到 __proto__属性

console.dir(a.constructor.prototype === a.__proto__);  //  true



console.log(
   	 (new Foo).__proto__ === Foo.prototype

   	 ,(new Foo).prototype === undefined

   	 );
```

#原型继承的用法

了解了prototype和__proto__的区别后，那原型继承该怎么用呢？

```
//构造函数
function Dog(){
this.bark = function(){
console.log("i'm a puppy");
}
}
function Cat(){
this.miaow = function(){
console.log("i'm a kitty");
}
}

function Animal(name,age)
{
this.eat = function(){
console.log("i can eat!");
}
}
Dog.prototype = new Animal;
Cat.prototype = new Animal;
var dog = new Dog();
var cat = new Cat();

cat.eat();
```


# constructor
constructor始终指向创建当前对象的构造函数，定义prototype会覆盖constructor，需要重新指定。


```
	var dog1 = new Dog();

	 //没设定prototype前
	 console.dir(Dog.prototype.constructor === Dog);
	 console.dir(dog1.constructor === Dog);

	 Dog.prototype = new Animal;

	 var dog2 = new Dog("ddd");
	 console.dir(Dog.prototype.constructor === Animal);
	 console.dir(dog2.constructor === Animal); 
	 //设定prototype后，dog2的constructor也变成了Animal,这不是我们想要的，我们只是想继承Animal的方法和属性
	 //因此需要手动修改

	 Dog.prototype.constructor = Dog;
	 console.dir(Dog.prototype.constructor === Dog);
	 console.dir(dog1.constructor === Dog);
	 
```

#references

有点多，但是都比较经典。

http://www.mollypages.org/misc/js.mp
http://stackoverflow.com/questions/9959727/proto-vs-prototype-in-javascript
http://stackoverflow.com/questions/7688902/what-is-functions-proto
http://stackoverflow.com/questions/650764/how-does-proto-differ-from-constructor-prototype
http://dmitrysoshnikov.com/ecmascript/javascript-the-core/
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto
http://javascript.crockford.com/prototypal.html
http://ejohn.org/blog/simple-javascript-inheritance/
http://dean.edwards.name/weblog/2006/03/base/
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain
http://www.cnblogs.com/sanshi/archive/2009/07/08/1519251.html