# JS 程序设计（上）



<a name="A2F07"></a>
# 概述


- JSON：JavaScript Object Notation(js对象表示法)
- JSONP：JavaScript Object Notation with padding(填充式JSON或参数式JSON)
- DTD：Document Type Declaration
- SVG：Scalable Vector Graphics(可缩放矢量图)
- Blob：Binary Large Object
```javascript
	// copy array( 浅拷贝 )
	var cloneArr = arr.concat();
	var cloneArr = [...arr];
	// copy json
	var cloneJson = JSON.parse(JSON.stringify(json));
```

<br />

<a name="VpOES"></a>
# JavaScript实现


- 【ECMAScript】核心（ECMA：欧洲计算机制造商协会）
- 【DOM】文档对象模型
- 【BOM】浏览器对象模型
- 宿主环境（ECMAScript）
   - Web浏览器
   - Node
   - Adobe Flash
- DOM级别
   - 【DOM 1级】DOM 1级的目标主要是映射文档的结构。由两个模块组成：DOM核心和DOM HTML。
   - 【DOM 2级】DOM 2级在原来DOM基础上又扩充了鼠标和用户界面事件、范围、遍历（迭代DOM文档的方法）等细分模块，而且通过对象接口增加了对CSS的支持。
   - 【DOM 3级】DOM 3级则进一步扩展了DOM，引入了以统一方式加载和保存文档的方法，新增了验证文档的方法。


<br />

<a name="TL2uZ"></a>
# 数据类型


- 基本类型值在内存中占据固定大小空间，因此保存在栈内存中；引用类型值是对象，保存在堆内存中。
- typeof是一个操作符而不是函数，因此圆括号尽管可以使用，但不是必须的。
   - undefined -- 如果值未定义
   - boolean -- 如果值是布尔
   - string -- 如果值是字符串
   - number -- 如果值是数值
   - object -- 如果值是对象或者null
   - function -- 如果值是函数
- Number类型
   - 最大值最小值 -- Number.MAX_VALUE, Number.MIN_VALUE
   - 正无穷负无穷 -- Infinity, -Infinity 或者 Number.NEGATIVE_INFINITY, Number.POSITIVE_INFINITY
   - 判断数值有穷 -- isFinite()
   - 判断参数是否不是数值 -- isNaN()
   - 数值转换 -- Number(), parseInt(), parseFloat() ( 其中一元加操作符等价于Number() )
- String类型
   - 字符字面量(转义序列)
   - \n 换行
   - \t 制表
   - \b 空格
   - \r 回车
   - \f 进纸
   - \\ 斜杠
   - \' 单引号
   - \" 双引号
   - \xnn 以十六进制表示的一个字符（\x41表示"A"）
   - \unnnn 以十六进制表示的一个Unicode字符（\u03a3表示"Σ"）
   - 字符串转换 -- String() ( 也可以使用"" + 值 )
- Object类型
   - Object实例的属性和方法
      - Object.prototype.constructor
      - Object.prototype.hasOwnProperty(propertyName)
      - Object.prototype.isPrototypeOf(object)
      - Object.prototype.propertyIsEnumerable(propertyName)
      - Object.prototype.toLocaleString()
      - Object.prototype.toString()：返回对象的字符串表示
      - Object.prototype.valueOf()：返回对象的字符串、数值、布尔值表示
```javascript
	var d = new Date('2018', '1', '18');
	typeof d === 'object' && d instanceof Date // true
	
	// Param2 进制里的 Param1 是十进制里的多少
	parseInt("070", 10) // 70
	parseInt(070, 10) // 56
	
	// 十进制里的 num 是 Param1 进制里的多少
	var num = 10;
	num.toString(2) // "1010"
	num.toString(10) // "10"
	
	// 确保循环自身属性
	for(var i in obj){
		if(obj.hasOwnProperty(i)){
			
		}
	}
```

<br />

<a name="iA0pC"></a>
# 操作符


- 关系操作符
   - 如果两个操作数都是字符串，则比较两个操作数对应的字符编码值。
   - 任何操作数与NaN进行关系比较，结果都是false。
- 相等操作符
   - null和undefined相等。
   - 如果两个操作数都是对象，则比较他们是不是同一个对象，如果都指向同一个对象，则返回true。
- 逗号操作符
   - 逗号操作符多用于声明多个变量。
   - 逗号操作符还可以用于赋值，用于赋值时，总是返回表达式中的最后一项。
```javascript
	"abc" > "ABHI" // true
	"235" > "5" // false
	NaN > 1 // false
	NaN <= 1 // false
	
	null == undefined // true
	0 == undefined // false
	null == 0 // false
	null == true // false
	null == false // false
```

