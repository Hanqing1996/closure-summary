# closure-summary
1. 我不打算用“函数上下文”，“AO”对象那一套来解释，太繁琐。
2. 做题的知识点：变量提升（智障 var）函数预解析和运行,具体参考[这里](https://github.com/Hanqing1996/JavaScript-advance/tree/master/%E4%BD%A0%E7%9C%9F%E7%9A%84%E6%87%82%E5%87%BD%E6%95%B0%E5%90%97)
3. 注意函数的参数相当于在函数内声明的变量

#### 题目
```
var data = [];

for (var i = 0; i < 3; i++) {
    data[i] = function () {
        console.log(i);
    };
}

data[0]();
data[1]();
data[2]();
```
具体过程
```
var data
data=[]

var i // A1
i=0 // A1处声明的变量i被赋值为0
data[0]=function () { console.log(i)};// 这里访问的i是A1处的i

//  var i; 根据变量提升的原则，同一作用域下的重复声明无效
i=1 // A1处声明的变量i被赋值为1
data[1]=function () {console.log(i)}; 
i=2 // A1处声明的变量i被赋值为2
data[2]=function () {console.log(i)};
i=3 // A1处声明的变量i被赋值为3


data[0] 运行,距离data[0] 运行前,A1处声明的i最近的值为3,所以输出3
data[1] 运行,距离data[1] 运行前,A1处声明的i最近的值为3,所以输出3
data[2] 运行,距离data[2] 运行前,A1处声明的i最近的值为3,所以输出3
```

#### 题目
```
var data = [];

for (var i = 0; i < 3; i++) {
    data[i] = (function (i) {
        return function(){
            console.log(i);
        }
    })(i);
}

data[0]();
data[1]();
data[2]();
```
答案
```
0
1
2
```
具体过程
```
var data
data=[]

var i // A1
i=0 // A1处的值被赋值为0

/* 第一个匿名函数 */
function (i) {...} 声明
var i // B1

function (i) {...} 运行
i=0 // B1处声明的i被赋值为0
return function(){console.log(i);} // 访问的是 B1 处声明的变量i

data[0]=function(){console.log(i);} // i是B1处的i

// var i,无效。根据变量提升的原则（同一作用域下重复声明无效）

i=1 // A1处的i被赋值为1

/* 第二个匿名函数 */
function (i) {...} 声明
var i // B2

function (i) {...} 运行
i=1 // B2处声明的i被赋值为1
return function(){console.log(i);} // 访问的是 B2 处声明的变量i

data[1]=function(){console.log(i);} // i是B2处的i

// var i,无效。

i=2 // A1处的i被赋值为2

/* 第三个匿名函数 */
function (i) {...} 声明
var i // B3

function (i) {...} 运行
i=2 // B3处声明的i被赋值为2
return function(){console.log(i);} // 访问的是 B3 处声明的变量i

data[2]=function(){console.log(i);} // i是B3处的i


data[0]声明，发现没啥好声明的

data[0]运行，输出此时B1处的i,值为0
data[1]运行，输出此时B1处的i,值为1
data[2]运行，输出此时B1处的i,值为2
```
