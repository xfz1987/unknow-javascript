# 函数绑定
## bind函数
> - bind函数绑定: 在函数执行时更改作用域
>   其他方式: call、apply、eval、with
> - 函数柯里化: 将传递多个参数的函数，转化成一个一个参数传递的函数，并且在执行时可以传递剩余的参数，并得到结果，在函数式编程中，增加了函数的适用性
```
var obj = {
    title: 'aaa',
}
function demo(){
    console.log(arguments);
    console.log(this.title);
}
demo(1,2,3);//[1,2,3]  undefined
demo.call(obj, 1,2,3);//[1,2,3] aaa
demo.apply(obj,[1,2,3]);//[1,2,3] aaa
document.body.onclick = demo.call(obj, 1,2,3);
//我们一旦定义了，就执行了，显然我们还没有点击就执行了
//为解决这个问题，ES5新增了函数绑定功能 bind
document.body.onclick = demo.bind(obj, 1,2,3);
var newDemo = demo.bind(obj, 1,2,3);
newDemo(4,5,6);//1,2,3,4,5,6
```
## 实现bind方法
> 要模拟bind方法要做两件事情: 函数绑定、函数柯里化
```
/**
 * 函数绑定：函数做参数传递的同时，可以保存函数执行的作用域
 * @fn 函数名
 * @context 函数执行的作用域
 */
function bind(fn, context){
    return function(){
        fn.apply(context, arguments);
    }
}
document.body.onclick = bind(demo, obj);
```
```
/**
 * 函数柯里化
 * @fn 执行的函数
 * 从第二个参数开始表示定义的参数
 */
function curry(fn){
    var args = Array.prototype.slice.call(arguments, 1);
    return function(){
        var allArgs = args.contact(Array.prototype.slice.call(arguments));
        return fn.apply(null, allArgs);
    }
}
var a = curry(demo, 1,2,3);
a(4,5,6);// [1,2,3,4,5,6] undefined
```
*将两个功能整合*
```
/**
 * 实现函数绑定的方法
 * @fn  绑定的函数
 * @context 绑定的作用域
 * @从第三个参数 表示定义定义时传递的参数
 * return 表示新的执行函数
 */
function bind(fn, context){
    //缓存传递的参数
    var args = Array.prototype.slice.call(arguments, 2);
    return function(){
        //缓存参数合并执行时的参数
        var thisArgs = args.concat(Array.prototype.slice.call(arguments));
        return fn.apply(context, thisArgs);
    }
}
var demo1 = bind(demo, obj, 11,22,33);
demo1(55,66,77); // [11,22,33,55,66,77] aaa
```