<br />

<a name="yY8RV"></a>
# 垃圾收集


- 标记清除（使用）
- 引用计数（循环引用问题）


<br />

<a name="jk5uW"></a>
# 函数


- ECMAScript中所有函数都是按值传递参数；访问变量有按值和按引用两种方式。
- 由于不存在函数签名的特性，ECMAScript函数不能重载。如果在ECMAScript中定义了两个函数，则该名字只属于后定义的函数。
- 通过检查传入函数中参数的类型和数量并作出不同的反应，可以模仿方法的重载。


<br />

<a name="395Xv"></a>
# 数组


- 栈数据结构的访问规则：LIFO(后进先出)，队列数据结构的访问规则：FIFO(先进先出)。
- valueOf()
- toString()
- toLocaleString()
- join()
- <br />
- 【检测数组】typeof arr 返回 object
- 【检测数组】arr instanceof Array 返回 true，instanceof操作符的问题在于它假定只有一个全局环境。（[] instanceof window.iframe[0].Array 返回 fasle，因为Array.prototype !== window.iframe[0].Array.prototype）
- 【检测数组】为了解决这个问题，ECMAScript 5新增了Array.isArray()方法；或者使用 Object.prototype.toString.call(arr) === "[object Array]" 判断。
- <br />
- 【栈方法】push() -- 返回新数组长度
- 【栈方法】pop() -- 返回移除项
- 【队列方法】unshift() -- 返回新数组长度
- 【队列方法】shift() -- 返回移除项
- 【重排序方法】reverse() -- 反转数组项顺序，返回新数组
- 【重排序方法】sort(fn) -- 比较函数接收两个参数，如果第一个参数应该位于第二个参数之前返回负数，反之返回正数，相等返回0，返回新数组
- 【操作方法】concat() -- 返回新数组，不影响原数组
- 【操作方法】slice() -- 返回新数组，不影响原数组（如果参数中有负数则用数组长度加上该数确定位置，如果结束位置小于起始位置则返回空数组）
- 【操作方法】splice() -- 返回移除项
- 【位置方法】indexOf(查找的项， 查找的起始位置索引)
- 【位置方法】lastIndexOf(查找的项， 查找的起始位置索引)
- <br />
- ECMAScript 5 为数组定义了 5 个迭代方法，每个方法接收两个参数：要在每一项上运行的函数、（可选）运行该函数的作用域对象 -- 影响 this 的值
- 【迭代方法】every() -- 对数组中的每一项运行给定函数，如果该函数对每一项都返回 true ，则返回 true 。
- 【迭代方法】some() -- 对数组中的每一项运行给定函数，如果该函数对任一项返回 true ，则返回 true 。
- 【迭代方法】filter() -- 对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
- 【迭代方法】map() -- 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
- 【迭代方法】forEach() -- 对数组中的每一项运行给定函数，本方法没有返回值。
- <br />
- ECMAScript 5 为数组定义了 2 个归并方法，每个方法接收两个参数：要在每一项上运行的函数、（可选）作为归并基础的初始值。
- 【归并方法】reduce() -- 迭代数组中的所有值，然后构建一个最终返回的值。
- 【归并方法】reduceRight() -- 迭代数组中的所有值，然后构建一个最终返回的值。
```javascript
	var arr = [54,65,87,87,456,786,23,54];
	arr.sort(function(a1, a2){
		if(a1 > a2) return 1;
		if(a1 < a2) return -1;
		return 0;
	});
	
	arr.slice(-5, -3); // [87, 456]
	arr.slice(-3, -5); // []
	
	arr.indexOf(87); // 2
	arr.lastIndexOf(87); // 3
	
	arr.every(function(item, index, array){
		return (item > 100);
	});
	arr.some(function(item, index, array){
		return (item > 100);
	});
	arr.filter(function(item, index, array){
		return (item > 100);
	});
	arr.map(function(item, index, array){
		return (item * 100);
	});
	arr.forEach(function(item, index, array){
		console.log(JSON.stringify(array) + '--' + index + '--' + item);
	});
	
	arr.reduce(function(prev, cur, index, array){
		return (prev + cur);
	});
	arr.reduceRight(function(prev, cur, index, array){
		return (prev + cur);
	});
```

<br />

<a name="RrGoa"></a>
# RegExp类型


