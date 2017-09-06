# 设计模式
## 工厂模式
```
【案例1】
function duckFn0(name, color, sound) {
  var duck = new Object();
  duck.name = name;
  duck.color = color;
  duck.sound = sound;
  duck.conFn = function () {
    console.log(this.color + this.name + this.sound);
  };
  return duck;
}
var duck0 = duckFn0("灰鸭子", "灰色的", "嘎嘎");
var duck1 = duckFn0("白鸭子", "白色的", "哼哼");
duck0.conFn();//灰色的灰鸭子嘎嘎
duck1.conFn();//白色的白鸭子哼哼
【案例2】
var factiory = {};
factiory.produceClothes = function(){
  this.workers = 50;
  console.log("我们有" + this.workers + "个工人");
};
factiory.produceShoe = function(){
  console.log("产鞋子");
};
factiory.transport = function(){
  console.log("运输");
};
//管理者：厂长
factiory.facManager = function(doSomething){
  return new factiory[doSomething]();
};
var me = factiory.facManager('produceClothes');
me.workers;//我们有50个工人
```
## 构造函数模式
```
var Foo = function(){
    this.name = 'xfz';
    this.getName(){
        console.log(this.name);
    }
}
```
## 原型模式
```
var Foo = function(){}
Foo.prototype = {};
```
## 混合模式
```
function duckFn3(name, color, sound){
    this.name = name;
    this.color = color;
    this.sound = sound;
    duckFn3.prototype.conFn = function(){
      console.log(this.color + this.name + this.sound);
    };
}
var duck6 = new duckFn3("蓝鸭子", "蓝色的", "哦哦");
var duck7 = new duckFn3("黄鸭子", "黄色的", "咳咳");
duck6.conFn();//蓝色的蓝鸭子哦哦
duck7.conFn();//黄色的黄鸭子咳咳
```
## 动态模式
```
function duckFn4(name, color, sound) {
   this.name = name;
   this.color = color;
   this.sound = sound;
   if (typeof this.conFn != 'function') {
     duckFn4.prototype.conFn = function() {
       console.log(this.color + this.name + this.sound);
     }
   }
}
var duck8 = new duckFn4("粉鸭子", "粉色的", "哧哧");
var duck9 = new duckFn4("橙鸭子", "橙色的", "噗噗");
duck8.conFn();
duck9.conFn();
```
## 单例模式
```
var Conf = (function(){
    var conf = {
        MAX_NUM : 100,
        MIN_NUM : 1,
        COUNT : 1000
    }
    return {
        get : function(name){
            return conf[name] ? conf[name] : null;
        }
    }
})();
var count = Conf.get('COUNT');//1000
```
### 惰性单例
```
var lazySingle = (function(){
    //单例实例引用
    var _instance = null;
    //单例
    function Single(){
        return {
            publicMethod : function(){},
            publicProperty: '1.0'
        }
    }
    //获取单例对象接口
    return function(){
        if(!_instance) _instance = Single();
        return _instance;
    }
})();
lazySingle().publicProperty;//1.0
```