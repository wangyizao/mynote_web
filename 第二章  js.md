# 第二章  js

### 2.1 let const var 相关

var ——ES5 变量声明方式

1. 在变量未赋值时，变量undefined（为使用声明变量时也为undefined）
2. 作用域——var的作用域为方法作用域；只要在方法内定义了，整个方法内的定义变量后的代码都可以使用

let——ES6变量声明方式

1. 在变量为声明前直接使用会报错
2. 作用域——let为块作用域——通常let比var 范围要小
3. let禁止重复声明变量，否则会报错；var可以重复声明

const——ES6变量声明方式

\1. const为常量声明方式；声明变量时必须初始化，在后面出现的代码中不能再修改该常量的值

\2. const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动

### 2.2 js数据类型，区别

基本数据类型：

Number，String，Boolean，null，undefined，symbol，bigint（后两个为ES6新增）

引用数据类型：

object，function（**proto** Function.prototype）

object：普通对象，数组对象，正则对象，日期对象，Math数学函数对象。

两种数据存储方式：

基本数据类型是直接存储在栈中的简单数据段，占据空间小、大小固定，属于被频繁使用的数据。栈是存储基 本类型值和执行代码的空间。

引用数据类型是存储在堆内存中，占据空间大、大小不固定。引用数据类型在栈中存储了指针，该指针指向堆 中该实体的起始地址，当解释器寻找引用值时，会检索其在栈中的地址，取得地址后从堆中获得实体。

两种数据类型的区别：

1. 堆比栈空间大，栈比堆运行速度快。
2. 堆内存是无序存储，可以根据引用直接获取。
3. 基础数据类型比较稳定，而且相对来说占用的内存小。
4. 引用数据类型大小是动态的，而且是无限的。

### 2.3 Object.assign的理解

作用：Object.assign可以实现对象的合并。

语法：Object.assign(target, ...sources)

1. Object.assign会将source里面的可枚举属性复制到target，如果和target的已有属性重名，则会覆盖。
2. 后续的source会覆盖前面的source的同名属性。
3. Object.assign复制的是属性值，如果属性值是一个引用类型，那么复制的其实是引用地址，就会存在引用共享的问题。

### 2.4 constructor的理解

创建的每个函数都有一个prototype（原型）对象，这个属性是一个指针，指向一个对象。在默认情况下，所有原型对象都会自动获得一个constructor（构造函数）属性，这个属性是一个指向prototype属性所在函数的指针。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（继承自构造函数的prototype），指向构造函数的原型对象。注意当将构造函数的prototype设置为等于一个以对象字面量形式创建的新对象时，constructor属性不再指向该构造函数。

### 2.5 map 和 forEach 的区别

相同点：

1. 都是循环遍历数组中的每一项
2. 每次执行匿名函数都支持三个参数，参数分别为item（当前每一项），index（索引值），arr（原数组）
3. 匿名函数中的this都是指向window
4. 只能遍历数组

不同点：

1. map()会分配内存空间存储新数组并返回，forEach()不会返回数据。
2. forEach()允许callback更改原始数组的元素。map()返回新的数组。

### 2.6 for of 可以遍历哪些对象

for..of..: 它是es6新增的一个遍历方法，但**只限于迭代器(iterator)**, 所以普通的对象用for..of遍历
是会报错的。

可迭代的对象：包括Array, Map, Set, String, TypedArray, arguments对象等等

### 2.7 js静态类型检查

静态类型语言：类型检查发生在编译阶段，因此除非修复错误，否则会一直编译失败

动态类型语言：只有在程序运行了一次的时候错误才会被发现，也就是在运行时，因此即使代码中包含了会 在运行时阻止脚本正常运行的错误类型，这段代码也可以通过编译

##### **js静态类型检查的方法**

**Flow**是Facebook开发和发布的一个开源的静态类型检查库，它允许你逐渐地向JavaScript代码中添加类型。

**TypeScript**是一个会编译为JavaScript的超集

##### **使用静态类型的优势**

- 可以尽早发现bug和错误
- 减少了复杂的错误处理
- 将数据和行为分离
- 减少单元测试的数量
- 提供了领域建模（domain modeling）工具
- 帮助我们消除了一整类bug
- 重构时更有信心

##### **使用静态类型的劣势**

- 代码冗长
- 需要花时间去掌握类型

### 2.8 indexof

语法：str.indexOf(searchValue [, fromIndex])

参数：

searchValue：要被查找的字符串值。

