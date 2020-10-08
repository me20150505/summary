# JS 教程笔记

<a name="nGpHL"></a>
# 基本语法


- JavaScript 是一种动态类型语言，也就是说，变量的类型没有限制，变量可以随时更改类型。
- 【变量提升】【函数提升】
- 【标识符】第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）。第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字0-9。
- 中文是合法的标识符，可以用作变量名。



```javascript
function countDown(n){
  while(n-- > 0){// 3, 2, 1
    console.log(n);// 2, 1, 0
  }
}
countDown(3);
// 2, 1, 0

var x = 1;
var y = 2;
if(x = y){
  console.log(x);
}
// 2
// 把y值赋值x，再判断x的布尔值，结果为true

var msg = '数字' + n + '是' + (n % 2 === 0 ? '偶数' : '奇数');
```


- 【for 循环】for语句的三个部分（initialize、test、increment），可以省略任何一个，也可以全部省略。省略了for语句表达式的三个部分，结果就导致了一个无限循环。
- 【do…while 循环】do...while循环与while循环类似，唯一的区别就是先运行一次循环体，然后判断循环条件。
- 【标签】标签通常与break语句和continue语句配合使用，跳出特定的循环。



```javascript
top:
		for(var i = 0; i < 3; i++){
			for(var j = 0; j < 3; j++){
				if(i === 1 && j === 1){
					continue top;
				}
				console.log('i的值为：' + i + '；j的值为：' + j);
			}
		}
	/*
	 i的值为：0；j的值为：0
	 i的值为：0；j的值为：1
	 i的值为：0；j的值为：2
	 i的值为：1；j的值为：0
	 i的值为：2；j的值为：0
	 i的值为：2；j的值为：1
	 i的值为：2；j的值为：2
	 */
```

<br />

<a name="fufOf"></a>
# 数据类型


- 数值（number）：整数和小数
- 字符串（string）：文本
- 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
- undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值
- null：表示空值，即此处的值为空
- 对象（object）：各种值组成的集合
- symbol【ES6】
- 对象是最复杂的数据类型，又可以分成三个子类型。<br />
   - 狭义的对象（object）
   - 数组（array）
   - 函数（function）
- JavaScript 有三种方法，可以确定一个值到底是什么类型。
   - typeof运算符【number、string、boolean、function、object、undefined】
   - instanceof运算符【variable instanceof Array】【Array、Date】
   - Object.prototype.toString方法
- 如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为false，其他值都视为true，包括空数组（[]）和空对象（{}）。
   - undefined
   - null
   - ''或""
   - 0
   - NaN
   - false



```javascript
Number(null);// 0
Number(undefined);// NaN
```

<br />

<a name="k1QA3"></a>
# 数值


- JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，1与1.0是相同的，是同一个数。
- 由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。
- 【数值精度】大于2的53次方以后，整数运算的结果开始出现错误。由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript 对15位的十进制数都可以精确处理。
- 【数值范围】如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回Infinity。



```javascript
0.1 + 0.2 === 0.3// false
0.3 / 0.1// 2.9999999999999996

Number.MAX_VALUE// 1.7976931348623157e+308
Number.MIN_VALUE// 5e-324

  -3.1E12// -3100000000000

0 / 0// NaN
0 * Infinity// NaN
(1 / +0) === (1 / -0)// false(+Infinity !== -Infinity)
```


- 数值的进制
   - 十进制：没有前导0的数值。
   - 八进制：有前缀0o或0O的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
   - 十六进制：有前缀0x或0X的数值。
   - 二进制：有前缀0b或0B的数值。


<br />

<a name="JUKpP"></a>
# 与数值相关的全局方法


- Number()<br />
- isNaN()<br />
- isFinite()<br />
   - isFinite方法返回一个布尔值，表示某个值是否为正常的数值。
   - 除了Infinity、-Infinity和NaN这三个值会返回false，isFinite对于其他的数值都会返回true。
- parseInt()
   - parseInt方法用于将字符串转为整数。
   - parseInt方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。如果第二个参数是0、undefined和null，则直接忽略。
   - 如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回NaN。
- parseFloat()
   - parseFloat方法用于将一个字符串转为浮点数。



```javascript
parseInt(011, 2);// NaN
parseInt('011', 2);// 3
isNaN([])// false
/*
	 parseInt(011, 2)
	 等同于parseInt(String(011), 2)
	 等同于parseInt('9', 2)

	 isNaN([])
	 等同于isNaN(Number([]))
	 等同于isNaN(0)

	 function numberIsNaN(value){
	 	return typeof value === 'number' && isNaN(value);
	 }
	 */
```

<br />

<a name="eC7OX"></a>
# 字符串


- 如果要在单引号字符串的内部，使用单引号，就必须在内部的单引号前面加上反斜杠，用来转义。
- 如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠。
- 如果想输出多行字符串，有一种利用多行注释的变通方法。
- 转义<br />
   - \0 ：null（\u0000）
   - \b ：后退键（\u0008）
   - \f ：换页符（\u000C）
   - \n ：换行符（\u000A）
   - \r ：回车键（\u000D）
   - \t ：制表符（\u0009）
   - \v ：垂直制表符（\u000B）
   - \' ：单引号（\u0027）
   - \" ：双引号（\u0022）
   - \\ ：反斜杠（\u005C）
   - 反斜杠还有三种特殊用法。
   - \HHH
   - \xHH
   - \uXXXX
- Base64 就是一种编码方法，可以将任意值转成 0～9、A～Z、a-z、+和/这64个字符组成的可打印字符。
   - 这两个方法不适合非 ASCII 码的字符，会报错。要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。
   - btoa()：任意值转为 Base64 编码
   - atob()：Base64 编码转为原来的值



```javascript
(function(){/*
	line 1
	line 2
	line 3
	*/}).toString().split("\n").slice(1, -1).join("\n");
	
	// 字符编码与解码
	function base64Encode(value){
		return btoa(encodeURIComponent(value));
	}
	function base64Decode(value){
		return decodeURIComponent(atob(value));
	}
	
	// UTF-16正则
	([\0-\uD7FF\uE000-\uFFFF]|[\uD800-\uDBFF][\uDC00-\uDFFF])
	
	// 字符集
	function getSymbols(string){
		var len = string.length,
		    index = -1,
		    output = [],
		    character,
		    charCode;
		while(++index < len){
			character = string.charAt(index);
			charCode = character.charCodeAt(0);
			if(charCode >= 0xD800 && charCode <= 0xDBFF){
				output.push(character + string.charAt(++index));
			}else{
				output.push(character);
			}
		}
		return output;
	}
```

<br />

<a name="g235m"></a>
# 对象