- valueOf()
- toString()
- toLocaleString()
- exec()
- test()
- 由于 RegExp 构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。（例如 \n ，字符 \ 在字符串中通常被转义成 \\ ，而在正则表达式中就会变成 \\\\。）
- 在 ECMAScript 3 中，使用正则表达式字面量和使用 RegExp 构造函数创建的正则表达式不一样，正则表达式字面量始终共享同一个 RegExp 实例，ECMAScript 5 做出了修改。
- 匹配模式
   - i：不区分大小写模式
   - m：多行模式
   - g：全局模式
- 元字符（必须转义）
   - ( ) [ ] { } ^ $ ? * | \ . +
   - 【注】点字符（.）匹配除回车（\r）、换行(\n) 、行分隔符（\u2028）和段分隔符（\u2029）以外的所有字符。
```javascript
	/*
	 exec()
	 返回包含第一个匹配项的数组，或者在没有匹配项时返回 null ，包含两个额外属性（input, index），在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串。
	 即使在模式中设置了全局标志，每次也只返回一个匹配项。在设置全局标志的情况下，每次调用 exec() 都会在字符串中查找新匹配项。
	 
	 match()
	 字符串方法
	 返回包含第一个匹配项的数组，或者在没有匹配项时返回 null ，包含两个额外属性（input, index），在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串。
	 在模式中设置了全局标志，则返回的数组包含所有匹配项，没有捕获项匹配项。
	 */
	var str = 'mom and dad and baby';
	var reg = /mom( and dad( and baby)?)?/ig;
	var matches = reg.exec(str);
	console.log(matches);
	console.log(matches.index);
	console.log(matches.input);
	
	var str = 'cat bat sat fat';
	var reg = /(.)at/g;
	var matches = reg.exec(str);
	console.log(reg.lastIndex);
	console.log(matches);
	console.log(matches.index);
	console.log(matches.input);
	
	
	/*
	 RegExp 构造函数属性
	 */
	var str = 'this has been a short summer';
	var reg = /(.)hort/g;
	if(reg.test(str)){
		console.log(RegExp.input);
		console.log(RegExp.lastMatch);
		console.log(RegExp.lastParen);
		console.log(RegExp.leftContext);
		console.log(RegExp.rightContext);
		console.log(RegExp.multiline);
		
		console.log(RegExp.$1);
		console.log(RegExp.$9);
	}
```

<br />

<a name="eDWFU"></a>
# Function类型


- 定义方式三种
- valueOf()
- toString()
- toLocaleString()
- 函数内部属性：arguments，this
- 函数属性：length，prototype
- 函数方法
   - call(scope, param1, param2, ...)
   - apply(scope, arguments)（apply(scope, [param1, param2, ...])）
   - ECMAScript 5 还定义了一个方法：bind(scope, param1, param2, ...)，返回新函数需要再次调用才可执行。
   - 【注1】在非严格模式下，使用函数的call()或apply()方法时，null或undefined值会被转换为全局对象。
   - 【注2】如果call方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象。
   - 【注3】bind方法每运行一次，就返回一个新函数。
- ------
- 闭包：指有权访问另一个函数作用域中变量的函数。
- this 和 arguments 也存在同样的问题，如果想访问作用域中的 arguments 对象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。
- 匿名函数可以用来模仿块级作用域。
```javascript
	// 函数声明
	function sum(num1, num2){
		return num1 + num2;
	}
	// 函数表达式
	var sum = function(num1, num2){
		return num1 + num2;
	};
	var sum = new Function('num1', 'num2', 'return num1 + num2');
	
	function compare(propertyName){
		return function(obj1, obj2){
			var val1 = obj1[propertyName],
				val2 = obj2[propertyName];
			if(val1 < val2)
				return -1;
			if(val1 > val2)
				return 1;
			return 0;
		};
	}
	var data = [{"name": "afg", "age": 18}, {"name": "ghi", "age": 8}];
	data.sort(compare("age"));
	
	/*
	 arguments有一个名叫 callee 的属性，该属性是一个指针，指向这个拥有 arguments 对象的函数。
	 严格模式下访问 arguments.callee 报错。
	 
	 ECMAScript 5 也规范化了另一个函数对象的属性 caller 。
	  如果在全局作用域调用当前函数，它的值为 null 。
	 */
	function outer(){
		inner();
	}
	function inner(){
		console.log(arguments.callee.caller);
	}
	outer();
	// 递归
	function factorial(num){
		if(num <= 1)
			return 1;
		return num * arguments.callee(num - 1);
	}
	var factorial = (function f(num){
		if(num < 1)
			return 1;
		return num * f(num - 1);
	});
	// 闭包
	function createFunctions(){
		var result = [];
		for(var i = 0; i < 10; i++){
			result[i] = function(num){
				return function(){
					return num;
				};
			}(i);
		}
		return result;
	}
	
	// 模块模式
	var application = function(){
		// 私有变量和函数
		var components = new Array();
		function BaseComponent(){
			
		}
		// 初始化
		components.push(new BaseComponent());
		// 公共接口
		return {
			getComponentCount: function(){
				return components.length;
			},
			registerComponent: function(component){
				if(typeof component == "object"){
					components.push(component);
				}
			}
		};
	}();
	// 增强的模块模式
	var application = function($, window, document){
		// 私有变量和函数
		var components = new Array();
		function BaseComponent(){
			
		}
		// 初始化
		components.push(new BaseComponent());
		// 创建 application 的一个局部副本
		var app = new BaseComponent();
		// 公共接口
		app.getComponentCount = function(){
			return components.length;
		};
		app.registerComponent = function(component){
			if(typeof component == "object"){
				components.push(component);
			}
		};
		// 返回这个副本
		return app;
	}(jQuery, window, document);
	
	var o = {"color": "blue"};
	function sayColor(p1, p2){
		console.log(this.color, p1, p2);
	}
	sayColor = sayColor.bind(o, 200);
	sayColor(500);
	
	var obj = {
		name: 'kerwin',
		list: [1, 2, 3],
		print: function(){
			this.list.forEach(function(item, index, list){
				console.log(this.name);
			}.bind(this));
		}
	};
	obj.print();
	
	Array.prototype.push.call([1, 2, 3], 0, 1);
	Function.prototype.call.bind(Array.prototype.push)([1, 2, 3], 0, 1);
```

