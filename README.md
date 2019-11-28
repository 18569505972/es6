# es6基础知识  
## 解构赋值 
解构赋值右边不是可遍历的结构Iterator，则会报错。  
```
let [a,b,...c]=['a']
/*
  a: a 
  b: undefined
  c: []
*/

let [x=2,y=1]=[1]
/*
  x:1
  y:1
 */
// ES6通过内部严格相等符号===undefined来判断一个位置是否有值。
let [x=2,y=3]=[null,undefined]
/*
  x:null
  y:3
 */
// 默认值可以一个表达式
function f() {
  return 666;
}
let [a=f()]=[]
/*
  a:666
 */
```
数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。  
```
var {foo,bar}={foo:1,bar:2};
/*
  foo:1
  bar:2 
 */
// 对象解构赋值，先找到对应属性，再给赋值
var {foo:a,bar:b}={foo:1,bar:2}
/*
  a:1
  b:2 
 */
 // 嵌套赋值
 let obj = {};
 let arr = [];
 ({foo:obj.foo,bar:arr[0]}={foo:1,bar:2})
 /*
   obj:{foo:1}
   arr:[2] 
  */
```
基本值类型解构，会先将其转换为对象，undefined和null无法结构会报错。
```
let [a,b,c]='hello'
let {length:len}='hello'
/*
  a:h
  b:e
  c:l 
  len:5
 */
```
## 字符串的扩展
unicode优化，es6之前js只能识别\u0000——\uFFFF之间字符；es6可以通过添加花括号识别。
```
// es6之前
\uD842\uDFB7
// es6
\u{20BB7}
// "𠮷"
```
codePointAt:正确返回32位的UTF-16字符的十进制码点。  
字符串具有iterator接口，可被for...of遍历。  
fromCodePoint:正确解析32位的UTF-16字符的十进制码点。  
at：替代之前的charAt返回给定位置字符，可以识别码点大于0xFFFF的字符。  
```
// 可以通过for...of正确遍历32位UTF-16字符
var s = '𠮷a';
s.codePointAt(0).toString(16) // "20bb7"
s.codePointAt(2).toString(16) // "61"
for(let ch of s) {
  console.log(ch.codePointAt().toString(16))
}
// 20bb7
// 61
// 检测字符为四字节还是两字节，四字节返回true
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}
String.fromCodePoint(0x20BB7)
// "𠮷"
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
'𠮷'.charAt(0) // "\uD842"
'𠮷'.at(0) // "𠮷"
```
indludes(findStr,startIndex):返回布尔值，字符串是否包含某字符串。  
startsWith(findStr,startIndex):返回布尔值，字符串是否以某字符串开头。    
endsWith(findStr,endIndex):返回布尔值，字符串是否以某字符串结尾。  
repeat(n):返回新字符串，将某字符串重复n次。  
padStart(strLength,str):字符串不满足strLength长度，重复使用str从字符串头部补齐到strLength。   
padEnd(strLength,str):字符串不满足strLength长度，重复使用str从字符串尾部补齐到strLength。   
```
'adcd'.includes('c',2) // true
'abcd'.startsWith('c',3)  // false
'abcd'.endsWith('c',2)  // false
'abcd'.endsWith('b',2)  // true
'a'.repeat(3)   // aaa
'a'.padStart(5, 'abc') // abcaa
'a'.padEnd(5, 'abc') // aabca
```
模板字符串:\`${}\`    
String.raw():替换模板字符串变量，返回源字符串的转义字符串 。   
```
// 函数调用模板字符串
function temStr(...tem) {
  for(let i =0;i<tem.length;i++) {
    console.log(tem[i])
  }
}
let str = 'def';
let str2 = 'ij';
temStr`abc${str}gh${str2}`
/*
  ['abc','gh','']
  'def'
  'ij'
*/
//String.row()
let a = 'bcd';
String.raw`a\n${a}e` // 实际返回a\\nbcde，浏览器显示a\nbcde
String.raw`a\\n${a}e` // 实际返回a\\nbcde，浏览器显示a\\nbcde
```
## 数值扩展
Number.isFinite(num):判断数字num是否有限。  
Number.isNaN(num):判是否为NAN。  
Number.isInteger(num):判断是否为整数。（此方法判断int.0也为整数）。    
Number.isSafeInteger(num):判断整数num是否在js可识别数字区间内。  
指数运算符：2**3=8。  
## 数组扩展
Array.from(obj,mapLoop,this):可以将类数组（含有length属性）或者可遍历对象转换为数组。（es5写法：[].slice.call(obj)）   
Array.of(item1,item2,item3....)： 将一组数值​转换为数组。   
target.find(funcion(value,index,arr)=>{value>0}):返回target中第一个匹配值，没有匹配值返回undefined。  
target.findIndex((value,index,arr)=>{value>0}):返回target中第一个匹配值下标，没有匹配值返回undefined。     
arr.includes(value，searchIndex):数组中是否包含value。 
arr.fill(value,startIndex,endIndex)：使用给定值填充数组startIndex到endIndex位置（不包含endIndex）。  
arr.copyWithin(replaceIndex,startIndex,endIndex)：​用startIndex（默认为0）到endIndex（默认为arr.length）元素替换replaceIndex起的数据。   
arr.forEach((value,index,arr)=>{}):forEach不返回值，不改变原数组。   
arr.map((value,index,arr)=>{return value}):返回新数组，不能深复制。  
for...of:遍历可迭代对象。  
arr.filter((value,index,arr)=>{return value>0}):返回满足条件的元素组成的新数组。  
arr.every((value,index,arr)=>{return value>0}):判断是否每个元素符合条件，返回布尔值。  
arr.some((value,index,arr)=>{return value>0}):判断是否存在元素符合条件，返回布尔值。  
arr.reduce((preValue,currentValue,index,arr)=>{return preValue+currentValue},initialVal):数组值累加,initialVal初始值。  
keys，values，entriesf:返回遍历器对象，可被for...of遍历。  
## 函数扩展
es6可设置函数默认值。  
```
// 设置默认值的参数必须放于未设置默认值的后面
function foo(x,y=1){
  console.log(x,y)
}
foo(1)
/*
  1
  1
*/
function bar(x,y=1){
  console.log(x,y)
}
/*
  undefined
  1
*/
``` 
与解构赋值一起使用。  
```
function foo({x,y=1}) {
  console.log(x,y)
}
foo({})
/*
  x:undefined
  y:1
*/
foo()
// TypeError: Cannot read property 'x' of undefined
function foo(a,{b,c=1}) {
  console.log(a,b,c)
}
foo(1)
// TypeError: Cannot read property 'x' of undefined
foo(1,{})
/*
  a:1
  b:undefined
  c:1
*/
function foo(a,{b,c=1}={}) {
  console.log(a,b,c)
}
foo(1)
/*
  a:1
  b:undefined
  c:1
*/
```  
rest参数（只能为函数最后一个参数，否则报错）。  
```
function foo(a,...b){
  console.log(a,b)
}
foo(1,2,3,4,5)
/*
  a:1
  b:[2,3,4,5]
*/
```
length属性：返回参数个数（不包含有默认值的参数,rest参数）。  
name属性:返回函数名（匿名函数返回空）。  
扩展运算符，展开iterate对象。
```
// 合并数组
let a = [1,2];
let b = [3,4];
console.log([...a,...b])
// [1,2,3,4]
[...'hello']
// ['h','e','l','l','o']
var nodeList = document.querySelectorAll('div');
var array = [...nodeList];
```
函数柯里化。
```
// 通用函数
function createCurry(func, arg) {
    var arity = func.length;
    var args = arg || [];

    return function() {
        var _args = [...arguments,...arg];
        // 如果参数个数小于最初的func.length，则递归调用，继续收集参数
        if (_args.length < arity) {
            return createCurry.call(this, func, _args);
        }
        // 参数收集完毕，则执行func
        return func.apply(this, _args);
    }
}
function sum(a,b,c){
  return a+b+c
}
var curry = createCurry(sum,[1,2]);
curry(3)
// 6
// 面试经典题
function add() {
    // 第一次执行时，定义一个数组专门用来存储所有的参数
    var _args =[...arguments];

    // 利用闭包的特性保存_args并收集所有的参数值
    var _adder = function() {
        _args.push(...arguments);
        return _adder;
    };

    // 利用toString隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
    _adder.valueOf = function () {
        return _args.reduce(function (a, b) {
            return a + b;
        });
    }
    return _adder;
}
add(1)(2)(3)                
// 6
add(1, 2, 3)(4)            
 // 10