- JavaScript 规定，如果行首是大括号，一律解释为语句（即代码块）。如果要解释为表达式（即对象），必须在大括号前加上圆括号。
- 【Object.keys】查看一个对象本身的所有属性，可以使用Object.keys方法。
- 【delete】删除一个不存在的属性，delete不报错，而且返回true。
- 【delete】属性存在，且不得删除，delete命令会返回false。
- 【delete】delete命令只能删除对象本身的属性，无法删除继承的属性。
- 【in】in运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。
- 【for...in】for...in循环不仅遍历对象自身的属性，还遍历继承的属性。
- 【for...in】for...in循环遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。



```javascript
eval('{a: 1}');
eval('({a: 1})');

var obj = Object.defineProperty({}, 'curKey', {
  "value": "defineValue",
  "configurable": false
});

'toString' in obj;

//------hasOwnProperty
var per = {'name': 'Perter'};
for(var i in per){
  if(per.hasOwnProperty(i)){
    console.log(per[i]);
  }
}
```

<br />

<a name="XBNOo"></a>
# 数组


- 如果数组的键名是添加超出范围的数值，该键名会自动转为字符串。
- 数组的slice方法可以将“类似数组的对象”变成真正的数组。



```javascript
var arr = [];
arr[Math.pow(2, 32)] = 'abc';
arr.length// 0

Array.prototype.slice.call(likeArr);
Array.prototype.forEach.call(likeArr, function(elem, i){
  console.log(i + '. ' + elem);
});
```

<br />

<a name="weWgS"></a>
# 函数


- function 命令
- 函数表达式
- Function 构造函数
   - Function构造函数除了最后一个参数是函数的“函数体”，其他参数都是函数的参数。
- 如果同时采用function命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义。
- 【name属性】函数的name属性总是返回紧跟在function关键字之后的那个函数名。
- 【length属性】函数的length属性返回函数预期传入的参数个数，即函数定义之中的参数个数。
- 【toString()】函数的toString方法返回一个字符串，内容是函数的源码。
- 【函数本身作用域】函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。
- 【闭包】外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。
- 【立即调用的函数表达式】推而广之，任何让解释器以表达式来处理函数定义的方法，都能产生同样的效果。
- 【eval命令】eval命令的作用是，将字符串当作语句执行。
   - JavaScript 规定，如果使用严格模式，eval内部声明的变量，不会影响到外部作用域。
   - 即使在严格模式下，eval依然可以读写当前作用域的变量。
   - 此外，eval的命令字符串不会得到 JavaScript 引擎的优化，运行速度较慢。
   - JavaScript 引擎内部，eval实际上是一个引用，默认调用一个内部方法。这使得eval的使用分成两种情况，一种是调用eval(expression)，这叫做“直接使用”，这种情况下eval的作用域就是当前作用域。除此之外的调用方法，都叫“间接调用”，此时eval的作用域总是全局作用域。



```javascript
var func = function func(){
		console.log(typeof func);
	};
	
	var add = new Function(
		'x',
		'y',
		'return x + y'
	);
	
	function fib(num){
		if(num === 0) return 0;
		if(num === 1) return 1;
		return fib(num - 1) + fib(num - 2);
	}
	
	if(false){
		var func = function(){
			console.log(1);
		};
	}
	func && func();
	
	var func = function(a, b){
		'use strict';
		arguments[0] = 3;
		arguments[1] = 2;
		return a + b;
	};
	func(1, 1);// 2
	
	function Person(name){
		var _age;
		function setAge(n){
			_age = n;
		}
		function getAge(){
			return _age;
		}
		return {
			name: name,
			setAge: setAge,
			getAge: getAge
		};
	}
	var person = Person('小明');
	person.setAge(18);
	person.getAge();
	
	var f = function(){}();
	true && function(){}();
	0, function(){}();
	!function(){}();
	+function(){}();
	-function(){}();
	~function(){}();
```

<br />

<a name="YZJYA"></a>
# 运算符


- 算术运算符
   - 加法运算符（Addition）：x + y
   - 减法运算符（Subtraction）： x - y
   - 乘法运算符（Multiplication）： x * y
   - 除法运算符（Division）：x / y
   - 余数运算符（Remainder）：x % y
   - 自增运算符（Increment）：++x 或者 x++
   - 自减运算符（Decrement）：--x 或者 x--
   - 数值运算符（Convert to number）： +x
   - 负数值运算符（Negate）：-x
- 加法运算符
   - 由于参数不同，而改变自身行为的现象，叫做“重载”（overload）。
   - 加法运算符以外的其他算术运算符（比如减法、除法和乘法），都不会发生重载。
- 余数运算符
   - 运算结果的正负号由第一个运算子的正负号决定。
   - 为了得到正确的负数的余数值，需要先使用绝对值函数。
- 数值运算符
   - 数值运算符的作用在于可以将任何值转为数值（与Number函数的作用相同）。
- 严格相等运算符
   - 两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个对象。
   - 对于两个对象的比较，严格相等运算符比较的是地址，而大于或小于运算符比较的是值。
   - undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。
- 位运算符
   - 【1】或运算（or）：符号为|，表示若两个二进制位都为0，则结果为0，否则为1。
   - 【2】与运算（and）：符号为&，表示若两个二进制位都为1，则结果为1，否则为0。
   - 【3】否运算（not）：符号为~，表示对一个二进制位取反。
   - 【4】异或运算（xor）：符号为^，表示若两个二进制位不相同，则结果为1，否则为0。
   - 【5】左移运算（left shift）：符号为<<。
   - 【6】右移运算（right shift）：符号为>>。
   - 【7】带符号位的右移运算（zero filled right shift）：符号为>>>。
   - 位运算符用于直接对二进制位进行计算，一共有7个。
   - 位运算只对整数有效，遇到小数时，会将小数部分舍去，只保留整数部分。
   - 一个数与自身的取反值相加，等于-1。
   - 对字符串进行二进制否运算，JavaScript 引擎会先调用Number函数，将字符串转为数值。
   - 二进制否运算遇到小数时，也会将小数部分舍去，只保留整数部分。所以，对一个小数连续进行两次二进制否运算，能达到取整效果。使用二进制否运算取整，是所有取整方法中最快的一种。
   - 左移运算符（<<）表示将一个数的二进制值向左移动指定的位数，尾部补0，即乘以2的指定次方（最高位即符号位不参与移动）。
   - 如果左移0位，就相当于将该数值转为32位整数，等同于取整，对于正数和负数都有效。
   - 右移运算可以模拟 2 的整除运算。
   - 带符号位的右移运算符（>>>）实际上将一个值转为32位无符号整数。查看一个负整数在计算机内部的储存形式，最快的方法就是使用这个运算符。