<br />

<a name="wMGpa"></a>
# Number类型


- 返回小数表示字符串：toFixed(digit)
- 返回指数表示字符串：toExponential(digit)
- 返回合适格式字符串：toPrecision(digit)


<br />

<a name="mncXV"></a>
# String类型


- ECMAScript 5 为所有字符串定义了 trim()。
- 比较两个字符串方法 localeCompare()。
- String 构造函数本身还有一个静态方法 fromCharCode()。
- 字符方法
   - charAt(index)（ECMAScript 5 还定义了另一个可以使用方括号加数字索引访问字符串中的特定字符）
   - charCodeAt(index)
- 字符串操作方法
   - concat(string)（或者使用 + 操作符）
   - slice(start, end)
   - substring(start, end)
   - substr(start, length)
   - 【注1】在传递给 slice() substring() substr() 负数的情况下，slice() 会将负值与字符串长度相加，substring() 会把负值参数转换为 0（两个参数中较小的数作为起始位置），substr() 会将第一个参数与字符串长度相加，第二个参数转换为 0。
   - 【注2】如果第二个参数大于第一个参数，substring方法会自动更换两个参数的位置。
- 字符串位置方法
   - indexOf(character[, index])
   - lastIndexOf(character[, index])
- 字符串大小写转换方法
   - toLowerCase()
   - toLocaleLowerCase()
   - toUpperCase()
   - toLocaleUpperCase()
- 字符串模式匹配方法
   - match(regexp)
   - search(regexp(| string))
   - replace(regexp( | string)[, function( | string)])
   - split(regexp( | string)[, arraysize])（regexp 支持因浏览器而异）
```javascript
	var str = "cat, bat, sat, fat";
	var result = str.replace(/(.at)/g, "word ($1)");
	console.log(result);
	
	function htmlEscape(text){
		return text.replace(/([<>&"])/g, function(match, $1, index, string){
			switch(match){
				case "<":
					return "<";
				case ">":
					return ">";
				case "&":
					return "&";
				case "\"":
					return """;
			}
		});
	}
	var str = "<p class=\"greeting\">hello world</p>";
	console.log(htmlEscape(str));
	
	var str = "red,green,blue,purple";
	var colors1 = str.split(",", 2);
	var colors2 = str.split(/[^\,]+/);
	
	var str = "good";
	var result1 = str.localeCompare('hand'); // 返回一个负值
	var result2 = str.localeCompare('compare'); // 返回一个正值
	var result = String.fromCharCode('100', '101', '102');
```

<br />

<a name="Zz3ps"></a>
# 单体内置对象