add(1)(2)(3)(4)(5)          
// 15
add(2, 6)(1)               
 // 9
```
箭头函数:this取决于定义位置，它自身无this，不能使用yield、arguments、call、bind、apply以及new。 
尾调用、尾递归的内存优化，防止栈溢出。 
## 对象的扩展
属性名表达式。  
```
var key = 'name';
var funcNm = 'getName';
var obj = {
  [key]:'张三',
  age:25,
  [funcNm]() {
    return this[key]
  }
}
obj[funcNm].name
// getname
``` 
Object.is(val1,val2)：比较val1和val2是否严格相等。  
```
Object.is({},{})
// false
Object.is(+0,-0)
// false
Object.is(NaN,NaN)
// true
```
Object.assign特例。  
```
Object.assign({},undefined)
// {}
Object.assign({},null)
// {}
Object.assign({},'abc',true,1)
// {0:'a',1:'b',2:'c'}
```
Object.keys(obj)：返回对象自身可枚举属性组成的数组。  
Object.values(obj)：返回对象自身可枚举属性值组成的数组。  
Object.entries(obj)：返回对象自身可枚举属性键值数组组成的数组。
Object.getOwnPropertyNames(obj)：返回对象自身所有属性（不含Symbol属性，包括不可枚举属性）。   
Object.getOwnPropertySymbols(obj)：返回对象自身所有Symbol属性。  
Reflect.ownKeys(obj)：返回对象自身所有属性。    
obj._proto_：设置或读取当前对象的property属性。   
Object.setPrototypeOf(obj,proto)：设置对象的prototype属性。  
Object.getPrototypeyOf(obj)：读取对象的prototype属性。  
super：super关键字指向当前对象的原型。
```
var proto = {}
var obj = {
  a: ​1,
  foo() {
    return super.b​
  ​}
​​}
Object.setPrototypeOf(obj,proto)
proto.b = 2
proto.c = 3
obj.a
// 1
obj.b
// 2​
obj.c
// 3​
obj.foo()
// 2​
```
对象扩展运算符是浅拷贝，后边的值会覆盖前边的值。
```
let obj = {a:1,b:2,c:3}
let o = {...obj,a:4}
// {a: 4, b: 2, c: 3}
```
## System
## set和map
### set数据结构
set默认遍历器为values方法。  
set.add(val)：添加值返回set结构本身，重复的默认忽略，set不存在类型转换，判断是否存在使用严格全等判断，唯一特例NAN算同一个值。  
set.delete(val)：删除值，返回布尔值。  
set.has(val)：是否存在val，返回布尔值。    
set.clear()：清空set，无返回。  
set.size：set成员数。
### weakSet数据结构
weakSet成员只能为对象，其他对象不在引用weakSet成员对象，weakSet成员对象自动被垃圾回收。  
weakset.add(val)：添加新成员。  
weakset.delete(val)：删除成员。  
weakset.has(val)：是否存在某对象。  
## map数据结构
类似于对象，但它的键值可以为任何值，map的遍历顺序是插入顺序，默认遍历方法为entries。  
map.size：map成数。  
map.set(key,val)：添加数据，返回map结构，键值严格相等则视为同一个键值，唯一特例NAN被视为同一键值。  
map.get(key)：获取数据，不存在返回undefined。  
map.has(key)：是否存在某个键值，返回布尔值。  
map.delete(key)：删除某对键值，返回布尔值。  
map.clear()：情况map，无返回。  
### weakmap
只接受对象作为键值，其他对象不在引用weakmap成员对象，weakmap成员对象自动被垃圾回收。 
weakmap.set(key,val)：添加数据，返回weakmap结构。  
weakmap.get(key)：获取数据，不存在返回undefined。  
weakmap.has(key)：是否存在某个键值，返回布尔值。  
weakmap.delete(key)：删除某对键值，返回布尔值。
## Proxy 
```
// target为代理对象，handle代理处理函数,proxy中的this指向自身而不是target
var proxy = new Proxy(target,handle)
function handle() {
  return {
    // 拦截读取操作，target为拦截对象，property为拦截属性
    get(target,property) {
      return target[property]
    },
    // 拦截赋值操作，target为拦截对象，property为拦截属性，value为赋值
    set(target,property,value) {

    },
    // 拦截函数调用，target为拦截对象，context为上下文this，args参数数组
    apply(target,context,args) {

    },
    // 拦截key in obj中的in操作符
    has(target,property) {

    },
    // 拦截new操作
    construct(target,args) {

    },
    // 拦截delete操作
    deleteProperty(target,property) {

    },
    // 拦截defineProperty操作
    defineProperty(target,property) {

    }
    // getOwnPropertyDescriptor、getPrototypeOf、isExtensible、ownKeys、preventExtensions、setPrototypeOf同上
  }
}
// 取消代理
let {proxy, revoke} = Proxy.revocable(target, handler);
revoke()
// Reflect替代Object属性方法
Reflect.apply(target,thisArg,args)
Reflect.construct(target,args)
Reflect.get(target,name,receiver)
Reflect.set(target,name,value,receiver)
Reflect.defineProperty(target,name,desc)
Reflect.deleteProperty(target,name)
Reflect.has(target,name)
Reflect.ownKeys(target)
Reflect.isExtensible(target)
Reflect.preventExtensions(target)
Reflect.getOwnPropertyDescriptor(target, name)
Reflect.getPrototypeOf(target)
Reflect.setPrototypeOf(target, prototype)
```
## Class
es6类内部方法不可枚举。constructor中为类自身属性，否则为prototype上属性。  
es5的继承是先创建子类this在设置父类方法，es6是先创建父类实例this，子类再修改它（可继承父类实例、prototype以及static静态方法）。  
es6子类调用父类方法this仍然指向的是子类。    