- void 运算符
   - void运算符的作用是执行一个表达式，然后不返回任何值，或者说返回undefined。
   - 因为void运算符的优先性很高，如果不使用括号，容易造成错误的结果。
- 逗号运算符
   - 逗号运算符用于对两个表达式求值，并返回后一个表达式的值。



```javascript
 '3' + 4 + 5// '345'
	3 + 4 + '5'// '75'
	
	function toInt32(x){
		return x | 0;
	}
	toInt32(Math.pow(2, 32) + 1);// 1
	toInt32(Math.pow(2, 32) - 1);// -1
	
	//-----RGB颜色转换HEX颜色
	function rgb2hex(r, g, b){
		return '#' + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).substr(1);
	}
	
	var a = 10;
	var b = 100;
	a ^= b;
	b ^= a;
	a ^= b;
	a// 100;
	b// 10;
	
	12.9 ^ 0;// 12
	
	21 >> 4// 21/2^4,即1
	
	var FLAG_A = 1,
		FLAG_B = 2,
		FLAG_C = 4,
		FLAG_D = 8;
	var flag = 5;
	if(flag & FLAG_A){
		console.log(111);
	}
	
```
```html
<a href="javascript:void(document.getElementById('myform').submit());">提交表单</a>
```

<br />

<a name="5QGmb"></a>
# 数据类型转换


- Number()
   - 首先调用valueOf()，若返回对象，然后调用toString()，若返回原始类型的值，调用Number()。
   - parseInt逐个解析字符，而Number函数整体转换字符串的类型。
   - parseInt和Number函数都会自动过滤一个字符串前导和后缀的空格。
   - null转为数值时为0，而undefined转为数值时为NaN。
- String()
   - 首先调用toString()，若返回对象，然后调用valueOf()，若返回原始类型的值，调用String()。
   - String方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。
- Boolean()


<br />
<br />

<a name="nGKvv"></a>
# 错误处理机制


- Error 实例对象
   - message：错误提示信息
   - name：错误名称（非标准属性）
   - stack：错误的堆栈（非标准属性）
- 原生错误类型
   - Error实例对象是最一般的错误类型，在它的基础上，JavaScript 还定义了其他6种错误对象。这6种派生错误，连同原始的Error对象，都是构造函数。
   - 【SyntaxError】对象是解析代码时发生的语法错误。
   - 【ReferenceError】对象是引用一个不存在的变量时发生的错误。
   - 【RangeError】对象是一个值超出有效范围时发生的错误。
   - 【TypeError】对象是变量或参数不是预期类型时发生的错误。
   - 【URIError】对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。
   - 【EvalError】对象是 eval 函数没有被正确执行时抛出的错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。
- throw 语句
- try{}catch(e){}finally{} 结构


<br />
<br />

<a name="uBaxu"></a>
# Object 对象


- Object()
- Object 的方法
   - Object.keys(): 可枚举属性
   - Object.getOwnPropertyNames(): 可枚举、不可枚举属性
   - Object.getOwnPropertySymbols()
   - Reflect.ownKeys()：可枚举、不可枚举、字符串属性、Symbol属性
   - ----关于对象属性模型
   - Object.getOwnPropertyDescriptor(): 获取对象某个属性的描述对象
   - Object.getOwnPropertyDescriptors(): 获取对象的描述对象
   - Object.defineProperty(): 通过描述对象定义某个属性
   - Object.defineProperties(): 通过描述对象定义多个属性
   - ----控制对象状态
   - Object.preventExtensions()
   - Object.isExtensible()
   - Object.seal()
   - Object.isSealed()
   - Object.freeze()
   - Object.isFrozen()
   - ----关于原型链
   - Object.create(prototype, object): 该方法可以指定原型对象和属性，返回一个新的对象
   - Object.getPrototypeOf(obj): 获取对象的Prototype对象
   - Object.setPrototypeOf(obj, prototype): 设置对象的Prototype对象
- Object 的实例属性和方法
   - Object.prototype.__proto__
   - Object.prototype.valueOf(): 返回当前对象对应的值
   - Object.prototype.toString(): 返回当前对象对应的字符串形式
   - Object.prototype.toLocaleString(): 返回当前对象对应的本地字符串形式
   - Object.prototype.hasOwnProperty(): 判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性
   - Object.prototype.isPrototypeOf(): 判断当前对象是否为另一个对象的原型
   - Object.prototype.propertyIsEnumerable(): 判断某个属性是否可枚举
- toString() 的应用
   - Object.prototype.toString(undefined): [object Undefined]
   - Object.prototype.toString(数值): [object Number]
   - Object.prototype.toString(字符串): [object String]
   - Object.prototype.toString(布尔值): [object Boolean]
   - Object.prototype.toString(null): [object Null]
   - Object.prototype.toString(函数): [object Function]
   - Object.prototype.toString(数组): [object Array]
   - Object.prototype.toString(Date): [object Date]
   - Object.prototype.toString(RegExp): [object RegExp]
   - Object.prototype.toString(arguments): [object Arguments]
   - Object.prototype.toString(Error): [object Error]
   - Object.prototype.toString(其他对象): [object Object]



```javascript
  var a = {x: 1};
	// 以下两式等价
	var b = Object.setPrototypeOf({}, a);
	var b = {__proto__: a};
	
	var F = function(){
		this.foo = 'bar';
	};
	var f = new F();
	// 上式与下式等价
	var f = Object.setPrototypeOf({}, F.prototype);
	F.call(f);
	
	// 创建一个不继承任何属性的对象
	var o = Object.create(null, {
		x: {
			value: 1,
			configurable: true
		}
	});
	Object.prototype.isPrototypeOf(o) // false
	
	// 继承父类原型不修改父类原型
	Sub.prototype = Object.create(Super.prototype);
	Sub.prototype.constructor = Sub;
	
	function typeOfObj(value){
		var s = Object.prototype.toString.call(value);
		return s.match(/\[object (.*?)\]/)[1].toLowerCase();
	}
	
	new Date().toLocaleString() // 2018/5/30 下午4:22:00
	
	// 拷贝对象
	var extend = function(to, from){
		var descriptor;
		for(var property in from){
			descriptor = Object.getOwnPropertyDescriptor(from, property);
			if(descriptor && (!descriptor.writable 
			  || !descriptor.enumerable 
			  || !descriptor.configable 
			  || descriptor.get 
			  || descriptor.set)){
				Object.defineProperty(to, property, descriptor);
			}else{
				to[property] = from[property];
			}
		}
		return to;
	};
	
	// 克隆对象：拷贝对象原型和实例属性
	var clone = function(o){
		return Object.assign(Object.create(Object.getPrototypeOf(o)), o);
	};
	var clone = function(o){
		return Object.create(Object.getPrototypeOf(o), Object.getOwnPropertyDescriptors(o));
	};
```

