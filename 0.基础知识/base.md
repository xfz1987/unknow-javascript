# 变量
> 变量就是内存中存储数据的一块空间
> 一切数据必须存在变量中
> 声明：var 变量名-->在内存中开辟一块空间
> 赋值：变量名=值-->将值保存到左边的变量空间中备用
> 使用：使用变量名，等效于直接使用变量中的数据
> 注意: 对未声明的变量赋值：js会自动在全局创建该变量！
>值传递：将一个变量的值赋值给另一个变量，其实将原变量中的值，复制一份给新变量,js中一切赋值都是值传递！

# 数据类型间的转换
> 隐式转换：程序自动转换数据类型（坑）
> 弱类型：
> - 1. 变量声明时不必限定数据类型，今后可能保存任何类型数据
> - 2. 数据类型间可以自动类型转换
> - 3. 只要有字符串参与，一切类型都加""变为字符串
> - 4. 如果没有字符串，都转为数字计算：true-->1  false-->0
> 强制转换：程序员通过调用专门函数手动转换类型
> - String：x.toString()-->将x转为字符串类型
> - Number：Number(x)-->将任意类型转为number类型
> - string --> number 转为整数：var num=parseInt("str"),读取字符串中的整数部分
```
1. 从第一个字符向后读。
2. 如果碰到第一个数字字符，开始获取数字再次碰到不是数字的字符（包括.）停止读取
3. 如果开头碰到空格，忽略
4. 如果碰到的第一个非空格字符，不是数字，说明不能转-->NaN: Not a Number
```
> str -> float: 
> var num=parseFloat("str"),读取字符串中浮点数部分用法和parseInt完全相同
> 唯一差别：parseFloat认识小数点，仅认第一个

# NaN
> 不是数字(内容)的数字（类型）,和任何数字做比较，永远返回false甚至不等于它自己`NaN == NaN // false`
> isNaN(x):如果是NaN返回true；如果不是NaN返回false
> - 是数字返回false；如果不是数字返回true
> - 只要不能自动转换为数字的，都返回true
> - 只要能自动转换为数字的，都返回false

*凡是从页面上获得的数据，都是字符串！必须先转换再计算*
        
# 运算符
> 运算符：程序模拟人类运算的特殊符号
> 算数运算：任意类型数据做-，都会被转为数字类型
> 如果有参数，不能自动转为数字，则返回NaN
> 如果除数为0:Infinity-->无穷大
> typeof(x):判断任意数据的类型
> 被除数%除数——模运算：被除数/除数，取除不尽的余数(何时用：限制一个数不能超过某个最大范围时)
> 递增/递减运算：
> - ++: i++==>i=i+1; 只要遇到对变量+1，就用i++
> - i++单独用：++放前放后，效果一样:i++等效于++i;
> - i++出现在表达式内部：
> - 前++，先+1，再参与表达式
> - 后++，先用旧值参与表达式。表达式之后再+1

# 关系运算
> 判断大小！条件判断中使用,结果：成立：true，不成立：false
> *自带隐式类型转换*
> 字符串参与关系运算：
> - 从第一个字符开始，取出每个字符PK unicode编号
> - "Hello" H e(65)  "HI" H I(49)
> 关系运算中：任何类型和数字比较，都转为数字，再比较
> - false -> 0 true -> 1 "" -> 0 
> 布尔类型参与关系运算，始终转为数字比较
> *关系运算：先将参与判断的数据，强转为相同类型，再比较*
> undefined==null ==>true  undefined===null ==>false
> - 因为undefined类型,是从null类型继承来的
> - ===严格相等：不带自动类型转换的相等比较！类型和值必须都相等！才返回true。
```
总结：
1. 普通数据，先转为相同类型，再比较
2. undefined，就用===
3. NaN，就用isNaN(x)
```