fromIndex：可选位置，数字表示开始查找的位置。可以是任意整数，默认值为0。

返回值：

查找的字符串searchValue的**第一次**出现的索引，如果没有找到，则返回-1。

若被查找的字符串searchValue是一个空字符串，则返回fromIndex。如果fromIndex值为空，或者fromIndex值小于被查找的字符串的长度，返回值和以下的fromIndex值一样。

如果fromIndex值大于等于字符串的长度，将会直接返回字符串的长度（str.length）

### 2.9 iframe有什么优点、缺点

优点：

1. iframe能够原封不动的把嵌入的网页展现出来。
2. 如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。
3. 网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。
4. 如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

缺点：

1. iframe会阻塞主页面的onload事件；
2. iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。会产生很多页面，不容易管理。
3. iframe框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。
4. 代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化（SEO）。
5. 很多的移动设备无法完全显示框架，设备兼容性差。
6. iframe框架页面会增加服务器的http请求，对于大型网站是不可取的。

### 2.10 webComponents

**Web Components** 总的来说是提供一整套完善的封装机制来把 Web 组件化这个东西标准化，每个框架实现的组件都统一标准地进行输入输出，这样可以更好推动组件的复用。

以下是他的四个部分：

**Custom Elements**

提供一种方式让开发者可以自定义 HTML 元素，包括特定的组成，样式和行为。支持 Web Components 标准的浏览器会提供一系列 API 给开发者用于创建自定义的元素，或者扩展现有元素。

**HTML Imports**

一种在 HTMLs 中引用以及复用其他的 HTML 文档的方式。这个 Import 很漂亮，可以简单理解为我们常见 的模板中的include之类的作用

**HTML Templates**

模板

**Shadow DOM**

提供一种更好地组织页面元素的方式，来为日趋复杂的页面应用提供强大支持，避免代码间的相互影响

### 2.11 dva的数据流流向是怎么样的

数据的改变发生通常是通过用户交互行为或者浏览器行为（如路由跳转等）触发的，当此类行为会改变数据 的时候可以通过dispatch发起一个 action，如果是同步行为会直接通过Reducers改变State，如果是 异步行为（副作用）会先触发Effects然后流向Reducers最终改变State，所以在 dva 中，数据流向非 常清晰简明，并且思路基本跟开源社区保持一致。

