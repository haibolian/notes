[TOC]



## 1. 函数没有重载

JavaScript中，函数没有重载

```javascript
function func(num1){
    console.log(num1+1);
}
function func(num1,num2){
    console.log(num1+num2);
}
func(12);
```

这个例子声明两个函数名相同的函数，参数数量不同，JavaScript不会因为参数的个数而重载函数，JavaScript中没有函数重载的概念。如果代码中声明了两个同名的函数，则后面的函数覆盖前面的函数。

## 2. 函数声明与函数表达式

定义函数有如下三种方式：

```javascript
function func(){};  // 函数声明
var func = function(){}; // 函数表达式
var func = new Function("num1","num2","return num1+num2") // Function构造函数
```

其中，使用function构造函数可以接收任意数量的参数，但最后一个参数始终被看成函数体，从技术角度讲，这是一个函数表达式，但不推荐这种方法定义函数。但这种方法非常有助于理解“ 函数是对象，函数名是指针 ”这一概念。

> 注意：函数名仅仅是指向函数的指针，所以函数名与包含对象指针的其他变量没有什么不同。

-----------------------

接下来看看函数声明与函数表达式的区别：

> 在代码解析时，解析器会率先读取函数声明，并使其在执行任何代码之前可用；至于函数表达式，必须等到解析器执行到它所在的代码行，才会真正被解释执行

就像变量声明提前一样

```javascript
console.log(a);
var a=10;
```

*误区：之前一直把函数声明和函数表达式记错，以为用类似声明变量的这种方法来定义函数是函数声明。结果它是函数表达式。。。*

```javascript
console.log(func(10,20));
function func(num1,num2){
	return num1+num2
}
```

以上代码完全正常运行，在代码开始执行之前，解析器就已经通过一个名为 函数声明提升 的过程，读取并将函数声明添加到执行环境中。

## 3. 理解参数

ECMAscript 函数的参数与大多数其他语言中的函数的参数有所不同。ECMAscript 函数不介意传递进来多少参数，也不在乎传递进来的是什么数据类型。**即使你定义的函数只接收两个参数，调用函数是也未必一定要传递两个参数。**

在函数体内，可以通过arguments对象来访问每一个参数。arguments是一个类数组对象，arguments[0]代表第一个参数，arguments[1]代表第二个参数，以此类推。当然，arguments也有length属性来确定传递进来多少个参数。

命名参数可以和arguments一起使用，第一个参数与 arguments[0] 的值相同，因此他们可以互换使用。**arguments的值永远与对应的命名参数的值保持同步。**如下实例：

```javascript
function func(num1,num2){
	arguments[0]=10;
	console.log(num1);
}
```

执行这个函数之后，会重写第一个参数。**这并不意味着arguments[0]和num1的值使用相同的内存空间**，它们的内存是独立的，但它们的值会同步。

> 注意：如果只传入一个参数，那么为arguments [1] 设置的值不会反映到命名参数中。这是因为 arguments 对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。

如下实例，

```javascript
function func(num1,num2){
    arguments[1]=10;
    console.log(num2);
}
func(12);   //undefined

function func(num1,num2){
    arguments[0]=10;
    arguments[1]=10;
    console.log(num1);//undefined
    console.log(num2);//undefined
}
func();     
```

> 没有传递值的命名参数将自动被赋予 undefined 值。
>
>  严格模式对如何使用arguments对象对出了一些限制。首先，像前面的例子中那样的赋值会变得无效。也就是说，即使把 arguments[0] 设置为10，num1 的值仍然还是undefined。其次，重写arguments的值会导致语法错误。

## 4. 作为值的函数

在ECMAscript中，函数名本身就是变量，所以函数也可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。

```javascript
function callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument);
}
function add10(num){
    return num + 10;
}
var result1 = callSomeFunction(add10, 10);
alert(result1);   //20

function getGreeting(name){
    return "Hello, " + name;
}
var result2 = callSomeFunction(getGreeting, "Nicholas");
alert(result2);   //Hello, Nicholas
```

## 5. 函数的内部属性

在函数内部，有两个特殊的对象：arguments 和 this。

### 5.1 arguments.callee

其中arguments是一个类数组对象，里面包含传入函数的所有参数。另外，arguments还有一个名为 callee 的属性，这个属性是一个指针，指向拥有这个arguments对象的函数。

在递归函数中，函数通过名字调用自身。如下经典的阶乘实例：