<br />

<a name="hHO2Z"></a>
# Array 对象


- Array 的方法
   - Array.isArray()
- Array 的实例方法
   - Array.prototype.valueOf()
   - Array.prototype.toString()
   - Array.prototype.push(): 返回新数组长度
   - Array.prototype.pop(): 返回删除元素
   - Array.prototype.unshift(): 返回新数组长度
   - Array.prototype.shift(): 返回删除元素
   - Array.prototype.join()
   - Array.prototype.concat()
   - Array.prototype.reverse()
   - Array.prototype.sort()
   - Array.prototype.slice()
   - Array.prototype.splice(): 返回删除元素，索引可为负整数
   - Array.prototype.map()
   - Array.prototype.forEach()
   - Array.prototype.filter()
   - Array.prototype.some()
   - Array.prototype.every()
   - Array.prototype.reduce()
   - Array.prototype.reduceRight()
   - Array.prototype.indexOf()
   - Array.prototype.lastIndexOf()
   - 【注】对于空数组，some方法返回false，every方法返回true，回调函数都不会执行。



```javascript
	// 合并数组
	var a = [1, 2, 3];
	var b = [4, 5, 6];
	Array.prototype.push.apply(a, b);
	a.concat(4, [5, 6]);
	
	Array.prototype.join.call("hello", " | ");// h | e | l | l | o
	Array.prototype.slice.call(arguments);
	
	var data = [{"name": "afg", "age": 18}, {"name": "ghi", "age": 8}];
	var arr1 = [1, 2];
	var arr2 = [];
	data.sort(function(a, b){
		return a.age - b.age;
	});
	// 第二个参数scope绑定回调函数的this关键字
	data.map(function(item, index, arr){
		item.age = item.age + index;
		console.log(this);// [1, 2]
		return item;
	}, arr1);
	data.forEach(function(item, index, arr){
		this.push(index);
	}, arr2);
	// 第二个参数initValue指定初值
	[1, 2, 3, 4, 5].reduce(function(prev, cur, index, arr){
		/*
		 * 10-1
		 * 11-2
		 * 13-3
		 * 16-4
		 * 20-5
		 * 25
		 */
		console.log(prev + "-" + cur);
		return prev + cur;
	}, 10);
	['a', 'bb', 'ccc', 'd', 'ee'].reduce(function(prev, cur, index, arr){
		return prev.length > cur.length ? prev : cur;
	}, '');
```

<br />

<a name="OObOO"></a>
# Number 对象


- Number 的属性
   - Number.POSITIVE_INFINITY
   - Number.NEGATIVE_INFINITY
   - Number.NaN
   - Number.MAX_VALUE：最大正数
   - Number.MIN_VALUE：最小正数
   - Number.MAX_SAFE_INTEGER
   - Number.MIN_SAFE_INTEGER
- Number 的实例方法
   - Number.prototype.toString(hexadecimal)
   - Number.prototype.toFixed()
   - Number.prototype.toExponential()
   - Number.prototype.toPrecision()：将一个数转为指定位数的有效数字
   - 【注】将其他进制的数，转回十进制，需要使用parseInt方法。


<br />

<a name="3zvR8"></a>
# String 对象


- String 的方法
   - String.fromCharCode()
- String 的实例方法
   - String.prototype.indexOf(str, index)
   - String.prototype.lastIndexOf(str, index)
   - String.prototype.charAt(index)
   - String.prototype.charCodeAt(index)
   - String.prototype.concat(str1, str2, ...)
   - String.prototype.slice(start, end)
   - String.prototype.substring(start, end)
   - String.prototype.substr(start, length)
   - String.prototype.trim()
   - String.prototype.toLowerCase()
   - String.prototype.toUpperCase()
   - String.prototype.match(str|reg)
   - String.prototype.search(str|reg)
   - String.prototype.replace(str|reg)
   - String.prototype.split(str|reg, number)
   - String.prototype.localeCompare(str)



```javascript
String.fromCharCode(0xD80F, 0xDF90);
```

<br />

<a name="GVZFn"></a>
# Math 对象


- Math 的属性
   - Math.E
   - Math.LN2
   - Math.LN10
   - Math.LOG2E
   - Math.LOG10E
   - Math.PI
   - Math.SQRT1_2
   - Math.SQRT2
- Math 的方法
   - Math.abs()
   - Math.ceil()
   - Math.floor()
   - Math.max()
   - Math.min()
   - Math.pow()
   - Math.sqrt()
   - Math.log()：以自然对数 e 为底。
   - Math.exp()
   - Math.round()
   - Math.random()
- Math 的三角函数方法
   - Math.sin()
   - Math.cos()
   - Math.tan()
   - Math.asin()
   - Math.acos()
   - Math.atan()



```javascript
	// 任意范围随机数
	function getRandom(min, max){
		return Math.random() * (max - min) + min;
	}
	// 任意整数范围随机数
	function getIntegerRandom(min, max){
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}
	// 任意长度随机字符串
	function getRandomString(length){
		var ALPHA,
		    index,
		    result = '';
		ALPHA = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
		ALPHA += 'abcdefghijklmnopqrstuvwxyz';
		ALPHA += '0123456789-_';
		for(var i = 0; i < length; i++){
			index = Math.floor(Math.random() * ALPHA.length);
			result += ALPHA[index];
		}
		return result;
	}
```

<br />

<a name="nyOlH"></a>
# Date 对象


- 综述
   - new Date(milliseconds)
   - new Date(year, month[, date, hours, minutes, seconds, milliseconds])
   - window.performance.now()
- Date 的方法
   - Date.now()
   - Date.parse()
   - Date.UTC()
- Date 的to类实例方法
   - Date.prototype.toString()
   - Date.prototype.toUTCString()
   - Date.prototype.toISOString()
   - Date.prototype.toJSON()
   - Date.prototype.toDateString()
   - Date.prototype.toTimeString()
   - Date.prototype.toLocaleDateString()
   - Date.prototype.toLocaleTimeString()
- Date 的get类实例方法
   - Date.prototype.getTime()
   - Date.prototype.getFullYear()
   - Date.prototype.getYear()
   - Date.prototype.getMonth()
   - Date.prototype.getDate()
   - Date.prototype.getDay()
   - Date.prototype.getHours()
   - Date.prototype.getMinutes()
   - Date.prototype.getSeconds()
   - Date.prototype.getMilliseconds()
   - Date.prototype.getTimezoneOffset()
