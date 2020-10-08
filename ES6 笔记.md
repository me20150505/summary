# ES6 笔记



<a name="Yk0O5"></a>
# 简介

- Babel 转码器：将ES6转为ES5。
   - 配置文件.babelrc
      - npm install babel-preset-latest --save-dev
      - npm install babel-preset-react --save-dev
      - npm install babel-preset-stage-3 --save-dev
   - 命令行转码babel-cli
      - npm install -g babel-cli
      - babel [name] --out-file [name]
      - babel [name] -o [name]
      - babel [path] --out-dir [path]
      - babel [path] -d [path]
      - babel [path] -d [path] -s：生成source map文件


<br />

<a name="jIJ1S"></a>
# let 和 const 命令


- 变量声明
   - var 命令
   - function 命令
   - let 命令
   - const 命令
   - import 命令
   - class 命令
- let 命令
   - let 命令只在所在的代码块内有效。
   - let 命令不存在变量提升。
   - “暂时性死区”也意味着typeof不再是一个百分之百安全的操作。
   - let不允许在相同作用域内，重复声明同一个变量。
   - 块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。
- const 命令
   - 对于复合类型的数据（主要是对象和数组），const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。



```javascript
	// ES5
	(function(){
		var test = '';
	}());
	
	// ES6
	{
		let test = '';
	}
	
	// 获取顶层对象
	var getGlobal = function(){
		return typeof self !== 'undefined' ? 
			self : 
			typeof window !== 'undefined' ? 
			window : 
			typeof global !== 'undefined' ? 
			global : 
			throw new Error('unable to locate global object');
	};
	var getGlobal = function(){
		typeof window !== 'undefined' ? 
			window : 
			(typeof process === 'object' && 
			typeof require === 'function' && 
			typeof global === 'object') ? 
			global : 
			this;
	};
```

<br />

<a name="Dp8lR"></a>
# 变量的解构赋值


- 数组的解构赋值
- 对象的解构赋值
- 字符串、数值、布尔值的解构赋值
- 函数参数的解构赋值
- 解构赋值允许指定默认值：默认值生效的条件是，对象的属性值严格等于undefined。



```javascript
	let {log, sin, cos} = Math;
	let {length: len} = 'hello';
	
	// 函数参数的默认值
	jQuery.ajax = function(url, {
		async = true,
		beforeSend = function(){},
		cache = true,
		complete = function(){}
	} = {}){
		
	};
```

<br />

<a name="gp9SM"></a>
# 字符串的扩展


- 概述
   - 字符的 Unicode 表示法：只要将码点放入大括号内即能正确解读该字符。
   - 模板字符串
   - 标签模版：模板字符串可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。这被称为“标签模板”功能。
- String.fromCodePoint(code)
- String.raw()：可以作为处理模板字符串的基本方法，它会将所有变量替换，方便下一步作为字符串来使用。
- str.codePointAt(index)：正确处理四字节字符。
- str.includes(str, pos)
- str.startsWith(str, pos)
- str.endsWith(str, pos)
- str.repeat(count)
- str.padStart(len, str)：字符串补全指定长度，默认使用空格补全。
- str.padEnd(len, str)
- str.matchAll(reg)：返回的是一个遍历器（Iterator），而不是数组。
- str.normalize(param)：将字符的不同表示方法统一为同样的形式，称为 Unicode 正规化。
- 【注1】codePointAt方法返回的是码点的十进制值，如果想要十六进制的值，可以使用toString方法转换一下。
- 【注2】for...of循环，会正确识别 32 位的 UTF-16 字符。



```javascript
	var s = "\u{20BB7}";
	
	'09-12'.padStart(10, 'YYYY-MM-DD');
	
	String.raw`Hi\n${2+3}`;
	String.raw({raw: 'test'}, 0, 1, 2);
	String.raw({raw: ['t', 'e', 's', 't']}, 0, 1, 2);
	
	function getStrLen(str){
		return [...str].length;
	}
```

<br />

<a name="EHx3T"></a>
# 数值的扩展


- 概述
   - 指数运算符 **：右结合性。
- Number.EPSILON：根据规格，它表示 1 与大于 1 的最小浮点数之间的差。对于 64 位浮点数来说，等于 2 的 -52 次方。
- Number.MAX_SAFE_INTEGER
- Number.MIN_SAFE_INTEGER
- Number.isSafeInteger()
- Number.isFinite()
- Number.isNaN()
- Number.parseInt()
- Number.parseFloat()
- Number.isInteger()：如果参数非数值则返回false。
- Math对象的扩展
   - Math.trunc()：用于去除一个数的小数部分，返回整数部分。
   - Math.sign()：用来判断一个数是正数、负数、还是零。
   - Math.cbrt()：用于计算一个数的立方根。
   - Math.hypot()：返回所有参数的平方和的平方根。
   - Math.clz32()：JavaScript 的整数使用 32 位二进制形式表示，Math.clz32方法返回一个数的 32 位无符号整数形式有多少个前导 0。对于小数，Math.clz32方法只考虑整数部分。
   - Math.log10()
   - Math.log2()
   - Math.sinh(x)
   - Math.cosh(x)
   - Math.tanh(x)
   - Math.asinh(x)
   - Math.acosh(x)
   - Math.atanh(x)
