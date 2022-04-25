[TOC]



## length

获取字符串长度

## indexOf

`indexOf(searchvalue，fromindex)` 在字符串中查找子字符串,如果存在则返回指定的子字符串值在字符串中首次出现的位置，否则返回 -1 

其中`searchvalue`（必需）规定需检索的字符串值。

`fromindex`可选的整数参数。规定在字符串中开始检索的位置。合法取值是 0 到 `stringObject.length - 1`。如省略该参数，则将从字符串的首字符开始检索。

## lastIndexOf()

返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。

## slice

`slice(start,end)` 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。

`start`  要抽取片断的起始下标。如果是负数，则规定从字符串的尾部开始算起的位置。-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。

`end` 要抽取片段的结尾下标。若未指定此参数，则要提取的是从 `start` 到原字符串结尾的字符串。如果是负数，则从字符串的尾部开始算起的位置。

```js
let myString = "hello kitty";
myString.slice(0,5)    // "hello"
myString.slice(6)    // "kitty"
myString.slice(3,-1)    // "lo kitt"
```

## concat() 

用于连接两个或多个字符串。返回连接后的新字符串。

## split()

用于把一个字符串分割成字符串数组

```js
let myString = "Hello kitty";
myString.split("");    // ["H", "e", "l", "l", "o", " ", "k", "i", "t", "t", "y"]
myString.split(" ");    // ["Hello", "kitty"]
 
let str2 = "23:34:56:78";
str2.split(":",3)     // ["23", "34", "56"]
 
let str3 = "How,are,you,doing,today?"
str3.split(",")    // ["How", "are", "you", "doing", "today?"]
```

## replace()

用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

```js
let myString = "Hello kitty";
myString.replace(/kitty/,"world")    //  "Hello world"
 
let name = "Doe, John";
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");    // "John Doe"
```

## match()

可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配

```js
let myString = "hello kitty";
myString.match("hello");    //hello
myString.match("Hello");    //null
 
let str = "2 plus 3 equal 5"
str.match(/\d+/g)    //["2", "3", "5"]
```

## toUpperCase() 

把字符串转换为大写。

## toLowerCase() 

把字符串转换为小写。

## charAt() 

返回指定索引的字符

```js
var name = 'hello world';
name.chartAt(0);           // "h"
```