- Date 的set类实例方法
   - Date.prototype.setTime()
   - Date.prototype.setFullYear()
   - Date.prototype.setYear()
   - Date.prototype.setMonth()
   - Date.prototype.setDate()
   - Date.prototype.setHours()
   - Date.prototype.setMinutes()
   - Date.prototype.setSeconds()
   - Date.prototype.setMilliseconds()



```javascript
	new Date(2011, 0, -1) // Thu Dec 30 2010 00:00:00 GMT+0800 (中国标准时间)
	new Date(1, 0) // Tue Jan 01 1901 00:00:00 GMT+0800 (中国标准时间)
	new Date(-1, 0) // Fri Jan 01 -001 00:00:00 GMT+0805 (中国标准时间)
	
	var d = new Date(2013, 0, 10);
	d.setDate(-2); // d:Sat Dec 29 2012 00:00:00 GMT+0800 (中国标准时间)
```

<br />

<a name="bOsaA"></a>
# RegExp 对象


- RegExp 的实例属性
   - r.ignoreCase
   - r.multiline
   - r.global
   - r.lastIndex
   - r.source
- RegExp 的实例方法
   - r.test()
   - r.exec()
   - 【注】如果正则表达式带有g修饰符，则match方法与正则对象的exec方法行为不同，会一次性返回所有匹配成功的结果。
- 特殊字符
   - \cX 表示Ctrl-[X]，其中的X是A-Z之中任一个英文字母，用来匹配控制字符
   - [\b] 匹配退格键(U+0008)
   - \n 匹配换行键
   - \r 匹配回车键
   - \t 匹配制表符tab（U+0009）
   - \v 匹配垂直制表符（U+000B）
   - \f 匹配换页符（U+000C）
   - \0 匹配null字符（U+0000）
   - \xhh 匹配一个以两位十六进制数（\x00-\xFF）表示的字符
   - \uhhhh 匹配一个以四位十六进制数（\u0000-\uFFFF）表示的unicode字符
- 预定义模式
   - \d 匹配0-9之间的任一数字，相当于[0-9]
   - \D 匹配所有0-9以外的字符，相当于[^0-9]
   - \w 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]
   - \W 除所有字母、数字和下划线以外的字符，相当于[^A-Za-z0-9_]
   - \s 匹配空格（包括制表符、空格符、断行符等），相等于[\t\r\n\v\f]
   - \S 匹配非空格的字符，相当于[^\t\r\n\v\f]
   - \b 匹配词的边界
   - \B 匹配非词边界，即在词的内部
- 量词符
   - ? 问号表示某个模式出现0次或1次，等同于{0, 1}
   - * 星号表示某个模式出现0次或多次，等同于{0,}
   - + 加号表示某个模式出现1次或多次，等同于{1,}
- 贪婪模式
   - *?：表示某个模式出现0次或多次，匹配时采用非贪婪模式
   - +?：表示某个模式出现1次或多次，匹配时采用非贪婪模式



```javascript
	/*
	 $` 匹配内容前面的文本
	 $& 匹配的内容
	 $' 匹配内容后面的文本
	 */
	'abc'.replace('b', '[$`-$&-$\']'); // a[a-b-c]c
	
	// 如果正则表达式带有括号，则括号匹配的部分也会作为数组成员返回
	'aaa*a*'.split(/(a*)/); // ["", "aaa", "*", "a", "*"]
	
	// 匹配一切字符
	var s = 'Sit down.\nPlease.';
	s.match(/down[^]*Please/); // ["down.\nPlease"]
	
	// [1-35] // 匹配1-3、5
	
	var r = /\bHello\b/;
	r.test('Hello World'); // true
	r.test('Hello-World'); // true
	r.test('HelloWorld'); // false
	
	var r = /a/g;
	var s = 'baah';
	r.test(s); // true
	r.test(s); // true
	r.test(s); // false
	
	// m修饰符
	/^World/m.test('Hello\nWorld'); // true
	
	// 内部引用组匹配
	/a(..)cd\1/.test('ayycdyy'); // true
	
	// 非捕获组
	/(?:.)b(.)/.test('abc');
	// 先行断言
	'abd'.match(/b(?=d)/); // ["b"]
	// 先行否定断言
	'abd'.match('b(?!c)'); // ["b"]
```

<br />

<a name="zZfar"></a>
# Console 对象


- 占位符
   - %s 字符串
   - %d 整数
   - %i 整数
   - %f 浮点数
   - %o 对象的链接
   - %c CSS格式字符串
- console 的方法
   - console.log()
   - console.warn()
   - console.error()
   - console.table()
   - console.count()：用于计数，输出它被调用了多少次。可以接受一个字符串作为参数，作为标签，对执行次数进行分类。
   - console.dir()：用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
   - console.dirxml()：主要用于以目录树的形式，显示DOM节点。
   - console.assert()：接受两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数。
   - console.time()
   - console.timeEnd()
   - console.profile()
   - console.profileEnd()
   - console.group()
   - console.groupCollapsed()
   - console.groupEnd()
   - console.trace()：显示当前执行的代码在堆栈中的调用路径。
   - console.clear()
- 命令行API
   - $0-$4：控制台保存了最近5个在Elements面板选中的DOM元素。



```javascript
	console.log('%cThis is styled text.', 'color:#f00;font-size:28px;');
	console.assert(3 > 5, '3不大于5');
```

<br />

<a name="QNHOm"></a>
# 面向对象编程


- 概述
   - new命令总是返回一个对象，要么是实例对象，要么是return语句指定的对象。



```javascript
	var b = 'world';
	var o = {
		a: {
			b: 'hello',
			c: function(){
				console.log(this.b);
			}
		}
	};
	var res = o.a.c;
	res(); // 等价于window.res()
	
	var MyArr = function(){};
	MyArr.prototype = new Array();
	MyArr.prototype.constructor = MyArr;
	var arr = new MyArr();
	arr.push(1, 2);
	// 下面两语句等价
	MyArr.prototype.isPrototypeOf(arr);
	arr instanceof MyArr;
	
	// 找出属性所在的对象名
	function getObjNameOfProp(obj, prop){
		while(obj && !Object.hasOwnProperty.call(obj, prop)){
			obj = Object.getPrototypeOf(obj);
		}
		return obj ? obj.constructor.name : obj;
	}