- 【注】与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而Number.isFinite()对于非数值一律返回false, Number.isNaN()对于非NaN一律返回false。



```javascript
	// 误差检查
	function withinError(left, right){
		return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
	}
	
	// 实际使用这个函数时，需要注意，不要只验证运算结果，而要同时验证参与运算的每个值
	Number.isSafeInteger = function(n){
		return (typeof n === 'number' && 
		    Math.round(n) === n && 
		    Number.MIN_SAFE_INTEGER <= n && 
		    n <= Number.MAX_SAFE_INTEGER);
	};
	
	Math.cbrt = Math.cbrt || function(n){
		let r = Math.pow(Math.abs(n), 1 / 3);
		return n < 0 ? -r : r;
	};
```

<br />

<a name="bHv1O"></a>
# 数组的扩展


- 概述
   - 扩展运算符 ...：任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组。
- Array.from(arraylike, function, scope)：用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。所谓类似数组的对象，本质特征只有一点，即必须有length属性。
- Array.of()：返回参数值组成的数组。
- Array.prototype.copyWithin(index, start, end)：复制从start到end-1位置成员到index位置。
- Array.prototype.find(function, scope)：都不符合则返回 undefined。
- Array.prototype.findIndex(function, scope)：都不符合则返回 -1。
- Array.prototype.fill(value, start, end)：如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。
- Array.prototype.entries()
- Array.prototype.keys()
- Array.prototype.values()
- Array.prototype.includes(value, pos)
- Array.prototype.flat(number|Infinity)：如果想要“拉平”多层的嵌套数组，可以传入一个整数，表示想要拉平的层数，默认是1。
- Array.prototype.flatMap(function, scope)：只能展开一层数组。



```javascript
	// 扩展运算符
	console.log(...[1, 2, 3]);
	
	Array.from('hello');
	Array.from({length: 2}, () => 'hello world');
	
	function typesOf(){
		return Array.from(arguments, (o) => {return typeof o;});
	}
	
	[1, 2, 3, 4, 5].copyWithin(0, 3);
	[1, 2, -5, 5].find(x => x < 0);
	
	for(let [key, value] of ['a', 'b', 'c'].entries()){
		console.log(key, value);
	}
	
	[1, 2, 3].flatMap((x) => {
		return [x, 2 * x];
	});
```

<br />

<a name="KFU54"></a>
# 函数的扩展


- 函数参数的默认值
   - 使用参数默认值时，函数不能有同名参数。
   - 如果传入undefined，将触发该参数等于默认值，null则不会。
   - 如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数。而且剩余参数也不会计入length属性。
   - 一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域。
   - 只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式。变通方法一是设定全局性的严格模式，二是把函数包在一个无参数的立即执行函数里面。
- 箭头函数
   - 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象（实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this）。
   - 不可以当作构造函数，也就是说，不可以使用new命令。
   - 不可以使用arguments对象。如果要用，可以用 rest 参数代替。
   - 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
- name 属性
- 双冒号运算符 ::：函数上下文环境绑定。
- 尾调用：ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。因为在正常模式下，函数内部有fn.arguments、fn.caller两个变量，可以跟踪函数的调用栈。
- 尾递归：尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。就是把所有用到的内部变量改写成函数的参数。
- 函数参数的尾逗号：使得函数参数与数组、对象的尾逗号规则，保持一致。



```javascript
	// 特殊 name 属性
	Object.getOwnPropertyDescriptor(obj, prop).get.name // 'get fnName'
	(new Function()).name // 'anonymous'
	fn.bind().name // 'bound fnName'
	
	// 函数参数默认值结合解构赋值默认值
	function fetch(url, {body = '', method = 'GET', headers = {}} = {}){
		console.log(method);
	}
	
	// 箭头函数
	var sum = (sum1, sum2) => {
		return sum1 + sum2;
	};
	[1, 2, 3].map(x => x * x);
	
	// this指向外层的全局对象
	const block = {
		count: 1,
		compute: () => {
			this.count++;
			console.log(this.count);
		}
	};
	
	// 函数上下文环境绑定
	let log = ::console.log;
	let log = console.log.bind(console);
	
	// 函数柯里化
	function currying(fn, n){
		return function(m){
			return fn.call(this, m, n);
		};
	}
	
	// rest 运算符
	function add(...args){
		let sum = 0;
		for(let i = 0, len = args.length; i < len; i++){
			sum += args[i];
		}
		return sum;
	}
	
	// 尾调用
	function f(x){
		return g(x);
	}
	
	// 尾递归
	var fibonacci = function(n){
		if(n <= 1) return 1;
		return fibonacci(n - 1) + fibonacci(n - 2);
	};
	var fibonacci2 = function(n, ac1 = 1, ac2 = 1){
		if(n <= 1) return ac2;
		return fibonacci2(n - 1, ac2, ac1 + ac2);
	};
	
	// 蹦床函数：将递归执行转为循环执行
	function trampoline(f){
		while(f && f instanceof Function){
			f = f();
		}
		return f;
	}
	function sum(x, y){
		if(y <= 0) return x;
		return sum.bind(null, x + 1, y - 1);
	}
	trampoline(sum(1, 100000));
```