```javascript
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * factorial(num-1);
    }
}
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4));  //error!
```

如果将函数名称换为其他名称，则在函数内部调用 factorial() 会显示失败，因此，此处应使用 指向正在执行函数的指针 。如下所示：

```javascript
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num-1);
    }
}
```

> 注意：虽然使用arguments.callee比使用函数名更为保险，但在严格模式下，不能通过脚本访问arguments.callee 

针对上述注意问题所修改的代码：

```javascript
function factorial = (function f(num){
    if (num <= 1){
        return 1;
    } else {
        return num * f(num-1);
    }
});
```

创建一个名为 f() 的命名函数表达式，然后将它赋值给变量factorial 。即便把函数赋值给另一个变量，f() 仍然有用。这种递归方式最佳。 

### 5.2 this

this引用的是函数执行的环境对象。

1. 当以函数的形式调用时，this是window
2. 当以方法的形式调用时，谁调用方法，this就是谁
3. 当以构造函数调用时，this就是新创建的那个对象

this:谁调用的，this就指向谁，例如：

​	obj.func();--------this指向obj

​	fun()-----------this指向window

### 5.3 caller

caller中保存着调用当前函数的函数的引用，如果是全局作用域中调用当前函数，它的值为null。

```javascript
function outer(){
    inner();
}
function inner(){
    alert(inner.caller);
}
outer();
```

outer中调用 inner ，inner.caller 为调用inner的outer的引用，所以最后输出的是outer函数的源代码。为了实现更松散的耦合，可以使用arguments.callee 来代替inner ，即指向拥有这个arguments对象的函数。当然，在严格模式下，访问arguments.callee 会导致错误

## 6. 函数的属性和方法

### 6.1 属性--length

length属性表示函数希望接收的命名参数的个数

```javascript
function func1(){
	alert("hh");
}
function func2(num1){
    var aa="123";
    alert(num1)
}
function func3(num1,num2){
    alert(num1+num2)
}
alert(func1.length);    //  0
alert(func2.length);    //  1
alert(func3.length);    //  2
func1().length;    //  hh + error
console.log(func2.aa)  // 这样做是行不通的，aa不是属性，是定义的变量。通过全局直接去访问函数里的变量是不行的。需要通过闭包提供对外“访问权限”才行
```

> 注意：在使用函数的属性的时候，函数名后不能带有括号。带括号则意味着执行这个函数。
>

虽然在执行func1().length时，先输出的是hh，后面接着输出了一个错误。因为执行完func1之后，得到的结果是空，空.length，所以没有length这个属性。

### 6.2 属性--prototype

后面更新原型、原型链文章中提到。

### 6.3 方法

每个函数都包含两个非继承而来的方法：apply()和call()。 call和apply都是函数对象的方法，需要通过函数对象来调用。

- 对函数调用call()和apply()时都会调用函数执行

```javascript
function fun1(){
    console.log("我是一个函数");
}
fun1();		  //我是一个函数 
fun1.call();  //我是一个函数 
fun1.apply(); //我是一个函数 
```

执行上述代码的结果将会输出三个 ”我是一个函数 “ ，也就是调用call和apply时会调用函数执行，尽管函数名后面没有加括号。

- 调用call和apply时可以将对象指定为第一个参数，此时这个对象会成为函数执行时的this。

```javascript
function fun2(){
    console.log(this.name);
}
var name="猪八戒";
var obj={
name:"孙悟空",
age:"15"
}
fun2();    	 // 猪八戒
fun2.call(); // 猪八戒
fun2.call(obj);  // 孙悟空
fun2.apply();	 // 猪八戒
fun2.apply(obj); // 孙悟空
```

- call和apply的唯一区别是接收参数的方式不同，其中apply的第二个参数可以是一个数组，也可以是arguments对象，例如：

```javascript
function sum(num1, num2){
    return num1 + num2;
}
function callSum1(num1, num2){
    return sum.apply(this, arguments);   //传入arguments对象
}
function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]); //传入数组
}
```

​	对于传入数组而言，看下面的例子或许更好一些

```javascript
var obj={
name:"孙悟空",
age:"15"
}
function fun3(a,b){
    console.log("a="+a);
    console.log(("b="+b));
}
var m=4;
var n=5;
fun3(1,2);    // a=1 b=2 
fun3.call(obj,obj.name,obj.age);  // a=孙悟空  b=15
fun3.apply(obj,[obj.name,obj.age]);  //a=孙悟空 b=15
```