```

<br />

<a name="GHJpB"></a>
# 单线程模型


- Event Loop机制
- 多任务执行
   - 排队
   - 新建进程
   - 新建线程
- setTimeout
   - setTimeout(function, time, param1, ...)
   - setTimeout(f, 0)：等到当前脚本的同步任务和“任务队列”中已有的事件全部处理完，才会执行setTimeout指定的任务。
   - 浏览器内部使用32位带符号的整数，来储存推迟执行的时间，意味着setTimeout最多只能推迟执行2147483647毫秒（24.8天）。
- 异步任务
   - 正常任务（不在当前Event Loop执行）
      - setTimeout
      - setInterval
      - setImmediate
      - I/O
      - 各种事件的回调函数
   - 微任务
      - process.nextTick
      - Promise
- Promise
   - 三种状态：未完成(pedding)、成功(resolved)、失败(rejected)
   - 指定一个错误回调函数，错误具有传递性
   - 如果一个任务已经完成，再添加回调函数，该回调函数会立即执行
   - Promise 的方法
      - Promise.all(iterable)
      - Promise.race(iterable)
      - Promise.resolve(value)
      - Promise.reject(reason)
   - Promise 的实例方法
      - Promise.prototype.catch(onRejected)
      - Promise.prototype.then(onFulfilled, onRejected)
      - Promise.prototype.finally(onFinally)



```javascript
	document.getElementById('text').onkeypress = function(){
		var self = this;
		setTimeout(function(){
			self.value = self.value.toUpperCase();
		}, 0);
	};
	
	Promise.resolve([1, 2, 3]).then(function(res){
		return res;
	}).then(function(res){
		return res;
	}).then(function(res){
		
	}, function(err){
		
	});
	var promise = new Promise(function(resolved, rejected){
		// 异步操作
		if(// 异步操作成功){
			resolved(res);
		}else{
			rejected(err);
		}
	});
	promise.then(function(res){
		
	}).then(function(res){
		
	}).then(function(res){
		
	}, function(err){
		
	});
```

<br />

<a name="wzjTZ"></a>
# 严格模式


- 全局变量显式声明
- 禁止this关键字指向全局对象
- 禁止删除变量
- 禁止使用fnname.arguments、fnname.caller，意味着不能在函数内部得到调用栈
- 禁止使用arguments.callee、arguments.caller
- 创设eval作用域，意味着eval所生成的变量只能用于eval内部（全局作用域、函数作用域、eval作用域）
- 函数内部改变参数与arguments的联系被切断，两者不再存在联动关系


<br />

<a name="OQNne"></a>
# ES6


- let命令
   - let所声明的变量，只在所在的代码块内有效
   - 不存在变量提升
- const命令
   - const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变
- 模版字符串
- for...of循环：ES6提供for...of循环，允许遍历获得键值
- 数据结构
   - class结构
   - module定义



```javascript
	// 块级作用域写法
	{
		let a = 10;
	}
	
	// 模版字符串
	var a = 10;
	var b = 2;
	var c = `this is a string,
		its value is ${a + b}`;
	
	// for...of循环
	for(let i of [10, 20, 30]){
		console.log(i);
	}
	var es6 = new Map();
	es6.set('name', 'UE');
	es6.set('age', 18);
	for(let [name, value] of es6){
		console.log(`${name}: ${value}`);
	}
	
	// class结构
	class Person{
		constructor(name, age){
			this.name = name;
			this.age = age;
		}
		sayName(){
			console.log(this.name);
		}
	}
```

<br />

<a name="2zN25"></a>
# 有限状态机


- 状态总数（state）是有限的
- 任一时刻，只处在一种状态之中
- 某种条件下，会从一种状态转变（transition）到另一种状态
- 【注】一个有限状态机的函数库Javascript Finite State Machine



```javascript
	var fsm = StateMachine.create({
		initial: 'green',
		events: [
			{name: 'warn', from: 'green', to: 'yellow'},
			{name: 'stop', from: 'yellow', to: 'red'},
			{name: 'go', from: 'red', to: 'green'}
		],
		error: function(eventName, from, to, args, errCode, errMsg){
			
		}
	});
	fsm.onleavegreen = function(){
		light.fadeOut('slow', function(){
			fsm.transition();
		});
		return SatteMachine.ASYNC;
	};
```

<br />

<a name="4IV94"></a>
# JavaScript解释器


- 词法分析器
- 句法解析器
- 字节码生成器
- 字节码解释器


<br />
<br />

<a name="pSe6u"></a>
# 浏览器环境


- image元素
   - someImage.alt
   - someImage.src
   - someImage.complete
   - someImage.width
   - someImage.height
   - someImage.naturalWidth
   - someImage.naturalHeight
- audio/video元素
   - 并非所有浏览器都支持所有媒体格式，因此可以用source标签指定多个不同的媒体来源。
   - new Audio()
   - 事件次序
      - loadstart：开始加载音频和视频。
      - durationchange：音频和视频的duration属性（时长）发生变化时触发，即已经知道媒体文件的长度。如果没有指定音频和视频文件，duration属性等于NaN。如果播放流媒体文件，没有明确的结束时间，duration属性等于Inf（Infinity）。
      - progress：浏览器正在下载媒体文件，周期性触发。下载信息保存在元素的buffered属性中。
      - canplay：浏览器准备好播放，即使只有几帧，readyState属性变为CAN_PLAY。
      - canplaythrough：浏览器认为可以不缓冲（buffering）播放时触发，即当前下载速度保持不低于播放速度，readyState属性变为CAN_PLAY_THROUGH。
   - 其他事件
      - abort：播放中断。
      - ended：播放结束。
      - error：发生错误。
      - pause：播放暂停。
      - play：暂停后重新开始播放。
      - playing：开始播放，包括第一次播放、暂停后播放、结束后重新播放。
      - ratechange：播放速率改变。
      - seeked：搜索操作结束。
      - seeking：搜索操作开始。
      - suspend：加载文件停止，有可能是播放结束，也有可能是其他原因的暂停。
      - timeupdate：网页元素的currentTime属性改变时触发。
      - volumechange：音量改变时触发（包括静音）。
      - waiting：由于另一个操作（比如搜索）还没有结束，导致当前操作（比如播放）不得不等待。
- script标签
   - defer 属性：等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），在DOMContentLoaded事件触发前执行，即刚刚读取完 html 标签（渲染完再执行）。多个脚本，顺序加载。
   - async 属性：一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染（下载完就执行）。
   - crossorigin="anonymous" 属性
   - 【注】如果同时使用async和defer属性，后者不起作用。
- 浏览器会累积DOM变动，然后一次性重绘。
- Web Components
   - template标签
      - someTemplate.content
      - document.importNode(oldNode, copyType)
   - Custom Element
      - document.registerElement(customTagName, {prototype: prototype, extends: tagName})
      - 如果组件是基于（extends）现有的网页元素，则必须在该种元素上使用is属性指定组件。
      - 【注】自定义网页元素的标签名必须含有连字符（-），一个或多个都可。
   - Shadow DOM
      - 浏览器将模板、样式表、属性、JavaScript代码等，封装成一个独立的DOM元素。外部的设置无法影响到其内部，而内部的设置也不会影响到外部。
      - someElement.createShadowRoot()
   - HTML Import
      - link标签rel="import"
      - someLink.import
      - document.currentScript.ownerDocument
- JS引擎
   - Chakra (Microsoft Internet Explorer)
   - V8 (Chrome, Chromium)
   - Nitro/JavaScript Core (Safari)
   - SpiderMonkey (Firefox)
   - Carakan (Opera)
- URLSearchParams API
   - new URLSearchParams('a=b&c=d')
   - sp.has()
   - sp.get()
   - sp.getAll()
   - sp.set()
   - sp.delete()
   - sp.append()
   - sp.toString()
   - sp.keys()：遍历所有参数名
   - sp.values()：遍历所有参数值
   - sp.entries()：遍历所有参数的键值对
- WebRTC：网络实时通信(Web Real Time Communication)
   - window.navigator.getUserMedia({video: true, audio: true}, successfunction, errorfunction)



```javascript
	window.navigator.getUserMedia({
		audio: true
	}, function(stream){
		var context = new AudioContext();
		var audio = context.createMediaStreamSource(stream);
		audio.connect(context.destination);
	}, function(error){
		
	});
