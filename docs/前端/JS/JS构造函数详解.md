# JS构造函数详解

## 一、js 函数

　　首先弄明白何为函数呢，按照犀牛书的说法，函数简单的说就是重复执行的代码块。函数是这样的一段JavaScript 代码，它只定义一次，但可能被执行或调用任意次。函数的定义方式：

### 声明式函数定义

 function  函数名 （）{}；这种定义方式，会将函数声明提升到该函数所在作用域的最开头，也是就无论你在这个函数的最小作用域的那儿使用这种方式声明的函数，在这个作用域内，你都可以调用这个函数为你所用。

### 函数表达式

let fun = function(){}; 此方式定义的函数，只能在该作用域中，这段赋值代码执行之后才能通过fun（）调用函数，否则，由于变量声明提升，fun === undefined。

### new Function 形式

 var fun1 = new Function (arg1 , arg2 ,arg3 ,…, argN , body  )；Function构造函数所有的参数都是字符串类型。除了最后一个参数, 其余的参数都作为生成函数的参数即形参。这里可以没有参数。最后一个参数, 表示的是要创建函数的函数体。

### 总结

>**1、第一种和第二种函数的定义的方式其实是第三种new Function 的语法糖，当我们定义函数时候都会通过 new Function 来创建一个函数，只是前两种为我们进行了封装，我们看不见了而已，js 中任意函数都是Function 的实例。**
>
>**2、ECMAScript 定义的 函数实际上是功能完整的对象。**

## 二、构造函数

### 定义

通过  new 函数名    来实例化对象的函数叫构造函数。任何的函数都可以作为构造函数存在。之所以有构造函数与普通函数之分，主要从功能上进行区别的，构造函数的主要 功能为 初始化对象，特点是和new 一起使用。new就是在创建对象，从无到有，构造函数就是在为初始化的对象添加属性和方法。构造函数定义时首字母大写（规范）。

### 对new理解

new 申请内存, 创建对象,当调用new时，后台会隐式执行new Object()创建对象。所以，通过new创建的字符串、数字是引用类型，而是非值类型。

```js
1、常用的构造函数： 
1.    var arr = [];     为      var arr = new Array();       的语法糖。
2.    var obj = {}     为      var obj = new Object();     的语法糖
3.　var  date = new Date();
4.    ...
2、执行构造函数时发生的事 ： let f = new Foo();
function Foo(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
Foo.prototype.belief = function(){
    console.log('量变是质变的必要准备，质变是量变积累到一定程度的必然结果！');
}
let f = new Foo ('zh',18,'男');
a .   let   f = {};   //一个继承自 Foo.prototype 的新对象被创建。
b.   f.__proto__ = Foo.prototype; // f 继承 Foo的原型。   
b   Foo.call(f,'zh',18,'男');    //执行Foo函数，将name,age,sex 参数传入Foo中执行，此时函数内部this 为 new 创建的 f对象，所以  f.name = 'zh';f.age = 18; f.sex = '男'；
c.  实例化对象完成，此时  f = {
    name:'zh',
    age:18,
    sex:'男'
}
d.   f.belief();     打印'量变是质变的必要准备，质变是量变积累到一定程度的必然结果！

手写new函数　　
function newTest (constructFunction){
    let obj = {};
    obj.__proto__ = constructFunction.prototype;
    return function(){
        constructFunction.apply(obj,arguments);
        return obj;
    }
}
```

### 注意

>**当构造函数中有返回对象时候，最终new出来的对象会是构造函数的返回值，而不是new过程中生成的对象。仅当构造函数返回值是对象时有效，当不是对象时依旧返回new过程中形成的对象（无论如何new 构造函数之后都会返回一个对象值）。**

## 三、ES6 中 class 与构造函数的关系

```js
class 为  构造函数的语法糖，即 class 的本质是 构造函数。class的继承 extends  本质 为构造函数的原型链的继承。
例如：
类的写法
class Person{  //定义一个名字为Person的类
    constructor(name,age){ //constructor是一个构造方法，用来接收参数
        this.name = name;  //this代表实例对象
        this.age = age;
    }　
    say(){  //这是一个类的方法，注意千万不要加上function
        return   this.name + this.age
    }
}
var obj = new Person('老铁'，18);
console.log(obj.say());
构造函数的写法
function Person(name,age){   //构造函数和实例化构造名相同且大写（非强制，但这么写有助于区分构造函数和普通函数）
    if(!(this instanceof Person)){ //避免使用者不小心讲Person当作普通函数执行
        throw new Error(''请使用 new Person"); //仿ES6 class 中的写法
                        }
    this.name = name;
    this.age = age;
}
Person.prototype.say = function(){
    return   this.name + this.age
}

var obj = new Person('老铁'，18);   //通过构造函数创建对象，必须使用new运算符
console.log(obj.say());
```



## 四、总结

>**通过 class定义的类 和 通过 构造函数定义的类 二者本质相同。并且在js执行时，会将第一种转化为第二种执行。所以 ES6 class的写法实质就是构造函数。**