# 逻辑运算
> 基于多组关系运算，得出1个结论
> && 并，所有条件为true，才返回true，否则返回false
> || 或，只要任意一个为true，就返回true，否则返回false
> !  否，颠倒true或false
## 短路逻辑
> 只要前一个判断足矣得出最终结论，则后续条件不执行！
> if(false && a>2) 根本就不会判断a>2,管你是个what
> if(true || a>2) 根本就不会判断a>2,管你是个what

# 位移
> - 左移：n<<m ==> n*(2的m次方),3<<3 ==>3*(2*2*2)
> - 右移：n>>m ==> n/(2的m次方),64>>3 ==> 64/(2*2*2)=8
> *小技巧: 给一个数字取整 12.12 >> 0 -> 12*

# 赋值运算
> 赋值运算结果就是等号右边表达式的结果,因此执行循序是从右往左来
```
var x=y=z=10;
        z=10
      y=10
    x=10
``` 
# 扩展的赋值表达式
> 对已有数据计算同时，将结果再存回变量
> i+=5;等效于i=i+5; (何时使用：修改原变量值)

# 三目运算
> 根据不同条件，动态返回不同结果，至少需要3个表达式
> 一个条件: 二选一
> ```
> 条件?当条件满足时返回的值:当条件不满足时返回的值
> ```
> 多个条件: 多选一
> ```
> 条件1?条件1满足时的值:
> 条件2?条件2满足时的值:
> 条件n?条件n满足时的值:
>               默认值;
> ```

# 程序
> 程序 = 数据结构+算法
> 良好的数据结构，可以极大提高程序的执行效率！

# 数组(array)
> 存储：连续存储多个数据的存储空间
> 使用：相当于多个变量的集合
> 创建：数组都是用[]创建出来的
> - var arr=[]; -->创建了一个数组对象，数组中包含0个元素
> - var arr=[91,true,“Hello”];-->创建了一个数组对象,数组中连续存储3个元素：91,61,95
> - 2个不限制：1. 不限制元素个数！2. 不限制元素数据类型
> *数组是引用类型的对象* 
> - 原始类型：数据保存在变量本地
> - 引用类型：数据不保存在变量本地！保存在“堆”中,由地址指向实际数据
> - 引用类型特点：可以保存多个数据，而且数据个数随时可变
```
arr.splice：删除！插入！替换！*直接修改原数组*！返回被删除的元素组成的新数组,
删除元素：arr.splice(start,count);
替换元素：arr.splice(start,count,值1,值2,...)
插入元素：arr.splice(start,0,值1,值2,...)
```
> 排序 arr.sort(): 默认按字符串升序排列
> 自定义排序:
> - 升序：arr.sort(function(a,b){return a-b;});
> - 降序: arr.sort(function(a,b){return b-a;});
> 栈和队列：
> - 栈: 后进的先出！（LIFO）,栈其实就是数组，只不过用一对儿方法模拟了栈的操作
> 入栈：arr.push()   出栈：var last=arr.pop()  
> push是从数组最后插入  pop是取出数组最后一个位置
> - 队列: 先进先出 (FIFO)
> 入队: arr.push()   出队: var first = arr.shift()
> unshift是从数组的开头插入， shift是取出数组的第一个位置     

# String包装类型
> 注意: 字符串内容一旦创建，不可改变
> API:  toLowerCase toUpperCase chatAt indexOf/lastIndexOf substr/slice/substring

# 类型检测
## typeof 适合基本类型及function检测，遇到null失效
> typeof null -> object
## 判断数组类型的方法
> - 1.instanceof  ->  a instanceof Array)
> - 2.constructor ->  a.constructor == Array
> - 3.特性判断法 算了，太恶心
> - 4.最简单方法: 通过原型判断
> 
```
//较为严谨并且通用的方法
function isArray(object){
    return object && typeof object==='object' && Array == object.constructor;
}
//最简单的方法
function isArray(o) {
    return Object.prototype.toString.call(o) === '[object Array]';
}
```

# 鄙视题：js中方法定义的方式有几种：3种！
```
    A. function compare(a,b){return a-b;}
    B. var compare=function(a,b){return a-b;}
    C. var compare=new Function("a","b","return a-b;")
```