```

<br />

<a name="PKMW8"></a>
# 排序算法


- 冒泡排序
- 选择排序
- 插入排序
- 合并排序
- 快速排序



```javascript
	function swap(array, i, j){
		var temp = array[i];
		array[i] = array[j];
		array[j] = temp;
	}
	// 冒泡排序
	function bubbleSort(array){
		for(var i = 0, len = array.length - 1; i < len; i++){
			for(var j = 0, len2 = array.length - 1 - i; j < len2; j++){
				if(array[j] > array[j + 1]){
					swap(array, j, j + 1);
				}
			}
		}
		return array;
	}
	// 选择排序
	function selectionSort(array){
		var min;
		for(var i = 0, len = array.length; i < len; i++){
			min = i;
			for(var j = i + 1; j < len; j++){
				if(array[j] < array[min]){
					min = j;
				}
			}
			if(i != min){
				swap(array, i, min);
			}
		}
		return array;
	}
	// 插入排序
	function insertionSort(array){
		var value,
			j;
		for(var i = 0, len = array.length; i < len; i++){
			value = array[i];
			for(j = i - 1; j > -1 && array[j] > value; j--){
				array[j + 1] = array[j];
			}
			array[j + 1] = value;
		}
		return array;
	}
	// 合并排序
	function mergeSort(array){
		if(array.length < 2){
			return array;
		}
		var middle = Math.floor(array.length / 2),
			left = array.slice(0, middle),
			right = array.slice(middle),
			params = merge(mergeSort(left), mergeSort(right));
		params.unshift(0, array.length);
		array.splice.apply(array, params);
		return array;
		function merge(left, right){
			var result = [],
				il = 0, 
				ir = 0;
			while(il < left.length && ir < right.length){
				left[il] < right[ir] ? result.push(left[il++]) : result.push(right[ir++]);
			}
			return result.concat(left.slice(il)).concat(right.slice(ir));
		}
	}
```

<br />

<a name="5xZyF"></a>
# AMD规范和requireJS


- script标签data-main入口
- 可应用于JSONP，但只能用于该JSONP URL一次，仅支持返回值类型为JSON object的JSONP服务。
- 使用插件
- requirejs提供一个基于node.js的命令行工具r.js，用来压缩多个js文件。
   - npm install -g requirejs
   - node r.js -o [arguments|filePath]



```javascript
	require.config({
		paths: {
			"backbone": [path1, path2],
			"underscore": path
		},
		shim: {
			"backbone": {
				deps: ["underscore"],
				exports: "Backbone"
			},
			"underscore": {
				exports: "_"
			}
		}
	});
	
	// JSONP
	require(['http://www.api.com?callback=define'], function(res){
		console.log(res);
	});
	
	// 使用插件
	require(["some/module", "text!some/module.html", "text!some/module.css"], function(module, html, css){
		
	});
```

<br />

<a name="Zcf5F"></a>
# underscoreJS


- 关于集合
   - _.map(array|json, function)
   - _.each(array|json, function)
   - _.reduce(array, function, initialValue)
   - _.reduceRight(array, function, initialValue)
   - _.shuffle(array)：返回一个打乱次序的集合。
   - _.invoke(array, methodName)：对集合的每个成员执行指定的操作。
   - _.sortBy(array, function)：根据处理函数的返回值返回一个排序后的集合，以升序排列。
   - _.indexBy(json, keyName)：返回一个对象，根据指定键名，对集合生成一个索引。
   - _.every(array, function)
   - _.some(array, function)
   - _.size(json)
   - _.sample(array)
   - _.filter(array, function)
   - _.reject(array, function)
   - _.find(array, function)：依次对集合的每个成员进行某种操作，返回第一个操作结果为true的成员。
   - _.contains(array, value)
   - _.countBy(array, function)：依次对集合的每个成员进行某种操作，将操作结果相同的成员算作一类，最后返回一个对象，表明每种操作结果对应的成员数量。
   - _.where(json, json)：检查集合中的每个值，返回一个数组，其中的每个成员都包含指定的键值对。
   - _.max(array, function)
   - _.min(array, function)
   - 【注】适用于数组和对象。
- 关于对象
   - _.toArray(json)
   - _.pluck(array, keyName)：将多个对象的某一个属性的值，提取成一个数组。
- 关于函数
   - _.bind(function, scope)
   - _.bindAll(scope, keyName1, ...)
   - _.partial(function, param1)
   - _.wrap(function, function)：将一个函数作为参数，传入另一个函数，最终返回前者的一个新版本。
   - _.compose(function, function)：接受一系列函数作为参数，由后向前依次运行，上一个函数的运行结果，作为后一个函数的运行参数。
   - _.memoize(function)
   - _.delay(function, time, param)
   - _.defer(function)：将函数推迟到待运行的任务数为0时再运行。
   - _.throttle(function, time)
   - _.debounce(function, time, boolean)：返回的新函数每次调用必须与上一次调用间隔一定的时间。参数boolean，表示立刻触发function还是等time后再触发。
   - _.once(function)：主要用于对象的初始化。
   - _.after(number, function)：返回的新版本函数，只有在被调用一定次数后才会运行，主要用于确认一组操作全部完成后，再做出反应。
- 关于工具方法
   - 链式操作
   - _.template(templateString[, jsonData][, options])
      - 变量使用 <%=...%>
      - 变量值包含[&, <, >, ", ', /]，转义使用 <%-...%>
      - JS命令使用 <%...%>



```javascript
	var fi = _.memoize(function(n){
		return n < 2 ? n : fi(n - 1) + fi(n - 2);
	});