- URI编码方法
   - encodeURI()
   - encodeURIComponent()
   - decodeURI()
   - decodeURIComponent()
   - 【注1】该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。该方法的目的是对 URI 进行完整的编码，不会对本身属于URI的特殊字符进行编码。因此对以下在 URI 中具有特殊含义的 ASCII 标点符号，encodeURI() 函数是不会进行转义的：;/?:@&=+$,#。如果 URI 组件中含有分隔符，比如 ? 和 #，则应当使用 encodeURIComponent() 方法分别对各组件进行编码。
   - 【注2】该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。其他字符（比如 ：;/?:@&=+$,# 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的。请注意 encodeURIComponent() 函数 与 encodeURI() 函数的区别之处，前者假定它的参数是 URI 的一部分（比如协议、主机名、路径或查询字符串）。因此 encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号。
- eval()
   - 在 eval() 中创建的任何变量或函数都不会被提升。它们只在 eval() 执行的时候创建。
   - 严格模式下，在外部访问不到 eval() 中创建的任何变量或函数。
- global(Web浏览器中作为window对象的一部分)对象属性
   - undefined
   - NaN
   - Infinity
   - Object
   - Boolean
   - Number
   - String
   - Function
   - Array
   - Date
   - RegExp
   - Error
   - EvalError
   - RangeError
   - ReferenceError
   - SyntaxError
   - TypeError
   - URIError
- Math对象
   - Math.E
   - Math.LN2
   - Math.LN10
   - Math.LOG2E
   - Math.LOG10E
   - Math.PI
   - Math.SQRT1_2
   - Math.SQRT2
   - ------
   - Math.min()
   - Math.max()
   - Math.floor()
   - Math.ceil()
   - Math.round()
   - Math.random()
   - Math.abs()
   - Math.exp()
   - Math.log()
   - Math.pow(num, power)
   - Math.sqrt()
   - Math.asin(x)
   - Math.scos(x)
   - Math.atan(x)
   - Math.atan2(y, x)
   - Math.sin(x)
   - Math.cos(x)
   - Math.tan(x)
```javascript
	// 获取数组最大项
	var arr = [1,4,5,67,8];
	Math.max.apply(null, arr);
	
	// 获取指定范围随机数
	function getRandom(lowerValue, upperValue){
		var total = upperValue - lowerValue + 1;
		return Math.floor(Math.random() * total + lowerValue);
	}
```

<br />

<a name="6bBI2"></a>
# 面向对象程序设计