<br />

<a name="3F7kv"></a>
# 对象的扩展


- 概述
   - 解构赋值：扩展运算符 ... 的解构赋值，不能复制继承自原型对象的属性。
   - 如果扩展运算符的参数是null或undefined，这两个值会被忽略。
- Object.is()：用来比较两个值是否严格相等。
- Object.assign()：用于对象的合并，将源对象的所有可枚举属性，复制到目标对象。如果该参数不是对象，则会先转成对象。由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。属性名为 Symbol 值的属性，也会被拷贝。
- Object.keys()：过滤属性名为 Symbol 值的属性。
- Object.values()
- Object.entries()
- Object.fromEntries()：用于将一个键值对数组转为对象。



```javascript
	// 属性简写
	function get(x, y){
		return {x, y};
	}
	// 方法简写
	const o = {
		getCount(){
			return 1;
		}
	};
	
	// 属性名表达式
	const a = 'foo';
	const b = {
		[a]: true,
		['b' + 'c']: false
	};
	
	// 解构赋值
	var {x, y, ...z} = {x: 1, y: 2, c: 5, d: 6};
	
	// 合并对象
	let x = {...a, ...b};
	let x = Object.assign({}, a, b);
	
	// 对数组的处理：索引覆盖
	var o = Object.assign([1, 2, 3], [4, 5]); // [4, 5, 3]
	
	// Object.entries()
	let {keys, values, entries} = Object;
	let obj = {a: 1, b: 2, c: 3};
	for(let [key, value] of entries(obj)){
		console.log(key, value);
	}
	
	// Object.fromEntries()
	let arr = [['a', '100'], ['b', '200']];
	Object.fromEntries(arr);
```

<br />

<a name="bcHZe"></a>
# 正则的扩展