```

<br />

<a name="vJuqT"></a>
# Grunt


- 相关文件
   - package.json
   - Gruntfile.js
- 命令行命令
   - grunt [模块名]
   - grunt [模块名:目标名]
   - grunt [任务名]
   - grunt
- npm install -g grunt-cli (command line interface)
- npm install [package] -D(--save-dev)
- npm install [package] -S(--save)
- grunt.initConfig(json)
- grunt.loadNpmTasks(moduleName)
- grunt.registerTask(taskName, useModuleList)
- 官方模块
   - grunt-contrib-clean
   - grunt-contrib-compass：使用compass编译sass文件。
   - grunt-contrib-sass
   - grunt-contrib-concat
   - grunt-contrib-copy
   - grunt-contrib-cssmin：压缩以及合并CSS文件。
   - grunt-contrib-imagemin
   - grunt-contrib-jshint
   - grunt-contrib-uglify：压缩以及合并JavaScript文件。
   - grunt-contrib-watch
   - grunt-autoprefixer：为CSS语句加上浏览器前缀。
   - grunt-htmlhint
   - grunt-markdown：将markdown文档转为HTML文档。
- 通配符
   - *：匹配任意数量的字符，不包括/。
   - ?：匹配单个字符，不包括/。
   - **：匹配任意数量的字符，包括/。
   - {}：允许使用逗号分隔的列表，表示“or”（或）关系。
   - !：用于模式的开头，表示只返回不匹配的情况。



```javascript
	// Gruntfile.js
	module.exports = function(grunt){
		grunt.initConfig({
			cssmin: {
				minify: {
					expand: true,
					cwd: "css/",
					src: ["*.css", "!*.min.css"],
					dest: "dest/",
					ext: ".min.css"
				},
				combine: {
					files: {
						"css/out.min.css": ["css/part1.min.css", "css/part2.min.css"]
					}
				}
			}
		});
		// require('load-grunt-tasks')(grunt);
		grunt.loadNpmTasks("grunt-contrib-cssmin");
		grunt.registerTask("default", ["cssmin:minify", "cssmin:combine"]);
	};
```

<br />

<a name="hLUvU"></a>
# Gulp


- 相关文件
   - package.json
   - gulpfile.js
- 命令行命令
   - gulp [任务名]
   - gulp
- npm install -g gulp
- npm install gulp --save-dev
- gulp.src(string|array)
   - js/app.js
   - js/*.js
   - js/**/*.js
   - !js/app.js
   - *.+(js css)
- gulp.dest(string, {cwd: cwd, mode: mode})
- gulp.task(taskName, taskDeps(并发执行), callback)
- gulp.watch(filePath, taskArray, callback)
   - watch.end()
   - watch.files()
   - watch.add(filePath, callback)
   - watch.remove(filePath)
   - change 事件
   - ready 事件
   - nomatch 事件
   - end 事件：回调函数运行完毕时触发。
   - error 事件



```javascript
	// gulpfile.js
	var gulp = require('gulp');
	var uglify = require('gulp-uglify');
	gulp.task('minify', function(){
		gulp.src('js/app.js')
		  .pipe(uglify()) // 压缩源码
		  .pipe(gulp.dest('build')); // 写入本地文件build.js
	});
```

<br />

<a name="gWWJV"></a>
# Source Map


- 启用SourceMap
   - //@ sourceMappingURL=/path/to/file.min.js.map
- VLQ编码(variable-length quantity)



```javascript
	// 格式
	{
		version: 3,
		file: "jquery.min.js",
		sourceRoot: "", // 转换前的文件所在的目录
		sources: ["jquery.js"],
		names: ["window", "undefined", "isArraylike"], // 转换前的所有变量名和属性名
		mappings: "AAgBC,SAAQ,CAAEA"
	}
```

<br />

<a name="u8SlO"></a>
# jQuery


- 资源加载
   - load 事件：所有资源（包括图片、flash、iframe）加载完成触发。
   - DOMContentLoaded 事件：所有DOM解析完成触发。
   - $(function(){}) 与 $(document).ready(function(){}) 等价，参数为 DOMContentLoaded 事件的回调函数。
- $.noConflict()
- $(selector, refSelector) 等效于 $(refSelector).find(selector)
- $(selector).html(string)
- $(selector).text(string)
- $(selector).html(function(index, cont){})
- $(selector).text(function(index, cont){})
- $(selector).removeAttr(string)：移除HTML属性
- $(selector).removeProp(string)：移除DOM属性
- $(selector).data(name, value)
- $(selector).append(string)
- $(selector).prepend(string)
- $(selector).after(string)
- $(selector).before(string)
- $(selector).wrap(string|function)
- $(selector).wrapAll(string)
- $(selector).unwrap(string)
- $(selector).wrapInner(string)
- 动画效果
   - jQuery.fx.off = true：关闭所有动画
   - jQuery.fx.speeds.normal = 1000
   - $(selector).animate(object, time, callback)
   - $(selector).stop()
   - $(selector).delay(time)
- $(selector).serialize()
- $(selector).serializeArray()
- 事件
   - $(selector).on(eventName.eventSpace, selector|data, callback)
   - $(selector).off(eventName.eventSpace)
   - $(selector).trigger(eventName.eventSpace)
- jQuery.fn === jQuery.prototype
- jQuery.Deferred对象
   - $.Deferred().done(function).fail(function).always(function).resolve(data)
   - then()会修改返回值，可以在调用其他回调函数之前，对前一步操作返回的值进行处理。
   - $.when(async1, async2).then(function(res1, res2){}, function(){})



```javascript
	// 深拷贝
	var o1 = {a: ['c1', 'c2']};
	var o2 = $.extend(true, {}, o1);
	
	// 插件
	;(function($, window){
		$.fn.plugin = function(options){
			// code goes here...
			// return this;
			options = $.extend({
				
			}, options);
			return this.each(function(index, item){
				
			});
		};
	})(jQuery, window);
	
	// 自定义操作
	var Twitter = {
		search: function(query){
			var df = $.Deferred();
			$.ajax({
				url: "",
				type: "",
				dataType: "",
				data: {},
				success: df.resolve
			});
			return df.promises();
		}
	};
	Twitter.search('script').then(function(res){
		
	});
```