![img](https://uploadfiles.nowcoder.com/images/20220301/4107856_1646121613938/80A2D6E5BA845BDC932EF25370C0DB0D)

### 2.12 变量提升

JavaScript是单线程语言，所以执行肯定是按顺序执行。但是并不是逐行的分析和执行，而是一段一段地分析执行，会先进行编译阶段然后才是执行阶段。在编译阶段阶段，代码真正执行前的几毫秒，会检测到所有的变量和函数声明，所有这些函数和变量声明都被添加到名为Lexical Environment的JavaScript数据结构内的内存中。所以这些变量和函数能在它们真正被声明之前使用。

### 2.13 作用域

作用域就是一个独立的地盘，让变量不会外泄、暴露出去。也就是说作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

ES6 之前 JavaScript 没有块级作用域,只有全局作用域和函数作用域。ES6 的到来，为我们提供了‘块级作用域’,可通过新增命令 let 和 const 来体现。

### 2.14 HashMap 和 Array 有什么区别？

1. 查找效率
   HashMap因为其根据hashcode的值直接算出index,所以其查找效率是随着数组长度增大而增加的。
   ArrayMap使用的是二分法查找，所以当数组长度每增加一倍时，就需要多进行一次判断，效率下降
2. 扩容数量
   HashMap初始值16个长度，每次扩容的时候，直接申请双倍的数组空间。
   ArrayMap每次扩容的时候，如果size长度大于8时申请size*1.5个长度，大于4小于8时申请8个，小于4时申 请4个。这样比较ArrayMap其实是申请了更少的内存空间，但是扩容的频率会更高。因此，如果数据量比较大的时候，还是使用HashMap更合适，因为其扩容的次数要比ArrayMap少很多。
3. 扩容效率
   HashMap每次扩容的时候重新计算每个数组成员的位置，然后放到新的位置。
   ArrayMap则是直接使用System.arraycopy，所以效率上肯定是ArrayMap更占优势。
4. 内存消耗
   以ArrayMap采用了一种独特的方式，能够重复的利用因为数据扩容而遗留下来的数组空间，方便下一个ArrayMap的使用。而HashMap没有这种设计。 由于ArrayMap之缓存了长度是4和8的时候，所以如果频繁的使用到Map，而且数据量都比较小的时候，ArrayMap无疑是相当的是节省内存的。

### 2.15 HashMap和Object

| Map      | Object                                                       |                                                              |
| :------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 意外的键 | Map默认情况不包含任何键。只包含显式插入的键。                | 一个Object有一个原型, 原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。**注意:** 虽然 ES5 开始可以用Object.create(null)来创建一个没有原型的对象，但是这种用法不太常见。 |
| 键的类型 | 一个Map的键可以是**任意值**，包括函数、对象或任意基本类型。  | 一个Object的键必须是一个 [String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 或是[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)。 |
| 键的顺序 | Map中的 key 是有序的。因此，当迭代的时候，一个Map对象以插入的顺序返回键值。 | 一个Object的键是无序的注意：自ECMAScript 2015规范以来，对象*确实*保留了字符串和Symbol键的创建顺序； 因此，在只有字符串键的对象上进行迭代将按插入顺序产生键。 |
| Size     | Map的键值对个数可以轻易地通过[size](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/size) 属性获取 | Object的键值对个数只能手动计算                               |
| 迭代     | Map是 [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/iterable) 的，所以可以直接被迭代。 | 迭代一个Object需要以某种方式获取它的键然后才能迭代。         |
| 性能     | 在频繁增删键值对的场景下表现更好。                           | 在频繁添加和删除键值对的场景下未作出优化。                   |

### 2.16 javascript中arguments相关的问题

**arguments**

在js中，我们在调用有参数的函数时，当往这个调用的有参函数传参时，js会把所传的参数全部存到一个叫arguments的对象里面。它是一个类数组数据

**由来**

Javascrip中每个函数都会有一个Arguments对象实例arguments，引用着函数的实参。它是寄生在js函数当中的，不能显式创建，arguments对象只有函数开始时才可用

**作用**

有了arguments这个对象之后，我们可以不用给函数预先设定形参了，可以动态地通过arguments为函数加入参数

### 2.17 instanceOf 原理，手动实现 function isInstanceOf (child, Parent)

其实 instanceof 主要的实现原理就是只要右边变量的 prototype 在左边变量的原型链上即可。因此，instanceof 在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype，如果查找失败，则会返回 false，告诉我们左边变量并非是右边变量的实例。

### 2.18 数组去重

**1. 利用ES6 Set去重（ES6中最常用）**

```
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
```

不考虑兼容性，这种去重的方法代码最少。这种方法还无法去掉“{}”空对象，后面的高阶方法会添加去掉重复“{}”的方法。

**2. 利用for嵌套for，然后splice去重（ES5中最常用）**

```
function unique(arr){           
        for(var i=0; i<arr.length; i++){
            for(var j=i+1; j<arr.length; j++){
                if(arr[i]==arr[j]){         //第一个等同于第二个，splice方法删除第二个
                    arr.splice(j,1);
                    j--;
                }
            }
        }
return arr;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
    //[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]     //NaN和{}没有去重，两个null直接消失了function` `unique(arr){      ``    ``for``(``var` `i=0; i<arr.length; i++){``      ``for``(``var` `j=i+1; j<arr.length; j++){``        ``if``(arr[i]==arr[j]){     ``//第一个等同于第二个，splice方法删除第二个``          ``arr.splice(j,1);``          ``j--;``        ``}``      ``}``    ``}``return` `arr;``}``var` `arr = [1,1,``'true'``,``'true'``,``true``,``true``,15,15,``false``,``false``, undefined,undefined, ``null``,``null``, NaN, NaN,``'NaN'``, 0, 0, ``'a'``, ``'a'``,{},{}];``  ``console.log(unique(arr))``  ``//[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]   //NaN和{}没有去重，两个null直接消失了
```

双层循环，外层循环元素，内层循环时比较值。值相同时，则删去这个值。

**3. 利用indexOf去重**

```
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array .indexOf(arr[i]) === -1) {
            array .push(arr[i])
        }
    }
    return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
   // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]  //NaN、{}没有去重
```

新建一个空的结果数组，for 循环原数组，判断结果数组是否存在当前元素，如果有相同的值则跳过，不相同则push进数组。

**4. 利用sort()**

```
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return;
    }
    arr = arr.sort()
    var arrry= [arr[0]];
    for (var i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i-1]) {
            arrry.push(arr[i]);
        }
    }
    return arrry;
}
     var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
// [0, 1, 15, "NaN", NaN, NaN, {…}, {…}, "a", false, null, true, "true", undefined]      //NaN、{}没有去重
```

利用sort()排序方法，然后根据排序后的结果进行遍历及相邻元素比对。

**5. 利用对象的属性不能相同的特点进行去重（这种数组去重的方法有问题，不建议用，有待改进）**

```
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var arrry= [];
     var  obj = {};
    for (var i = 0; i < arr.length; i++) {
        if (!obj[arr[i]]) {
            arrry.push(arr[i])
            obj[arr[i]] = 1
        } else {
            obj[arr[i]]++
        }
    }
    return arrry;
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", 15, false, undefined, null, NaN, 0, "a", {…}]    //两个true直接去掉了，NaN和{}去重
```

**6. 利用includes**

```
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array =[];
    for(var i = 0; i < arr.length; i++) {
            if( !array.includes( arr[i]) ) {//includes 检测数组是否有某个值
                    array.push(arr[i]);
              }
    }
    return array
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
    //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]     //{}没有去重
```

**7. 利用hasOwnProperty**

```
function unique(arr) {
    var obj = {};
    return arr.filter(function(item, index, arr){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]   //所有的都去重了
```

利用hasOwnProperty 判断是否存在对象属性

**8. 利用filter**

```
function unique(arr) {
  return arr.filter(function(item, index, arr) {
    //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
    return arr.indexOf(item, 0) === index;
  });
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, "NaN", 0, "a", {…}, {…}]
```

**9. 利用递归去重**

```
function unique(arr) {
        var array= arr;
        var len = array.length;
 
    array.sort(function(a,b){   //排序后更加方便去重
        return a - b;
    })
 
    function loop(index){
        if(index >= 1){
            if(array[index] === array[index-1]){
                array.splice(index,1);
            }
            loop(index - 1);    //递归loop，然后数组去重
        }
    }
    loop(len-1);
    return array;
}
 var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```

**10. 利用Map数据结构去重**

```
function arrayNonRepeatfy(arr) {
  let map = new Map();
  let array = new Array();  // 数组用于返回结果
  for (let i = 0; i < arr.length; i++) {
    if(map .has(arr[i])) {  // 如果有该key值
      map .set(arr[i], true);
    } else {
      map .set(arr[i], false);   // 如果没有该key值
      array .push(arr[i]);
    }
  }
  return array ;
}
 var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```

创建一个空Map数据结构，遍历需要去重的数组，把数组的每一个元素作为key存到Map中。由于Map中不会出现相同的key值，所以最终得到的就是去重后的结果。

**11. 利用reduce+includes**

```
function unique(arr){
    return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[]);
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr));
// [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
```

### 2.19 编码和字符集的区别

字符集是书写系统字母与符号的集合，而字符编码则是将字符映射为一特定的字节或字节序列，是一种规则。通常特定的字符集采用特定的编码方式（即一种字符集对应一种字符编码（例如：ASCII、IOS-8859-1、GB2312、GBK，都是即表示了字符集又表示了对应的字符编码，但Unicode不是，它采用现代的模型））

**扩展：**

字符：在计算机和电信技术中，一个字符是一个单位的字形、类字形单位或符号的基本信息。即一个字符可以是一个中文汉字、一个英文字母、一个阿拉伯数字、一个标点符号等。

字符集：多个字符的集合。例如GB2312是中国国家标准的简体中文字符集，GB2312收录简化汉字（6763个）及一般符号、序号、数字、拉丁字母、日文假名、希腊字母、俄文字母、汉语拼音符号、汉语注音字母，共 7445 个图形字符。

字符编码：把字符集中的字符编码为（映射）指定集合中的某一对象（例如：比特模式、自然数序列、电脉冲），以便文本在计算机中存储和通过通信网络的传递。

### 2.20 null 和 undefined 的区别，如何让一个属性变为null

undefined 表示一个变量自然的、最原始的状态值，而 null 则表示一个变量被人为的设置为空对象，而不是原始状态。所以，在实际使用过程中，为了保证变量所代表的语义，不要对一个变量显式的赋值 undefined，当需要释放一个对象时，直接赋值为 null 即可。

**undefined** 的字面意思就是：未定义的值 。这个值的语义是，希望**表示一个变量最原始的状态，而非人为操作的结果 。** 这种原始状态会在以下 4 种场景中出现：

1. 声明了一个变量，但没有赋值
2. 访问对象上不存在的属性
3. 函数定义了形参，但没有传递实参
4. 使用 void 对表达式求值

因此，undefined 一般都来自于某个表达式最原始的状态值，不是人为操作的结果。当然，你也可以手动给一个变量赋值 undefined，但这样做没有意义，因为一个变量不赋值就是 undefined 。

**null** 的字面意思是：空值 。这个值的语义是，希望**表示 一个对象被人为的重置为空对象，而非一个变量最原始的状态 。** 在内存里的表示就是，栈中的变量没有指向堆中的内存对象

#### 2.21 数组和伪数组的区别

##### 1.定义

数组是一个特殊对象,与常规对象的区别：

1. 当由新元素添加到列表中时，自动更新length属性
2. 设置length属性，可以截断数组
3. 从Array.protoype中继承了方法
4. 属性为'Array'

类数组是一个拥有length属性，并且他属性为非负整数的普通对象，类数组不能直接调用数组方法。

##### 2.区别

本质：类数组是简单对象，它的原型关系与数组不同。

##### 类数组转换为数组

转换方法

- 使用Array.from()
- 使用Array.prototype.slice.call()
- 使用Array.prototype.forEach()进行属性遍历并组成新的数组

### 2.22 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

**Set**

1. 成员不能重复；
2. 只有键值，没有键名，有点类似数组；
3. 可以遍历，方法有add、delete、has

**WeakSet**

1. 成员都是对象（引用）；
2. 成员都是弱引用，随时可以消失（不计入垃圾回收机制）。可以用来保存 DOM 节点，不容易造成内存泄露；
3. 不能遍历，方法有add、delete、has；

**Map**

1. 本质上是键值对的集合，类似集合；
2. 可以遍历，方法很多，可以跟各种数据格式转换；

**WeakMap**

1. 只接收对象为键名（null 除外），不接受其他类型的值作为键名；
2. 键名指向的对象，不计入垃圾回收机制；
3. 不能遍历，方法同get、set、has、delete；

### 2.23 简单说说 js 中有哪几种内存泄露的情况

1. 意外的全局变量；
2. 闭包；
3. 未被清空的定时器；
4. 未被销毁的事件监听；
5. DOM 引用；

### 2.24 json和xml数据的区别

1.  数据体积方面：xml是重量级的，json是轻量级的，传递的速度更快些。
2. 数据传输方面：xml在传输过程中比较占带宽，json占带宽少，易于压缩。
3. 数据交互方面：json与javascript的交互更加方便，更容易解析处理，更好的进行数据交互
4. 数据描述方面：json对数据的描述性比xml较差
5. xml和json都用在项目交互下，xml多用于做配置文件，json用于数据交互。

### 2.25 JavaScript有几种方法判断变量的类型?

1. 使用typeof检测当需要判断变量是否是number, string, boolean, function, undefined等类型时，可以使用typeof进行判断。
2. 使用instanceof检测instanceof运算符与typeof运算符相似，用于识别正在处理的对象的类型。与typeof方法不同的是，instanceof 方法要求开发者明确地确认对象为某特定类型。
3. 使用constructor检测constructor本来是原型对象上的属性，指向构造函数。但是根据实例对象寻找属性的顺序，若实例对象上没有实例属性或方法时，就去原型链上寻找，因此，实例对象也是能使用constructor属性的。

### 2.26 delete的使用

1. delete使用原则：delete 操作符用来删除一个对象的属性。
2. delete在删除一个不可配置的属性时在严格模式和非严格模式下的区别:
   （1）在严格模式中，如果属性是一个不可配置（non-configurable）属性，删除时会抛出异常;
   （2）非严格模式下返回 false。
3. delete能删除隐式声明的全局变量：这个全局变量其实是global对象(window)的属性
4. delete能删除的：
   （1）可配置对象的属性（2）隐式声明的全局变量 （3）用户定义的属性 （4）在ECMAScript 6中，通过 const 或 let 声明指定的 "temporal dead zone" (TDZ) 对 delete 操作符也会起作用
   delete不能删除的：
   （2）显式声明的全局变量 （2）内置对象的内置属性 （3）一个对象从原型继承而来的属性
5. delete删除数组元素：
   （1）当你删除一个数组元素时，数组的 length 属性并不会变小，数组元素变成undefined
   （2）当用 delete 操作符删除一个数组元素时，被删除的元素已经完全不属于该数组。
   （3）如果你想让一个数组元素的值变为 undefined 而不是删除它，可以使用 undefined 给其赋值而不是使用 delete 操作符。此时数组元素是在数组中的
6. delete 操作符与直接释放内存（只能通过解除引用来间接释放）没有关系。