- 概述
   - Unicode 字符表示法：ES6 新增了使用大括号表示 Unicode 字符，这种表示法在正则表达式中必须加上u修饰符。
   - 后行断言：/(?<=y)x/，先匹配x再回到左边匹配y，"后行断言"的反斜杠引用，也与通常的顺序相反。
   - 后行否定断言：/(?
   - 具名组匹配：(?)，允许为每一个组匹配指定一个名字。字符串替换时，使用$引用具名组。如果要在正则表达式内部引用某个“具名组匹配”，可以使用\k的写法。
- 修饰符
   - y 修饰符：粘连(sticky)修饰符，与 g 修饰符类似，也是全局匹配，不同之处在于，g修饰符只要剩余位置中存在匹配就可，而y修饰符确保匹配必须从剩余的第一个位置开始。另y修饰符对match方法，只能返回第一个匹配，必须与g修饰符联用，才能返回所有匹配。
   - u 修饰符：Unicode 模式，用来正确处理大于 \uFFFF 的 Unicode 字符。
   - s 修饰符：dotAll 模式。
- Unicode 属性类
   - 正向匹配：\p{unicodePropName=unicodeProValue}。
   - 反向匹配：\P{unicodePropName=unicodeProValue}，匹配不满足条件的字符。
- reg.flags [reg.source]
- reg.unicode
- reg.sticky
- reg.dotAll



```javascript
	/\u{61}/u.test('a');
	/hello.world/s.test('hello\nworld');
	
	/(?<=(o)n\1)r/.exec('honor');
	/(?<=\1n(o))r/.exec('honor');
	
	let reg = /^\p{Decimal_Number}+$/u; // 匹配十进制字符
	let reg = /^\p{White_Space}+$/u; // 匹配空格
	let reg = /^\p{Block=Arrows}+$/u; // 匹配箭头字符
	
	// 具名组匹配
	let reg = /(?\d+)-(?\d+)-(?\d+)/;
	let match = reg.exec('2018-10-10');
	console.log(match.groups.year);
	'2015-09-09'.replace(reg, '$/$,$');
	
	const REG_Y = /\s*(\+|\d+)\s*/y,
	    REG_G = /\s*(\+|\d+)\s*/g;
	function tokenize(REG, str){
		let result = [],
		    match;
		while(match = REG.exec(str)){
			result.push(match[1]);
		}
		return result;
	}
```

<br />

<a name="fg523"></a>
# Symbol


- 概述
   - Symbol 值通过Symbol函数生成。
   - 凡是属性名属于 Symbol 类型，就都是独一无二的。
   - Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。
- Symbol 的属性
   - Symbol.hasInstance
   - Symbol.isConcatSpreadable：表示该对象用于Array.prototype.concat()时，是否可以展开。
   - Symbol.species：实例对象在运行过程中，需要再次调用自身的构造函数时，会调用该属性指定的构造函数。
   - Symbol.match：当执行str.match(myObject)时，如果该属性存在，会调用它，返回该方法的返回值。
   - Symbol.replace
   - Symbol.search
   - Symbol.split
   - Symbol.iterator：指向该对象的默认遍历器方法。
   - Symbol.toPrimitive：该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。
   - Symbol.toStringTag：在该对象上面调用Object.prototype.toString方法时，如果这个属性存在，可以用来定制[object Object]或[object Array]中object后面的那个字符串。
   - Symbol.unscopables：指向一个对象，该对象指定了使用with关键字时，哪些属性会被with环境排除。
- Symbol 的方法
   - Symbol.for()：接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。
   - Symbol.keyFor()：返回一个已登记的 Symbol 类型值的key。



```javascript
	let s1 = Symbol.for('foo');
	let s2 = Symbol.for('foo');
	s1 === s2;
	
	// Symbol.for()登记的名字的全局性
	iframe.contentWindow.Symbol.for('foo') === Symbol.for('foo');
	
	foo instanceof Foo;
	Foo[Symbol.hasInstance](foo);
	
	// 默认的Symbol.species属性
	class MyArray extend Array{
		static get [Symbol.species](){
			return this;
		}
	}
```

<br />

<a name="cAUOx"></a>
# Set 和 Map 数据结构


- 概述
   - Set 结构成员值唯一。
   - Set 结构的键名就是键值。
   - Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。
   - WeakSet 结构的成员只能是对象，而不能是其他类型的值。
   - WeakSet 结构中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。因此 ES6 规定 WeakSet 不可遍历。
   - Map 结构，类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
   - WeakMap 结构只接受对象作为键名（null除外），不接受其他类型的值作为键名。
   - WeakMap 结构的键名所指向的对象，不计入垃圾回收机制。
- Set 实例的属性
   - Set.prototype.size
- Set 实例的方法
   - Set.prototype.has()
   - Set.prototype.add()
   - Set.prototype.delete()
   - Set.prototype.clear()
   - Set.prototype.keys()：和values方法行为一致。
   - Set.prototype.values()：默认遍历器生成函数。
   - Set.prototype.entries()
   - Set.prototype.forEach(function, scope)
   - ------
   - WeakSet.prototype.has()
   - WeakSet.prototype.add()
   - WeakSet.prototype.delete()
- Map 实例的属性
   - Map.prototype.size
- Map 实例的方法
   - Map.prototype.has()
   - Map.prototype.set()
   - Map.prototype.get()
   - Map.prototype.delete()
   - Map.prototype.clear()
   - Map.prototype.keys()
   - Map.prototype.values()
   - Map.prototype.entries()：默认遍历器生成函数。
   - Map.prototype.forEach(function, scope)
   - ------
   - WeakMap.prototype.has()
   - WeakMap.prototype.get()
   - WeakMap.prototype.set()
   - WeakMap.prototype.delete()



```javascript
	// 数组去重
	[...(new Set(arr))];
	
	// 变通应用数组方法
	let set = new Set([1, 2, 3, 4, 5]);
	set = new Set([...set].filter(x => x % 2 === 0));
	
	// 实现并集、交集、差集
	let a = new Set([1, 2, 5]);
	let b = new Set([2, 3, 5]);
	let union = new Set([...a, ...b]);
	let intersect = new Set([...a].filter(x => b.has(x)));
	let difference = new Set([...a].filter(x => !b.has(x)));
	
	let map = new Map([['a', 1], ['b', 2]]);
	map.set('c', 3).set('d', 4);
	for(let [key, value] of map){
		console.log(key, value);
	}
```

<br />

<a name="BW7NT"></a>
# Proxy


- var proxy = new Proxy(function|object, object)
- Proxy 的方法
   - Proxy.revocable(function|object, object)：返回一个对象，该对象的proxy属性是Proxy实例，revoke属性是一个函数，可以取消Proxy实例。
- Proxy 实例的拦截方法
   - get(object, property, proxy)
   - set(object, property, value, proxy)：严格模式下，set代理如果没有返回true，就会报错。
   - has(object, property)：典型的操作就是in运算符。如果某个属性不可配置（或者目标对象不可扩展），则has方法就不得“隐藏”（即返回false）目标对象的该属性。
   - apply(object, scope, args)
   - construct(object, args)：用于拦截new命令。返回的必须是一个对象，否则会报错。
   - deleteProperty(object, property)：用于拦截delete操作。
   - defineProperty(object, property, descriptor)
   - getOwnPropertyDescriptor(object, property)
   - getPrototypeOf(object)：用于拦截 Object.prototype.__proto__、Object.prototype.isPrototypeOf()、Object.getPrototypeOf()、Reflect.getPrototypeOf()、instanceof，返回值必须是对象或者null，否则报错。
   - setPrototypeOf(object, prototype)：只能返回布尔值，否则会被自动转为布尔值。
   - preventExtensions(object)：只有目标对象不可扩展时，才能返回true，否则会报错。
   - isExtensible(object)：返回值必须与目标对象的isExtensible属性保持一致，否则就会抛出错误。
   - ownKeys(object)：用于拦截 Object.getOwnPropertyNames()、Object.getOwnPropertySymbols()、Object.keys()、for...in循环。返回的数组成员，只能是字符串或 Symbol 值。如果目标对象自身包含不可配置的属性，则该属性必须被ownKeys方法返回，否则报错。



```javascript
	var proxy = new Proxy(function(x, y){
		return x + y;
	}, {
		get: function(target, property, proxy){
			if(property === 'prototype'){
				return Object.prototype;
			}
			return 'Hello, ' + name;
		},
		apply: function(target, scope, args){
			return args[0];
		},
		construct: function(target, args){
			return {
				value: args[1]
			}
		}
	});
	proxy(1, 2); // 1
	new proxy(1, 2); // {value: 2}
	proxy.prototype === Object.prototype; // true
	
	var proxy = new Proxy({
		"_foo": "baz",
		"foo": "bar"
	}, {
		getOwnPropertyDescriptor: function(target, property){
			if(property[0] === '_'){
				return;
			}
			return Object.getOwnPropertyDescriptor(target, property);
		}
	});
	Object.getOwnPropertyDescriptor(proxy, '_foo'); // undefined
```

<br />

<a name="a0mRW"></a>
# Reflect


- 概述
   - Reflect对象的方法与Proxy对象的方法一一对应，这让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。
- Reflect 的方法
   - Reflect.get(object, property, receiver)
   - Reflect.set(object, property, value, receiver)：如果 Proxy对象和 Reflect对象联合使用，前者拦截赋值操作，后者完成赋值的默认行为，而且传入了receiver，那么Reflect.set会触发Proxy.defineProperty拦截。
   - Reflect.has(object, property)
   - Reflect.apply(object, scope, args)：等同于Function.prototype.apply.call(object, scope, args)。
   - Reflect.construct(object, args)：等同于new object(...args)，提供了一种不使用new，来调用构造函数的方法。
   - Reflect.deleteProperty(object, property)
   - Reflect.defineProperty(object, property, descriptor)
   - Reflect.getOwnPropertyDescriptor(object, property)
   - Reflect.getPrototypeOf(object)：Reflect.getPrototypeOf和Object.getPrototypeOf的一个区别是，如果参数不是对象，Object.getPrototypeOf会将这个参数转为对象然后运行，而Reflect.getPrototypeOf会报错。
   - Reflect.setPrototypeOf(object, prototype)
   - Reflect.preventExtensions(object)
   - Reflect.isExtensible(object)
   - Reflect.ownKeys(object)


<br />

<a name="jb508"></a>
# Promise 对象


- 概述
   - 一旦状态改变，就不会再变，任何时候都可以得到这个结果。事件的特点是，如果你错过了它，再去监听，是得不到结果的。
   - 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
   - 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部（不会退出进程、终止脚本执行）。
- var promise = new Promise(function)
- Promise 的方法
   - Promise.all([promise1, ...])：参数可以不是数组，但必须具有 Iterator 接口。只有所有promise实例的状态都变成fulfilled，或者其中有一个变为rejected，才会调用Promise.all方法后面的回调函数。如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法（实际指向的是新实例，该实例执行完catch方法后，也会变成resolved）。
   - Promise.race()
   - Promise.resolve()
   - Promise.reject()：注意，Promise.reject()方法的参数，会原封不动地变成后续方法的参数。
   - Promise.try()：这样就可以不管f是同步函数还是异步操作，都用then方法指定下一步流程，用catch方法处理f抛出的错误。
- Promise 实例的方法
   - Promise.prototype.then(function, function)：返回的是一个新的Promise实例。
   - Promise.prototype.catch(function)
   - Promise.prototype.finally(function)



```javascript
	let getJSON = function(url){
		let promise = new Promise(function(resolve, reject){
			// 异步操作
			if(/*  异步操作成功  */){
				resolve(value);
			}else{
				reject(error);
			}
		});
		return promise;
	};
	getJSON(url).then(function(value){
		
	}).catch(function(error){
		
	});
	
	var p = Promise.race([
		fetch(url),
		new Promise(function(resolve, reject){
			setTimeout(() => reject(new Error('request timeout.')), 5000);
		});
	]);
	p.then(function(res){
		
	}).catch(function(error){
		
	});
```

<br />

<a name="Fi1Xc"></a>
# Iterator 和 for...of 循环


- for...of 循环：一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口，就可以用for...of循环遍历它的成员。会正确识别 32 位 UTF-16 字符。



```javascript
	let arr = [5, 6, 7];
	let it = arr[Symbol.iterator]();
	it.next(); // {value: 5, done: false}
	
	// 关于对象
	function* entries(obj){
		for(let key of Object.keys(obj)){
			yield [key, obj[key]];
		}
	}
	let obj = {a: 1, b: 2};
	for(let [key, value] of entries(obj)){
		console.log(key, value);
	}
```

<br />

<a name="LacQe"></a>
# Generator 函数


- 概述
   - function关键字与函数名之间有一个星号。
   - 函数体内部使用yield表达式，定义不同的内部状态。
   - yield表达式只能用在 Generator 函数里面。
   - yield表达式如果用在另一个表达式之中，必须放在圆括号里面。用作函数参数或放在赋值表达式的右边，可以不加括号。
   - yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。V8 引擎直接忽略第一次使用next方法时的参数。
   - next()是将yield表达式替换成一个值。
   - throw()是将yield表达式替换成一个throw语句。
   - return()是将yield表达式替换成一个return语句。
- Generator 实例的方法
   - Generator.prototype.next(param)
   - Generator.prototype.throw(error)：可以在函数体外抛出错误，然后在 Generator 函数体内捕获。如果函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。throw方法抛出的错误要被内部捕获，前提是必须至少执行过一次next方法。throw方法被捕获以后，会附带执行下一条yield表达式，即会附带执行一次next方法。
   - Generator.prototype.return(param)：如果 Generator 函数内部有try...finally代码块，且正在执行try代码块，那么return方法会推迟到finally代码块执行完再执行。
- yield* 表达式：用来在一个 Generator 函数里面执行另一个 Generator 函数（如果yield表达式后面跟的是一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象）。
- 异步应用
   - 参数求值策略：JavaScript 语言的 Thunk 函数



```javascript
	function* helloworld(){
		yield 'hello';
		yield 'world';
		return 'end';
	}
	var g = helloworld();
	g.next(); // {value: 'hello', done: false}
	g.next(); // {value: 'world', done: false}
	g.next(); // {value: 'end', done: true}
	g.next(); // {value: undefined, done: true}
	
	// 斐波那契数列
	function* fibonacci(){
		let [prev, cur] = [0, 1];
		for(;;){
			yield cur;
			[prev, cur] = [cur, prev + cur];
		}
	}
	for(let n of fibonacci()){
		if(n > 1000){
			break;
		}
		console.log(n);
	}
	
	// yield* 表达式
	function* gen (){
		yield* ['a', 'b', 'c'];
	}
	var g = gen();
	g.next();
	
	// 作为对象属性的 Generator 函数
	var obj = {
		* gen(){
			
		}
	};
	
	// 部署对象的iterator接口
	Object.prototype[Symbol.iterator] = function*(){
		for(let key in this){
			yield [key, this[key]];
		}
	};
	let obj = {a: 1, b: 2};
	for(let [key, value] of obj){
		console.log(key, value);
	}
	
	// Thunk 函数
	var thunk = function(fn){
		return function(){
			var args = Array.prototype.slice(arguments);
			return function(callback){
				args.push(callback);
				return fn.apply(this, args);
			};
		};
	};
	let thunk = function(fn){
		return function(...args){
			return function(callback){
				return fn.call(this, ...args, callback);
			};
		};
	};
	thunk(fs.readFile)(file)(callback);
```

<br />

<a name="Riad0"></a>
# async 函数


- 概述
   - 返回值是 Promise 对象。
   - 希望即使前一个异步操作失败，也不要中断后面的异步操作。可以将可能失败的await放在try...catch结构里面。
   - 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。
- 异步遍历器
   - 调用遍历器的next方法，返回的是一个 Promise 对象。
   - 一个对象的同步遍历器的接口，部署在Symbol.iterator属性上面。同样地，对象的异步遍历器接口，部署在Symbol.asyncIterator属性上面。
   - for await...of循环
      - for...of循环用于遍历同步的 Iterator 接口。新引入的for await...of循环，用于遍历异步的 Iterator 接口。
      - 如果next方法返回的 Promise 对象被reject，for await...of就会报错，要用try...catch捕捉。
      - for await...of循环也可以用于同步遍历器。
   - 异步 Generator 函数
      - 就像 Generator 函数返回一个同步遍历器对象一样，异步 Generator 函数返回一个异步遍历器对象。
      - 普通的 async 函数返回的是一个 Promise 对象，而异步 Generator 函数返回的是一个异步 Iterator 对象。async 函数和异步 Generator 函数，是封装异步操作的两种方法，都用来达到同一种目的。区别在于，前者自带执行器，后者通过for await...of执行，或者自己编写执行器。



```javascript
	async function timeout(ms){
		await new Promise((resolve, reject) => {
			setTimeout(resolve, ms);
		});
		return 'Hello World';
	}
	timeout(1000).then((value) => {
		
	});
	
	async function loadOrder(urls){
		// 并发读取URL
		let textPromises = urls.map(async url => {
			let response = await fetch(url);
			return response.text();
		});
		// 次序输出
		for(let textPromise of textPromises){
			console.log(await textPromise);
		}
	}
	
	// 读取文件的传统写法与异步遍历器写法(Node)
	function main(path){
		let readStream = fs.createReadStream(path, {encoding: 'utf8', highWaterMark: 1024});
		readStream.on('data', function(chunk){
			console.log(chunk);
		});
		readStream.on('end', function(){
			console.log('end');
		});
	}
	async function main(path){
		let readStream = fs.createReadStream(path, {enocding: 'utf8', highWaterMark: 1024});
		for await(let chunk of readStream){
			console.log(chunk);
		}
		console.log('end');
	}
	
	// 同步Generator函数和异步Generator函数
	function* map(iterable, func){
		let iter = iterable[Symbol.iterator]();
		while(true){
			let {value, done} = iter.next();
			if(done) break;
			yield func(value);
		}
	}
	async function* map(iterable, func){
		let iter = iterable[Symbol.asyncIterator]();
		while(true){
			let {value, done} = await iter.next();
			if(done) break;
			yield func(value);
		}
	}
```

<br />

<a name="PHeyt"></a>
# Class


- 概述
   - 类必须使用new调用，否则会报错。
   - 类和模块的内部，默认就是严格模式，不需要使用use strict指定运行模式。
   - ES6 不会把类的声明提升到代码头部。
   - name属性总是返回紧跟在class关键字后面的类名。
   - 静态方法：在一个方法前加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用。静态方法可以与非静态方法重名。
   - Class 可以通过extends关键字实现继承。
   - 在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。
   - super 关键字
      - 第一种情况，super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数，用在其他地方就会报错。
      - 第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
   - 由于this指向子类实例，所以如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性。
   - ES6 允许继承原生构造函数定义子类，因为 ES6 是先新建父类的实例对象this，然后再用子类的构造函数修饰this。
- new.target



```javascript
	class Person{
		constructor(name, age){
			this.name = name;
			this.age = age;
		}
		get prop(){
			return 'getter';
		}
		set prop(value){
			console.log('setter: ' + value);
		}
		say(){
			console.log(this.name, this.age);
		}
	}
	let person = new Person('kerwen', 18);
	person.say();
	
	// super关键字
	class Bar{
		constructor(x, y){
			this.x = x;
			this.y = y;
		}
		static hello(){
			return 'hello';
		}
	}
	class Foo extends Bar{
		constructor(x, y, z){
			super(x, y);
			this.z = z;
		}
		static hello(){
			return super.hello() + ', world';
		}
	}
	let obj = {
		toString(){
			return 'obj: ' + super.toString();
		}
	};
	Foo.hello(); // 静态方法
	Object.getPrototypeOf(Foo) === Bar;
	
	// 继承Array
	class MyArray extends Array{
		constructor(...args){
			super(...args);
		}
	}
```

<br />

<a name="cBSRr"></a>
# Decorator 修饰器


- 概述
   - 修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。
   - 修饰器只能用于类和类的方法，不能用于函数，因为存在函数提升。



```javascript
	function testable(isTest){
		return function(target, prop, descriptor){
			target.prototype.isTestable = isTest;
		};
	}
	@testable(true)
	class TestClass{
		
	}
	var cl = new TestClass();
	cl.isTestable;
```

<br />

<a name="z0BOa"></a>
# Module


- 概述
   - ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。不能使用表达式和变量这些只有在运行时才能得到结果的语法结构。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。
   - ES6 的模块自动采用严格模式。
   - ES6 模块之中，顶层的this指向undefined。
   - 通常情况下，export输出的变量就是本来的名字，但是可以使用as关键字重命名；可以使用as关键字，将import命令输入的变量重命名。
   - export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。CommonJS 模块输出的是值的缓存，不存在动态更新。
   - import命令具有提升效果，会提升到整个模块的头部，首先执行。
   - 模块的整体加载。
   - 因为export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。因为export default命令的本质是将后面的值，赋给default变量，所以可以直接将一个值写在export default之后。
   - export * from './module.js'命令会忽略模块的default方法。
- export 命令
- export default 命令
- import 命令
- import()：返回一个 Promise 对象。
- Module 的加载
   - ----浏览器中
   - 使用 script 标签，type="module"，默认defer（异步加载）。
   - ----Node中
   - CommonJS模块的循环加载
   - ES6模块的循环加载



```javascript
	// module.js
	let firstName = 'kerwin';
	let year = 2018;
	export {
		firstName,
		year as cYear
	};
	
	// index.js
	import {firstName, cYear} from './module.js';
	import * as module from './module.js'; // 整体加载模块
	import _, {firstName, cYear} from './module.js'; // 同时引入默认方法和其他接口
```

<br />

<a name="5DICh"></a>
# ArrayBuffer


- 概述
   - 小端字节序将最不重要的字节排在前面。
   - 所有个人电脑几乎都是小端字节序。
   - 很多网络设备和特定的操作系统采用的是大端字节序。
- 二进制数组
   - ArrayBuffer 对象
   - TypedArray 视图：数组成员都是同一个数据类型。
   - DataView 视图：数组成员可以是不同的数据类型。
- ArrayBuffer 的属性和方法
   - ArrayBuffer.isView()
   - ArrayBuffer.prototype.byteLength
   - ArrayBuffer.prototype.slice()
- 关于 SharedArrayBuffer 的 Atomics 对象
   - ----基本方法
   - Atomics.load(view, index)：用来从共享内存读出数据。
   - Atomics.store(view, index, value)：用来向共享内存写入数据，返回写入的值。
   - Atomics.exchange(view, index, value)：用来向共享内存写入数据，返回被替换的值。
   - Atomics.wait(view, index, expectedValue, time|Infinity)
   - Atomics.wake(view, index, count|Infinity)
   - ----运算方法
   - Atomics.add(view, index, value)
   - Atomics.sub(view, index, value)
   - Atomics.and(view, index, value)
   - Atomics.or(view, index, value)
   - Atomics.xor(view, index, value)
   - ----其他方法
   - Atomics.compareExchange(view, index, oldValue, newValue)
   - Atomics.isLockFree(size)
- TypedArray 的属性和方法
   - 视图类型
      - Int8Array
      - Uint8Array
      - Uint8ClampedArray：溢出规则不同。
      - Int16Array
      - Uint16Array
      - Int32Array
      - Uint32Array
      - Float32Array
      - Float64Array
   - TypedArray.of()
   - TypedArray.from()
   - TypedArray.prototype.BYTES_PER_ELEMENT
   - TypedArray.prototype.buffer
   - TypedArray.prototype.byteLength：字节长度。
   - TypedArray.prototype.byteOffset
   - TypedArray.prototype.length：成员长度。
   - TypedArray.prototype.set(array|typedarray, index)：将一段内容完全复制到另一段内存。
   - TypedArray.prototype.subarray()：对于 TypedArray 数组的一部分，再建立一个新的视图。
   - TypedArray.prototype.slice()
- DataView 的属性和方法
   - DataView.prototype.buffer
   - DataView.prototype.byteLength
   - DataView.prototype.byteOffset



```javascript
	Float64Array.BYTES_PER_ELEMENT // 8
	
	// index.js
	console.log(sa[37]); // 163
	Atomics.store(sa, 37, 12345);
	Atomics.wake(sa, 37, 1);
	// worker.js
	Atomics.wait(sa, 37, 163);
	console.log(sa[37]); // 12345
	
	
	// 判断视图是小端字节序或大端字节序
	function getPlatformEndian(){
		let arr32 = Uint32Array.of(0x12345678),
		    arr8 = new Uint8Array(arr32.buffer);
		switch((arr8[0] * 0x1000000) + (arr8[1] * 0x10000) + (arr8[2] * 0x100) + arr8[3]){
			case 0x12345678:
			    return 'BIG_ENDIAN';
			case 0x78563412:
			    return 'LITTLE_ENDIAN';
			default:
			    throw new Error('Unkonwn endian');
		}
	}
	
	// ArrayBuffer与字符串的转换( 假定字符串为UTF-16编码，JavaScript 的内部编码方式 )
	function ab2str(buf){
		if(buf && buf.byteLength < 1024)
		    return String.fromCharCode.apply(null, new Uint16Array(buf));
		// 如果是大型二进制数组，为了避免溢出，必须一个一个字符地转
		let bufView = new Uint16Array(buf),
		    len = bufView.length,
		    tArray = new Array(len);
		for(let i = 0; i < len; i++){
			tArray[i] = String.fromCharCode.call(null, bufView[i]);
		}
		return tArray.join('');
	}
	function str2ab(str){
		let buf = new ArrayBuffer(str.length * 2), // 每个字符占用2个字节
		    bufView = new Uint16Array(buf);
		for(let i = 0, len = str.length; i < len; i++){
			bufView[i] = str.charCodeAt(i);
		}
		return buf;
	}
```

<br />

<a name="9R4MZ"></a>
# 创建于 2018 年 12 月 29 日


- 当前城市：北京
- 当日天气：晴
- 当日气温：-3/-11℃
- 空气质量：22 优