- 属性类型
   - 数据属性
   - [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接在对象上定义的属性，默认值为 true。
   - [[Enumerable]]：表示能否通过 for-in 循环返回属性。直接在对象上定义的属性，默认值为 true。
   - [[Writable]]：表示能否修改属性的值。直接在对象上定义的属性，默认值为 true。
   - [[Value]]：包含这个属性的数据值。默认值为 undefined。
   - ------
   - 访问器属性
   - [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。直接在对象上定义的属性，默认值为 true。
   - [[Enumerable]]：表示能否通过 for-in 循环返回属性。直接在对象上定义的属性，默认值为 true。
   - [[Get]]：在读取属性时调用的函数。默认值为 undefined。
   - [[Set]]：在写入属性时调用的函数。默认值为 undefined。
- 定义属性
   - Object.defineProperty()
   - Object.defineProperties()
   - 【注】通过本函数定义的属性，其writable、configurable、enumerable这三个属性的默认值都为false。
- 读取属性的特性
   - Object.getOwnPropertyDescriptor()
- 创建对象
   - 工厂模式
   - 构造函数模式
   - 原型模式
   - 【注01】使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。
   - 【注02】Person.prototype.constructor === person.constructor === Person。
   - 【注03】Person.prototype.isPrototypeOf(person)
   - 【注04】当调用构造函数创建一个新实例后，该实例内部将包含一个指针指向构造函数的原型对象，ECMAScript 5 称作 [[Prototype]]，虽然在脚本中没标准的方式访问，但 Firefox, Safari, Chrome 在每个对象上都支持一个属性 __proto__。
   - 【注05】hasOwnProperty() 方法可以检测一个属性是存在实例中还是存在原型中。
   - 【注06】in 操作符会在通过对象可以访问给定属性时返回 true，无论该属性在实例中或原型中。
   - 【注07】枚举属性：for-in(包括继承自原型对象的属性), Object.keys()(只返回对象本身的属性), Object.getOwnPropertyNames()(返回对象自身的所有属性，不管是否可枚举)。
   - 【注08】【组合使用构造函数模式和原型模式】构造函数模式用于定义实例属性，原型模式用于定义共享属性和方法。
   - 【注09】【动态原型模式】把所有信息都封装在构造函数中，仅在必要的情况下在构造函数中初始化原型。
- 继承
   - 接口继承：继承方法签名
   - 实现继承：继承实际的方法（ECMAScript 只支持实现继承）
   - 【注01】原型链问题一：包含引用类型值的原型。
   - 【注02】原型链问题二：在创建子类型的实例时，不能向超类型的构造函数中传递参数。
   - 【注03】【借用构造函数】有时也称作伪造对象或经典继承。在解决原型中包含引用类型值所带来的问题中，它通过使用 call() 和 apply() 在子类型构造函数的内部调用超类型构造函数。很少单独使用。
   - 【注04】【组合继承】有时也称作伪经典继承。其背后的思路是使用原型链实现对原型属性和方法的继承，通过借用构造函数实现对实例属性的继承。缺点是两次调用超类型构造函数。
   - 【注05】【寄生组合式继承】即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。
   - 【注06】【原型式继承】ECMAScript 5 通过 Object.create() 规范化了原型式继承。其参数与 Object.defineProperties() 方法格式相同。
```javascript
	/*
	 在把 configurable 特性设置为 false 之后不可改回。可配置性决定了一个变量是否可以被删除（delete）。当使用var命令声明变量时，变量的configurable为false。
	 只要 writable 和 configurable 有一个为 true ，就允许改 value 。
	 在调用 Object.defineProperty() 方法时，如果不指定，configurable, enumerable, writable 特性的默认值为 false。
	 */
	var a = 1;
	Object.getOwnPropertyDescriptor(this, 'a'); // {value: 1, writable: true, enumerable: true, configurable: false}
	var person = Object.defineProperty({}, "name", {
		value: "test",
		writable: false
	});
	var person1 = Object.create(person, {
		userInfo: {
			get: function(){
				return 'name: ' + this.name + '; age: ' + (this.age ? this.age : 0);
			},
			set: function(age){
				this.age = age;
			}
		}
	});
	
	/*
	 一旦定义了取值函数get（或存值函数set），就不能将writable设为true，或者同时定义value属性，会报错。
	 只指定 getter 函数意味着属性不可写，尝试写入属性会被忽略，只指定 setter 函数的属性不可读，尝试读取会返回 undefined。在严格模式下都会抛出错误。
	 */
	var book = {
		_year: 2004,
		edition: 1
	};
	// 定义访问器的旧有方法
	// 在不支持 Object.defineProperty() 的浏览器中不能修改 [[Configurable]], [[Enumerable]]。
	book.__defineGetter__("year", function(){
		return this._year;
	});
	book.__defineSetter__("year", function(newValue){
		if(newValue > 2004){
			this._year = newValue;
			this.edition += newValue - 2004;
		}
	});
	Object.defineProperty(book, "year", {
		get: function(){
			return this._year;
		},
		set: function(newValue){
			if(newValue > 2004){
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}
	});
	book.year = 2018;
	console.log(book.edition);
	
	var book = Object.defineProperties({}, {
		_year: {
			value: 2004
		},
		edition: {
			value: 1
		},
		year: {
			get: function(){
				return this._year;
			},
			set: function(newValue){
				if(newValue > 2004){
					this._year = newValue;
					this.edition += newValue - 2004;
				}
			}
		}
	});
	var descriptor1 = Object.getOwnPropertyDescriptor(book, "_year");
	var descriptor2 = Object.getOwnPropertyDescriptor(window, "location");
	
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
	}
	Person.prototype.sayName = function(){
		console.log(this.name);
	};
	// 1. 当做构造函数使用
	var person = new Person("N", "18", "Engineer");
	person.sayName();
	// 2. 当做普通函数使用
	Person("N", "18", "Engineer");
	window.sayName();
	// 3. 在另一个对象作用域中调用
	var o = {};
	Person.call(o, "N", "18", "Engineer");
	o.sayName();
	
	// 动态原型模式
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		if(typeof this.sayName !== "function"){
			Person.prototype.sayName = function(){
				console.log(this.name);
			};
		}
	}
	
	// 组合继承
	function SuperFn(name){
		this.name = name;
		this.colors = ["blue", "purple"];
	}
	SuperFn.prototype.sayName = function(){
		console.log(this.name);
	};
	function SubFn(name, age){
		SuperFn.call(this, name);
		this.age = age;
	}
	SubFn.prototype = new SuperFn();
	SubFn.prototype.constructor = SubFn;
	SubFn.prototype.sayAge = function(){
		console.log(this.age);
	};
	var ins1 = new SubFn("aa", 18);
	ins1.colors.push("black");
	console.log(ins1.colors);
	ins1.sayName();
	ins1.sayAge();
	var ins2 = new SubFn("cc", 28);
	console.log(ins2.colors);
	ins2.sayName();
	ins2.sayAge();
	
	// 寄生组合式继承
	// Object.create()
	function createObject(o){
		function F(){}
		F.prototype = o;
		return new F();
	}
	function inheritPrototype(SuperFn, SubFn){
		var prototype = createObject(SuperFn.prototype); // 创建对象
		prototype.contructor = SubFn; // 增强对象
		SubFn.prototype = prototype; // 指定对象
	}
	function Super(name){
		this.name = name;
		this.colors = ["blue", "purple"];
	}
	Super.prototype.sayName = function(){
		console.log(this.name);
	};
	function Sub(name, age){
		Super.call(this, name);
		this.age = age;
	}
	inheritPrototype(Super, Sub);
	Sub.prototype.sayAge = function(){
		console.log(this.age);
	};
```

<br />

<a name="nixjU"></a>
# BOM


- 窗口关系及框架
   - top.frames
   - 与 top 相对的另一个 window 对象是 parent。
   - 与框架有关的最后一个对象是 self。
   - 【注】window.frames[0].frameElement
- window对象属性
   - window.name
   - window.length === window.frames.length
   - window.frames === window
   - window.screenX、window.screenY：返回浏览器窗口左上角相对于当前屏幕左上角的水平距离和垂直距离。
   - window.innerWidth、window.innerHeight：返回网页在当前窗口中可见部分的高度和宽度，即“视口”，包括滚动条的高度和宽度。
   - window.outerWidth、window.outerHeight：返回浏览器窗口的高度和宽度，包括浏览器菜单和边框。
   - window.pageXOffset、window.pageYOffset：返回页面的水平滚动距离和垂直滚动距离。
   - window.screen.width、window.screen.height：一般用来了解屏幕的分辨率。
   - window.screen.colorDepth：返回屏幕的颜色深度，一般为16（表示16-bit）或24（表示24-bit）。
- 获取选中文本
   - window.getSelection().toString()
- 窗口位置
- 视口大小
- 导航和打开窗口
   - window.open(URL, 窗口目标, 特性字符串, 是否取代历史记录中当前页面)
   - 窗口目标：name 值或者 _self, _parent, _top, _blank。
   - window.close()
- 系统对话框
   - alert(string)
   - confirm(string)
   - prompt(string, defaultValue)
   - window.find()
   - window.print()
- location对象
   - 属性
   - window.location.href
   - window.location.hash
   - window.location.search
   - window.location.protocol
   - window.location.host
   - window.location.hostname
   - window.location.port
   - window.location.pathname
   - ------
   - 方法
   - window.location.assign(URL)
   - window.location.reload(boolean)
   - window.location.replace(URL)
- navigator对象
   - window.navigator.cookieEnabled
   - window.navigator.language
   - window.navigator.mimeTypes
   - window.navigator.onLine
   - window.navigator.platform
   - window.navigator.plugins
   - window.navigator.userAgent
- 历史状态管理
   - popstate 事件
   - e.state
   - window.history.length
   - window.history.go(number( | string))
   - window.history.back()
   - window.history.forward()
   - window.history.pushState(stateObject, stateTitle[, stateURL])
   - window.history.replaceState(stateObject, stateTitle[, stateURL])
```javascript
	// 窗口位置
	var leftPos = window.screenLeft || window.screenX;
	var topPos = window.screenTop || window.screenY;
	// 视口大小
	var pageWidth = document.documentElement.clientWidth || document.body.clientWidth;
	var pageHeight = document.documentElement.clientHeight || document.body.clientHeight;
```

<br />

<a name="2vFbz"></a>
# 客户端检测


- 能力检测
   - 在可能的情况下，要尽量使用 typeof 进行能力检测。
- 用户代理检测
```javascript
	var client = function(){
		// 呈现引擎
		var engine = {
			ie: 0,
			gecko: 0,
			webkit: 0,
			khtml: 0,
			opera: 0,
			// 完整的版本号
			ver: null
		};
		
		// 浏览器
		var browser = {
			// 主要浏览器
			ie: 0,
			firefox: 0,
			safari: 0,
			konq: 0,
			opera: 0,
			chrome: 0,
			// 具体的版本号
			ver: null
		};
		
		// 平台、设备和操作系统
		var system = {
			win: false,
			mac: false,
			unix: false,
			// 移动设备
			iphone: false,
			ipod: false,
			ipad: false,
			ios: false,
			android: false,
			nokiaN: false,
			winMobile: false,
			// 游戏系统
			wii: false,
			ps: false
		};
		
		// 检测呈现引擎和浏览器
		var ua = window.navigator.userAgent;
		if(window.opera){
			engine.ver = browser.ver = window.opera.version();
			engine.opera = browser.opera = parseFloat(engine.ver);
		}else if(/AppleWebKit\/(\S+)/.test(ua)){
			engine.ver = RegExp["$1"];
			engine.webkit = parseFloat(engine.ver);
			// 确定是Chrome还是Safari
			if(/Chrome\/(\S+)/.test(ua)){
				browser.ver = RegExp["$1"];
				browser.chrome = parseFloat(browser.ver);
			}else if(/Version\/(\S+)/.test(ua)){
				browser.ver = RegExp["$1"];
				browser.safari = parseFloat(browser.ver);
			}else{
				// 近似的确定版本号
				var safariVersion = 1;
				if(engine.webkit < 412){
					safariVersion = 1.3;
				}else{
					safariVersion = 2;
				}
				browser.ver = browser.safari = safariVersion;
			}
		}else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
			engine.ver = browser.ver = RegExp["$1"];
			engine.khtml = browser.konq = parseFloat(engine.ver);
		}else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){
			engine.ver = RegExp["$1"];
			engine.gecko = parseFloat(engine.ver);
			// 确定是不是Firefox
			if(/Firefox\/(\S+)/.test(ua)){
				browser.ver = RegExp["$1"];
				browser.firefox = parseFloat(browser.ver);
			}
		}else if(/MSIE ([^;]+)/.test(ua)){
			engine.ver = browser.ver = RegExp["$1"];
			engine.ie = browser.ie = parseFloat(engine.ver);
		}
		
		// 检测浏览器
		browser.ie = engine.ie;
		browser.opera = engine.opera;
		
		// 检测平台
		var p = window.navigator.platform;
		system.win = p.indexOf("Win") == 0;
		system.mac = p.indexOf("Mac") == 0;
		system.unix = (p.indexOf("Linux") == 0) || (p == "xll");
		
		// 检测Windows操作系统
		if(system.win){
			if(/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
				if(RegExp["$1"] == "NT"){
					switch(RegExp["$2"]){
						case "5.0":
							system.win = "2000";
							break;
						case "5.1":
							system.win = "XP";
							break;
						case "6.0":
							system.win = "Vista";
							break;
						case "6.1":
							system.win = "7";
							break;
						case "6.2":
							system.win = "8";
							break;
						default:
							system.win = "NT";
							break;
					}
				}else if(RegExp["$1"] == "9x"){
					system.win = "ME";
				}else{
					system.win = RegExp["$1"];
				}
			}
		}
		
		// 移动设备
		system.iphone = ua.indexOf("iPhone") > -1;
		system.ipod = ua.indexOf("iPod") > -1;
		system.ipad = ua.indexOf("iPad") > -1;
		system.nokiaN = ua.indexOf("NokiaN") > -1;
		
		// Window Mobile
		if(system.win == "CE"){
			system.winMobile = system.win;
		}else if(system.win == "Ph"){
			if(/Window Phone OS (\d+\.\d+)/.test(ua)){
				system.win = "Phone";
				system.winMobile = parseFloat(RegExp["$1"]);
			}
		}
		
		// 检测iOS版本
		if(ua.indexOf("Mobile") > -1){
			if(/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)){
				system.ios = parseFloat(RegExp.$1.replace("_", "."));
			}else{
				system.ios = 2;
			}
		}
		
		// 检测Android版本
		if(/Android (\d+\.\d+)/.test(ua)){
			system.android = parseFloat(RegExp["$1"]);
		}
		
		// 游戏系统
		system.wii = ua.indexOf("Wii") > -1;
		system.ps = /playstation/i.test(ua);
		
		// 返回对象
		return {
			engine: engine,
			browser: browser,
			system: system
		};
	}();
```
```javascript
function getClientInfo(){
    var u = window.navigator.userAgent;
    return {//移动终端浏览器信息
        trident: u.indexOf('Trident') > -1,//IE内核
        presto: u.indexOf('Presto') > -1,//opera内核
        webkit: u.indexOf('AppleWebKit') > -1,//苹果、谷歌内核
        gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1,//火狐内核
        mobile: !!u.match(/AppleWebKit.*Mobile.*/),//是否为移动终端
        ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),//ios终端
        android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1,//android终端
        iphone: u.indexOf('iPhone') > -1,//是否为iPhone
        ipad: u.indexOf('iPad') > -1,//是否iPad
        webapp: u.indexOf('Safari') == -1,//是否web应该程序，没有头部与底部
        weixin: u.toLowerCase().match(/MicroMessenger/i) == 'micromessenger',//是否为微信
        taobao: u.indexOf('AliApp(TB') > -1
    };
}
```


