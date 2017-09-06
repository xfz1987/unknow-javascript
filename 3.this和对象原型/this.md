# 理解this
> 人们很容易把this理解成指向函数自身，但this有时并不像我们认为的那样指向函数/对象本身
> this的绑定和函数生命的位置没有任何关系，之取决于函数的调用方式(也就是说，this是在运行时绑定的-看谁调用的函数this就是谁，并不是在编写时绑定)
```
function foo(){
    //'use strict' TypeError, this is undefined
    console.log(this.a); // this就是我，我就是window
}
var a = 2;
foo(); // 相当于 window.foo();那么foo里面的this显而易见了吧
```
> 隐式绑定 this
```
function foo(){
    console.log(this.a);//obj调用了foo，那么this就是obj
}
var obj = {
    a: 2,
    foo: foo
}
obj.foo();//2 
```
> 注意: 有些写法会造成*隐式丢失*
```
//code1
function foo(){
    console.log(this.a); //this 指向 window
}
var obj = {a:2, foo: foo};
var bar = obj.foo; // 注意看这，只是给foo方法换了一个别名
var a = '妞儿';
bar();// 妞儿 , window.bar();
//code2, 回掉函数形成了隐式丢失
function foo(){
    console.log(this.a); //this 指向 window
}
function doFoo(fn){
    fn(); // window.fn()
}
var obj = {a:2, foo: foo};
var a = '娘们儿';
doFoo(obj.foo); //娘们儿  window.doFoo(obj.foo); obj形成了一个别名
```
> 显式绑定 this
> 通过call，apply实现
> call和apply的区别：call的第二个参数可以是任意的，apply的参数必须是数组
> A.call(B,a,b,c,[d,e])  -   A.apply(B,[a,b,c])
```
function foo(x,y){
    console.log(x,y,this);
}
foo();//undefined undefined window
foo.call(100,1,2);//1,2,Number(100);
foo.apply(true,[3,4]);//3,4,Boolean(true)
foo.apply(null);//undefined,undefined,window
foo.apply(undefined);//undefined,undefined,undefined
```

```
function foo(){
    console.log(this.a);
}
var obj = {a:2};
foo.call(obj);//2  A.call(B) 调用foo时强制将A的this绑定到B上
```
> 可惜啊，可惜啊，显式绑定仍然无法解决丢失绑定的问题
```
function foo(){
    console.log(this.a);
}
var obj = {a:2};
var bar = function(){
    foo.call(obj);
}
bar();//2;
setTimeout(bar, 100);//2
bar.call(window);//2 
```
## new机制
> 在传统的面向类的语言中，*构造函数*是类中的特殊方法，使用new初始化类时会调用类中的构造函数，通常形式是这样的
`something = new MyClass(..)`
> 然而，js中new的机制实际上和面向类的语言完全不同
> 使用new时被调用的函数并不属于某个类，也不会实例化一个类，实际上并不存在所谓的*构造函数*，只有对于函数的*构造调用*，使用new来调用函数，或者说发生构造函数调用时，会自动执行一下操作
```
1.创建(或者说构造)一个全新的空对象
2.这个新对象会被执行[原型]连接，初始化对象中的属性和方法
3.这个新对象会绑定到函数调用的this(设置当前对象的_proto_,指向自己的原型)
4.如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象
```
## 绑定this的优先级
显式绑定 > 隐式绑定
```
function foo(){
    console.log(this.a);
}
var obj1 = {a:2,foo:foo};
var obj2 = {a:3,foo:foo};
obj1.foo.call(obj2);//3
obj2.foo.call(obj1);//2
```
显式绑定 > 隐式绑定
```
function foo(something){
    this.a = something;
}
var obj1 = {foo:foo};
var obj2 = {};
obj1.foo(2);
console.log(obj1.a);//2
obj1.foo.call(obj2, 3);
console.log(obj2.a);//3;
var bar = new obj1.foo(4);
console.log(obj1.a);//2
console.log(bar.a);//4
```
new绑定 > 显式绑定
```
function foo(something){
    this.a = something;
}
var obj1 = {};
var bar = foo.bind(obj1);// A.bind(B),会创建一个新的包装函数，A会将B绑定到A中的this
bar(2);
console.log(obj1.a);//2
var baz = new bar(3);
console.log(obj1.a);//2
console.log(baz.a);//3
```
## 判断this
> 1.函数是否在new中调用? var bar = new foo(); this绑定的是新创建的对象
> 2.函数是否通过call、apply(显示绑定)或硬绑定调用? var bar = foo.call(obj); this绑定的是指定的对象obj
> 3.函数是否在某个上下文对象中调用(隐式绑定)? var bar = obj.foo(); this绑定的是那个上下文对象obj
> 4.如果都不是的话，使用默认绑定到undefined，否则绑定到全局对象 var bar = foo();
> 5.对于闭包，有时候this

## 被忽略的this
> 如果把null或undefined作为this的绑定对象传入call、apply或bind，这些值在调用时会被忽略掉，实际应用的是默认绑定规则
```
function foo(){
    console.log(this.a); // this -> window
}
var a = 2;
foo.call(null);// 2
```
> 那么什么时候传入 null 呢？
```
function foo(a,b){
    console.log(a,b);
}
foo.apply(null, [2,3]);//把数组展开成参数 2,3
var bar = foo.bind(null, 2); //使用bind进行柯里化
bar(3);// 2,3 ?????
```
## 安全的this
```
function foo(a,b){
    console.log(a,b);
}
var obj = Object.create(null);//创建一个空对象
foo.apply(obj, [2,3]);//2,3
var bar = foo.bind(obj, 2);
bar(3);// 2,3
```
## 间接引用
```
function foo(){
    console.log(this.a);
}
var a = 2;
var o = {a:3,foo:foo};
var p = {a:4};
o.foo();//2
(p.foo = o.foo)();//2 p.foo = o.foo => 返回的是目标函数foo的引用，也就是window.foo();
```