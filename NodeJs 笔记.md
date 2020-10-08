# NodeJs 笔记



<a name="jmaBF"></a>
# 准备前


- 编程方式
   - 面向过程编程
   - 面向对象编程
   - 面向切面编程
- 注释规范
   - jsDoc：根据javascript文件中注释信息，生成JavaScript应用程序或库、模块的API文档。



```javascript
	function compare(x, y){
		return x - y;
	}
	function swap(arr, i, j){
		var t = arr[i];
		arr[i] = arr[j];
		arr[j] = t;
	}
	function bubbleSort(arr){
		for(var i = 0, len = arr.length; i < len; i++){
			for(var j = 0, len1 = arr.length - i - 1; j < len1; j++){
				if(compare(arr[j], arr[j + 1]) > 0){
					swap(arr, j, j + 1);
				}
			}
		}
		return arr;
	}
```

<br />

<a name="NFr5M"></a>
# 数据库


- Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。
- MongoDB是一个由C++语言编写基于分布式文件存储的数据库。
- MySQL是一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle旗下产品。


<br />

<a name="2CiqW"></a>
# 模块机制


- CommonJS 模块规范( 后端 )
   - 模块引用
   - 模块定义
   - 模块标识
- Node 模块实现
   - 路径分析
   - 文件定位
   - 编译执行
   - 【注1】目录的加载规则：package.json的main字段 - index.js 或 index.node 或 index.json
   - 【注2】由于模块一旦被加载以后，就会被系统缓存。如果希望模块执行多次，则可以让模块返回一个函数，然后多次调用该函数。
   - 【注3】delete require.cache[moduleName]
   - 【注4】exports 与 module.exports
      - module.exports 初始值为一个空对象 {}。
      - exports 是指向 module.exports 的引用。
      - exports 变量在模块的文件级别作用域内有效。
      - require() 返回的是 module.exports 而不是 exports。
- package.json 文件
   - scripts 字段：指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm run start时，所要执行的命令。
   - main 字段：指定了加载的入口文件。
   - 版本限定
      - 指定版本
      - ~ + 指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2）。
      - ^ + 指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2）。
      - latest
- Node 命令
   - REPL(read-eval-print loop) 环境
   - node -e 'console.log("1")'
   - node --use_strict
- NPM 命令
   - npm init
   - npm view [module] version：查看线上最新版本
   - npm adduser
   - npm login
   - npm publish [folder]
   - npm owner ls [package name]
   - npm owner add [user] [package name]
   - npm owner rm [user] [package name]
   - npm ls
   - npm -l：查看各个命令的简单用法
   - npm config list -l：查看 npm 的配置
   - npm info [package]
   - npm list：以树型结构列出当前项目安装的所有模块，以及它们依赖的模块
   - npm shrinkwrap：快照当前项目的模块版本。运行该命令后，会在当前项目的根目录下生成一个npm-shrinkwrap.json文件。下次运行npm install命令时，就会只安装里面提到的模块，且版本也会保持一致。
   - ------
   - npm install
   - npm install --production：只安装dependencies字段的模块
   - npm install [package]@[version|latest|'>=0.1.0 <0.2.0']
   - npm install [package] -D(--save-dev)
   - npm install [package] -S(--save)
   - npm install -g [package]
   - npm uninstall -g [package]
   - npm update -g [package]
   - npm update --depth 9999 [package]：不只更新顶层模块，递归更新依赖的依赖
   - npm run(run-script)：列出package.json里面所有可以执行的脚本命令
   - npm run [command]
   - npm test = npm run test, npm start = npm run start(未配置默认执行node server.js)
   - 【注1】npm run为每条命令提供了pre-和post-两个钩子。以npm run lint为例，执行这条命令之前，npm会先查看有没有定义prelint和postlint两个钩子。
   - 【注2】pre和post脚本既可以为npm自动运行，也可以手动执行它们。
   - 【注3】scripts字段还可以使用一些内部变量，主要是package.json的各种字段，例如$npm_package_version。
- 从本地安装依赖包
   - npm install [tarball file]
   - npm install [tarball url]
   - npm install [folder]
- 从镜像源安装依赖包
   - npm install -g [package] --registry=https://registry.npm.taobao.org
   - npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org)
   - alias lnpm='npm --registry=https://registry.npm.taobao.org'
- 其他模块规范
   - AMD：Asynchronous Module Definition
   - CMD：Common Module Definition
- 兼容多种模块规范



```javascript
	// Linux系统的管道命令：一个操作的输出，是另一个操作的输入
	browserify browser/main.js | uglifyjs -mc > static/bundle.js
	// npm run命令
	npm run build-js && npm run build-css // 串行运行
	npm run build-js & npm run build-css // 并行运行
	
	// CommonJS( JS模块 )
	(function(exports, require, module, __filename, __dirname){\n CODE \n});
	(function(exports, require, module, __filename, __dirname){
		var math = require("math");
		module.exports.area = function(radius){
			return Math.PI * radius * radius;
		};
	});
	
	define(id[string], dependencies[array], function(require, exports, module){});
	// AMD
	// -- 定义非独立模块
	define(['./a', './b'], function(a, b){
		var exports = {};
		exports.sayHello = function(){
			a.doSomething();
			b.doSomething();
		}
		return exports;
	});
	// -- 定义独立模块
	define({
		method1: function(){},
		method2: function(){}
	});
	// 兼容CommonJS规范
	define(function(require, exports, module){
		var a = require('./a'),
			b = require('./b');
		return {
			method1: function(){},
			method2: function(){}
		};
	});
	require.config({
		paths: json,
		baseUrl: string,// data-main特性
		shim: json// 帮助requirejs加载非AMD规范的库
	});
	require(['./test'], function(test){
		
	}, function(err){
		
	});
	requirejs.onError = function(err){
		
	};
	// CMD
	define(function(require, exports, module){
		var exports = {};
		exports.sayHello = function(){
			var a = require('./a');
			a.doSomething();
			var b = require('./b');
			b.doSomething();
		}
		return exports;
	});
	seajs.config({
		
	});
	seajs.use(['./test'], function(test){
		
	});
	
	// 兼容多种模块规范
	;(function(name, definition){
			// 检测上下文环境中是否为 AMD 或 CMD
		var hasDefine = typeof define === 'function',
			// 检测上下文环境是否为 Node
			hasExports = typeof module !== undefined && module.export;
		if(hasDefine){
			// AMD 环境或 CMD 环境
			define(definition);
		}else if(hasExports){
			// 定义为普通 Node 模块
			module.exports = definition();
		}else{
			// 将模块的执行结果挂在 window 环境中，在浏览器中 this 指向 window
			this[name] = definition();
		}
	}('hello', function(){
		var hello = function(){};
		return hello;
	}));
```

<br />

<a name="b4Z2m"></a>
# fs 模块


- fs.readFileSync(path, options)
- ----异步调用
- fs.readFile(path, options, callback)：文件编码默认为null，读取模式默认为r（只读）
- fs.writeFile(path, input, options, callback)：写入字符串的编码默认是utf8
- fs.exists(path, callback)
- fs.rmdir(path, callback)
- fs.mkdir(path, 0777, callback)
- fs.readdir(path, callback)
- fs.stat(path, callback)：判断正在处理的到底是一个文件，还是一个目录
- fs.watchFile(path, callback)
- fs.unwatchFile(path, callback)
- fs.createReadStream(path, options)：用于打开大型的文本文件，创建一个读取操作的数据流
- fs.createWriteStream(path, options)



```javascript
	// 行结尾字符
	var os = require('os');
	os.EOL
	var EOL = fileContents.indexOf('\r\n') >= 0 ? '\r\n' : '\n';
	var EOL = process.platform === 'win32' ? '\r\n' : '\n';
```

<br />

<a name="i1z6l"></a>
# path 模块


- path.join(path, string)：用于连接路径
- path.resolve([from, ...], to])：用于将相对路径转为绝对路径，类似cd与pwd结合使用
- path.relative(path, path)：返回第二个路径相对于第一个路径的那个相对路径
- path.parse(path)：返回路径各部分的信息



```javascript
path.join(dir, "foo"); // unix: dir/foo, window: dir\foo
```

<br />

<a name="MNs2v"></a>
# process 对象


- ----属性
- process.pid
- process.platform
- process.version
- process.stdout.write(string)
- process.stdin.pipe(process.stdout) [fs.createReadStream(path, options).pipe(fs.createWriteStream(path, options))]
- process.stderr
- ----方法
- process.argv() ['node', pathName, param1, ...]
- process.cwd()：返回进程的当前目录
- process.chdir()：用来切换目录
- process.getgid()：group id
- process.getuid()：user id
- process.setgid()
- process.setuid()
- process.nextTick()
- process.on()
   - data 事件
   - exit 事件
   - uncaughtException 事件
   - SIGINT 事件：接收到系统信号SIGINT时触发，主要是用户按Ctrl + c时触发
   - SIGTERM 事件：系统发出进程终止信号SIGTERM时触发
- process.exit()：可以接受一个数值参数，如果参数大于0，表示执行失败；如果等于0表示执行成功
- process.kill(pid, signal)


<br />

<a name="kKAdK"></a>
# events 模块


- ----EventEmitter 的实例方法
- emitter.on(eventName, function)
- emitter.addListener(eventName, function)
- emitter.once(eventName, function)
- emitter.listeners(eventName)：返回该事件所有回调函数组成的数组。
- emitter.removeListener(eventName, function)
- emitter.removeAllListeners(eventName)
- emitter.emit(eventName, param1, param2, ...)：事件处理过程中抛出的错误，可以使用try...catch捕获。
- emitter.setMaxListeners(number)：Node默认允许同一个事件最多可以指定10个回调函数。
- newListener 事件：添加新的回调函数时触发(包括移除事件监听)。
- removeListener 事件



```javascript
	// EventEmitter接口的error事件
	var util = require('util');
	var EventEmitter = require('events').EventEmitter;
	var emit = new EventEmitter();
	emit.emit('error', new Error('something happened!'));
	emit.on('error', function(err){
		
	});
	function Person(name){
		
	}
	Person.prototype.__proto__ = EventEmitter.ptototype;
	// 或者
	util.inherits(Person, EventEmitter);
```

<br />

<a name="uX8Ur"></a>
# stream 接口


- var readable = fs.createReadStream(path, options)
- 可读数据流模式
   - pull 模式
   - push 模式
- ----可读数据流
- readable.readable
- readable.setEncoding(string)
- readable.read()
- readable.pause()
- readable.isPaused()
- readable.resume()
- readable.pipe(writable)：必须在可读数据流上调用，它的参数必须是可写数据流。
- readable.unpipe(writable)：移除pipe方法指定的数据流目的地。如果没有参数，则移除所有的pipe方法目的地。
- data 事件
- readable 事件：同时使用readable与data事件时，显式调用pause方法，会使得readable事件释放一个data事件，否则data监听无效。
- end 事件
- error 事件
- ----可写数据流
- writable.writable
- writable.write(data, encoding, callback)：返回一个布尔值，表示本次数据是否处理完成。
- writable.end(data, encoding, callback)
- drain 事件
- finish 事件
- pipe 事件
- unpipe 事件
- error 事件



```javascript
	var data = '';
	readable.setEncoding('utf8');
	readable.on('data', function(chunk){
		data += chunk;
	});
	readable.on('readable', function(){
		var chunk;
		while(null !== (chunk = readable.read())){
			data += chunk;
		}
	});
	
	function writeMore(writable, data, encoding, callback){
		var i = 10000;
		write();
		function write(){
			var ok = true;
			do{
				i -= 1;
				if(i === 0){
					writable.write(data, encoding, callback);
				}else{
					ok = writable.write(data, encoding);
				}
			}while(i > 0 && ok);
			if(i > 0){
				writable.once('drain', write);
			}
		}
	}
```

<br />

<a name="gLumD"></a>
# 异步 I/O


- Node 的异步 I/O
   - 事件循环
   - 观察者
   - 请求对象
   - I/O 线程池
- 非 I/O 的异步 API
   - setTimeout(function, time)
   - setInterval(function, time)
   - setImmediate(function)
   - process.nextTick(function)
   - 【注1】间隔的毫秒数在1毫秒到2,147,483,647毫秒（约24.8天）之间。如果超过这个范围，会被自动改为1毫秒。
   - 【注2】process.nextTick指定的回调函数是在本次"事件循环"触发，setImmediate指定的是在下次"事件循环"触发。
   - 【注3】多个process.nextTick语句总是在当前"执行栈"一次执行完，多个setImmediate可能则需要多次loop才能执行完。
- 服务器模型
   - 同步式
   - 每进程/每请求
   - 每线程/每请求


<br />

<a name="uIzNP"></a>
# 异步编程


- 函数式编程
   - 高阶函数：可以把函数作为参数，或是将函数作为返回值的函数。
   - 偏函数：通过指定部分参数来产生一个新的定制函数的形式。
- 难点
   - 异常处理
   - 函数嵌套过深
   - 多线程编程
- 解决方案
   - 事件发布/订阅模式：回调函数的事件化( on/trigger )
   - Promise/Deferred模式：先执行异步调用，延迟传递处理
   - 流程控制库
   - 协程
   - 【注1】进程拥有自己独立的堆和栈，既不共享堆，亦不共享栈，进程由操作系统调度。
   - 【注2】线程拥有自己独立的栈和共享的堆，共享堆，不共享栈，线程亦由操作系统调度(标准线程是的)。
   - 【注3】协程和线程一样共享堆，不共享栈，协程由程序员在协程的代码里显式调度。
- 异步并发控制
   - bagpipe 解决方案
   - async 解决方案



```javascript
	// 【Promise/Deferred模式】
	// Promises/A规范的一个实现：Q模块
	var readFile = function(file, encoding){
		var deferred = Q.defer();
		fs.readFile(file, encoding, deferred.makeNodeResolver());
		return deferred.promise;
	};
	// 调用
	readFile('example.txt', 'utf-8').then(function(data){
		
	}, function(err){
		
	});
	
	// API Promise化
	var smooth = function(method){
		return function(){
			var deferred = new Deferred();
			var args = Array.prototype.slice.call(arguments, 0);
			args.push(deferred.callback());
			method.apply(null, args);
			return deferred.promise;
		};
	};
	
	// 【流程控制库】
	// 流程控制库方案的一个实现：async、Step、wind
	async.series();
	async.parallel();
	async.waterfall();
	async.auto();
```

<br />

<a name="Oa5BU"></a>
# 内存控制


- V8的垃圾回收机制与内存限制
   - process.memoryUsage()
   - os.totalmem()
   - os.freemem()
   - os.cpus()
   - node --max-old-space-size=1700 test.js (MB)
   - node --max-new-space-size=1024 test.js (KB)
   - V8的垃圾回收策略主要基于分代式垃圾回收机制
   - 新生代中的对象主要通过 Scavenge 算法进行垃圾回收
   - 老生代中主要采用 Mark-Sweep 和 Mark-Compact 相结合的方式进行垃圾回收
- 内存泄露
   - 缓存
   - 队列消费不及时
   - 作用域未释放
   - 【注】严格意义的缓存有着完善的过期策略，而普通对象的键值对没有。
- 大内存应用
   - fs.createReadStream()
   - fs.createWriteStream()
   - crs.setEncoding(encoding)
   - Buffer.concat(list, [length])
   - Buffer.isEncoding(encoding)
   - new Buffer(str, [encoding])
   - buf.write(string, [offset], [length], [encoding])
   - buf.toString([encoding], [start], [end])
   - 【注1】Node中的大多数模块都有stream的应用，比如fs模块的createReadStream()和createWriteStream()，process模块的stdin和stdout。
   - 【注2】支持编码类型：ASCII、UTF-8、UTF-16LE/UCS-2、Base64、Binary、Hex。



```javascript
	Buffer.concat = function(list, length){
		if(!Array.isArray(list)){
			throw new Error('Usage: Buffer.concat(list, [length])');
		}
		if(list.length === 0){
			return new Buffer(0);
		}else if(list.length === 1){
			return list[0];
		}
		if(typeof length !== "number"){
			length = 0;
			for(var i = 0; i < list.length; i++){
				var buf = list[i];
				length += buf.length;
			}
		}
		var buffer = new Buffer(length);
		var pos = 0;
		for(var i = 0; i < list.length; i++){
			var buf = list[i];
			buf.copy(buffer, pos);
			pos += buf.length;
		}
		return buffer;
	};
```

<br />

<a name="bbpq5"></a>
# 网络编程


- 概述
   - TCP（传输控制协议）：net模块
   - UDP（用户数据报协议）：dgram模块
   - HTTP：http模块
   - HTTPS：https模块
   - 禁用Nagle算法：socket.setNoDelay(true)
- 网络服务与安全
   - 术语
      - SSL：安全套接层（Secure Sockets Layer）
      - TLS：安全传输层协议（Transport Layer Security）
      - CA：数字证书认证中心（Certificate Authority）
      - CSR：证书签名请求（Certificate Signing Request）
   - Node在网络安全上提供了3个模块：crypto、tls、https。tls模块提供了与net模块类似的功能，区别在于它建立在TLS/SSL加密的TCP连接上。
   - TLS/SSL是一个公钥/私钥的非对称结构，每个服务器端和客户端都有自己的公私钥。为了解决中间人攻击，其引入了数字证书来进行认证。



```javascript
	// 创建TCP服务器端
	var net = require('net');
	var server = net.createServer(function(socket){
		socket.on('data', function(data){
			socket.write('你好');
		});
		socket.on('end', function(){
			console.log('连接断开');
		});
		socket.write('欢迎');
	});
	server.listen(8124, function(){
		console.log('server bound');
	});
	// Domain Socket
	//server.listen('/tmp/echo.sock');
	// 创建TCP客户端
	// 通过 Telnet 工具作为客户端
	telnet 127.0.0.1 8124
	// 通过 nc(NetCat) 工具作为客户端
	nc -U /tmp/echo.sock
	// 通过 net 模块
	var net = require('net');
	var client = net.connect({port: 8124}, function(){
		console.log('client connect');
		client.write('world');
	});
	//var client = net.connect({path: '/tmp.echo.sock'});
	client.on('data', function(data){
		console.log(data.toString());
		client.end();
	});
	client.on('end', function(){
		console.log('client disconnected');
	});
	
	
	// 创建UDP服务器端
	var dgram = require('dgram');
	var server = dgram.createSocket('udp4');
	server.on('message', function(msg, rinfo){
		console.log(msg, rinfo.address, rinfo.port);
	});
	server.on('listening', function(){
		var address = server.address();
		console.log(address.address, address.port);
	});
	server.bind(41234);
	// 创建UDP客户端
	var dgram = require('dgram');
	var message = new Buffer('深入浅出NodeJs');
	var client = dgram.createSocket('udp4');
	client.send(message, 0, message.length, 41234, 'localhost', function(err, bytes){
		client.close();
	});
	
	
	// 创建HTTP服务器端
	var http = require('http');
	var url = require('url');
	http.createServer(function(req, res){
		console.log(req.method, req.url, req.headers.cookie);
		// 业务逻辑需要判断查询字符串值是数组还是字符串
		req.query = url.parse(req.url, true).query;
		
		res.setHeader('Set-Cookie', serializeCookie('isVisit', '1'));
		//res.setHeader('Set-Cookie', [serializeCookie('foo', '1'), serializeCookie('baz', '2')]);
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end('hello');// 务必调用，结束请求
	}).listen(1337, '127.0.0.1');
	// 创建HTTP客户端
	var http = require('http');
	var options = {
		hostname: '127.0.0.1',
		port: 1334,
		path: '/',
		method: 'GET'
	};
	options.agent = new http.Agent({
		maxSockets: 10
	});
	var req = http.request(options, function(res){
		console.log(res.statusCode, JSON.stringify(res.headers));
		res.setEncoding('utf8');
		res.on('data', function(chunk){
			console.log(chunk);
		});
	});
	req.end();
	
	
	// 创建TLS服务器端
	/*
	 * openssl genrsa -out ca.key 1024
	 * openssl req -new -key ca.key -out ca.csr
	 * openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
	 */
	var tls = require('tls');
	var fs = require('fs');
	var options = {
		key: fs.readFileSync('./keys/server.key'),
		cert: fs.readFileSync('./keys/server.crt'),
		requestCert: true,
		ca: [fs.readFileSync('./keys/ca.crt')]
	};
	var server = tls.createServer(options, function(stream){
		console.log(stream.authorized ? 'authorized' : 'unauthorized');
		stream.write('welcome!\n');
		stream.setEncoding('utf8');
		stream.pipe(stream);
	});
	server.listen(8000, function(){
		console.log('server bound');
	});
	// 创建TLS客户端
	var tls = require('tls');
	var fs = require('fs');
	var options = {
		key: fs.readFileSync('./keys/client.key'),
		cert: fs.readFileSync('./keys/client.crt'),
		ca: [fs.readFileSync('./keys/ca.crt')]
	};
	var stream = tls.connect(8000, options, function(){
		console.log(stream.authorized ? 'authorized' : 'unauthorized');
		process.stdin.pipe(stream);
	});
	stream.setEncoding('utf8');
	stream.on('data', function(data){
		
	});
	stream.on('end', function(){
		server.close();
	});
	
	
	// 创建HTTPS服务器端
	var https = require('https');
	var fs = require('fs');
	var options = {
		key: fs.readFileSync('./keys/server.key'),
		cert: fs.readFileSync('./keys/server.crt')
	};
	https.cretaeServer(options, function(req, res){
		res.writeHead(200);
		res.end('hello world!\n');
	}).listen(8000);
	// 创建HTTPS客户端
	var https = require('https');
	var fs = require('fs');
	var options = {
		hostname: '127.0.0.1',
		port: 8000,
		path: '/',
		method: 'GET',
		key: fs.readFileSync('./keys/client.key'),
		cert: fs.readFileSync('./keys/client.crt'),
		ca: [fs.readFileSync('./keys/ca.crt')]// 自签名证书
	};
	options.agent = new https.Agent(options);
	var req = https.request(options, function(res){
		res.setEncoding('utf8');
		res.on('data', function(chunk){
			console.log(chunk);
		});
	});
	req.end();
	req.on('error', function(e){
		
	});
```

<br />

<a name="BrhHV"></a>
# 构建Web应用


- 请求方法
   - GET
   - POST
   - DELETE
   - PUT
   - CONNECT
   - HEAD
- Cookie
   - 名称( name )
   - 值( value )
   - 失效时间( Expires )
   - 失效时间( Max-Age )
   - 路径( Path )
   - 域( domain )
   - 安全标志( secure )
   - 脚本能否更改( HttpOnly )
- 性能影响解决方案
   - 减小Cookie大小
   - 为静态组件使用不同域名（对于来自同一个域名的资源，浏览器一般最多同时下载六个，通常把静态文件放在不同的域名之下，以加快下载速度。）
   - 减少DNS查询
- Session
   - 基于Cookie来实现用户和数据的映射
   - 通过查询字符串来实现服务器端和客户端数据的映射
- 路由解析
   - RESTful：表现层状态转化（Representational State Transfer），其设计哲学主要将服务器端提供的内容实体看做一个资源，并表现在URL上。
- 页面渲染
   - bigpipe：Facebook公司前端加载技术，先向用户输出没有数据的布局，再持续数据输出渲染框架。建议在网站重要的且数据请求时间较长的页面中使用。



```javascript
	// 解析与序列化cookie
	var parseCookie = function(cookie){
		var cookies = {},
			list,
			pair;
		if(!cookie){
			return cookies;
		}
		list = cookie.split(';');
		for(var i = 0, len = list.length; i < len; i++){
			pair = list[i].split('=');
			cookies[pair[0].trim()] = pair[1];
		}
		return cookies;
	};
	var serializeCookie = function(name, value, options){
		var pairs = [name + '=' + encode(val)];
		options = options || {};
		if(options.expires) pairs.push('Expires=' + options.expires.toUTCString());
		if(options.maxAge) pairs.push('Max-Age=' + options.maxAge);
		if(options.path) pairs.push('Path=' + options.path);
		if(options.domain) pairs.push('Domain=' + options.domain);
		if(options.secure) pairs.push('Secure');
		if(options.httpOnly) pairs.push('HttpOnly');
		return pairs.join('; ');
	};
	
	// Session与安全[通过私钥加密口令进行签名]
	var sign = function(val, secret){
		return val + '.' + crypto
		  .createHmac('sha256', secret)
		  .update(val)
		  .digest('base64')
		  .replace(/\=+$/, '');
	};
	var unsign = function(val, secret){
		var str = val.slice(0, val.lastIndexOf('.'));
		return sign(str, secret) == val ? str : false;
	};
	
	// 缓存[根据内容生成散列值]
	var getHash = function(str){
		return crypto.createHash('sha1')
		  .update(str)
		  .digest('base64');
	};
	req.headers['if-modified-since']
	res.setHeader('Last-Modified', stat.mtime.toUTCString());
	req.headers['if-none-match']
	res.setHeader('ETag', getHash(file));
	res.setHeader('Expires', expires.toUTCString());
	res.setHeader('Cache-Control', 'max-age=' + 10 * 365 * 24 * 3600 * 1000);
	
	// Basic认证
	var encode = function(user, pass){
		return new Buffer(user + ':' + pass).toString('base64');
	};
	req.headers['authorization']
	res.setHeader('WWW-Authenticate', 'Basic realm="Secure Area"');
	
	// 数据上传
	var hasBody = function(req){
		return 'transfer-encoding' in req.headers || 'content-length' in req.headers;
	};
	var mime = function(req){
		var str = req.headers['content-type'] || '';
		return str.split(';')[0];
	};
	
	// CSRF(跨站请求伪造)
	var generateRandom = function(len){
		return crypto.randomBytes(Math.ceil(len * 3 / 4))
		  .toString('base64')
		  .slice(0, len);
	};
	
	// 内容相应
	// 1.MIME (Content-Type: text/plain|text/html|text/javascript...)
	res.writeHead(200, {
		"Content-Type": "text/plain; charset=utf-8"
	});
	res.end('Hello World');
	// 2.附件下载 (Content-Disposition: attachment|inline)
	var http = require('http');
	var path = require('path');
	var mime = require('mime');
	var fs = require('fs');
	http.createServer(function(req, res){
		res.sendfile = function(filepath, callback){
			fs.stat(filepath, function(err, stat){
				if(err){
					callback(err);
					return;
				}
				var stream = fs.createReadStream(filepath);
				res.setHeader("Content-Type", mime.getType(filepath) + "; charset=utf-8");
				res.setHeader("content-Length", stat.size);
				res.setHeader("content-Disposition": "attachment; filename=" + path.basename(filepath));
				res.writeHead(200);
				stream.pipe(res);
				res.u_filepath = filepath;
				callback(null, res);
			});
		};
	}).listen(8000, '0.0.0.0');
	// 3.响应JSON (Content-Type: application/json)
	res.json = function(json){
		res.setHeader("Content-Type", "application/json");
		res.writeHead(200);
		res.end(JSON.stringify(json));
	};
	// 4.响应跳转 (Location: exampleurl)
	res.redirect = function(url){
		res.setHeader("Location", url);
		res.writeHead(302);
		res.end('Redirect to ' + url);
	};
	
	// 模板安全
	var escape = function(html){
		return String(html)
		  .replace(/&(?!\w+;)/g, '&')
		  .replace(//g, '>')
		  .replace(/"/, '"')
		  .replace(/'/, ''');
	};
	
	var urlObj = url.parse(req.url, true);
	var query = urlObj.query;
	var url = url.format(urlObj);
```

<br />

<a name="bHNco"></a>
# 玩转进程


- 创建子进程
   - child_process.spawn()：创建一个子进程来执行特定命令，没有回调函数，只能通过监听事件，来获取运行结果，适用于子进程长时间运行的情况。
   - child_process.exec(command, options, callback)
   - child_process.execFile()
   - child_process.fork()
   - 【注】如果JS文件通过execFile运行，它的首行必须添加：#!/usr/bin/env node
- 进程间通信
   - message 事件
   - error 事件
   - exit 事件
   - close 事件
   - disconnect 事件
   - uncaughtException 事件
   - unhandledRejection 事件
   - process.send(message, [sendHandle])
      - 句柄类型（sendHandle）
      - net.Socket：TCP套接字
      - net.Server：TCP服务器
      - net.Native：C++层面的TCP套接字或IPC(Inter-Process Communication)管道
      - dgram.Socket：UDP套接字
      - dgram.Native：C++层面的UDP套接字
   - 【注】对于send()发送的句柄还原出的服务而言，它们的文件描述符相同，因此监听相同端口不会发生异常。
- cluster模块：解决多核CPU利用率问题



```javascript
	var cp = require('child_process');
	cp.spawn('node', ['worker.js']);
	cp.exec('node worker.js', function(err, stdout, stderr){
		
	});
	cp.execFile('worker.js', function(err, stdout, stderr){
		
	});
	cp.fork('./worker.js');
	// 或者
	var child = cp.exec('node worker.js');
	child.stdout.on('data', function(data){
		
	});
	child.stderr.on('data', function(data){
		
	});
	child.on('close', function(code){
		
	});
	
	// worker.js
	var http = require('http');
	http.createServer(function(req, res){
		res.writeHead(200, {"Content-Type": "text/plain"});
		res.end('hello world\n');
	}).listen(Math.round((1 + Math.random()) * 1000), '127.0.0.1');
	// master.js
	var fork = require('child_process').fork;
	var cpus = require('os').cpus();
	var server = require('net').createServer();
	server.listen('1337');
	var workers = {};
	var createWorker = function(){
		var worker = fork('./worker.js');
		worker.on('exit', function(){
			console.log('Worker ' + worker.pid + ' exited.');
			delete workers[worker.pid];
			createWorker();
		});
		worker.send('server', server);
		workers[worker.pid] = worker;
		console.log('Create worker. pid: ' + worker.pid);
	};
	for(var i = 0, len = cpus.length; i < len; i++){
		createWorker();
	}
	process.on('exit', function(){
		for(var pid in workers){
			workers[pid].kill();
		}
		console.log('process ' + process.pid + ' exited.');
	});
	console.log('process pid: ' + process.pid);
	
	// cluster模块
	var cluster = require('cluster');
	var numCPUs = require('os').cpus().length;
	cluster.setupMaster({
		"exec": "worker.js"
	});
	for(var i = 0; i < numCPUs; i++){
		cluster.fork();
	}
```

<br />

<a name="gTxtl"></a>
# 测试


- 测试风格
   - TDD：测试驱动开发（Test-driven development）
   - BDD：行为驱动开发（Behavior-driven development）
      - describe.only(string, function)
      - it.only(string, function)
      - describe.skip(string, function)
      - it.skip(string, function)
- 术语
   - 测试套件
   - 测试用例
   - 断言
      - assert 风格
      - expect 风格
      - should 风格
- 测试用例
   - 正向测试
   - 反向测试
   - [Node]异步代码
   - [Node]超时设置
- 断言库
   - shouldjs
   - chai
- 单元测试框架-mocha
   - ----浏览器端
   - mocha init [path]
   - ----服务器端
   - mocha.opts：所有mocha的命令行参数，都可以写在test目录下的配置文件mocha.opts之中
   - mocha -w [path]：监控测试文件
   - mocha --ui tdd
   - mocha --grep [name]：搜索包含name的测试用例执行
   - mocha --invert：只执行不符合条件的测试用例
   - mocha --compilers js:babel/register：使用ES6语法的话要指定babel转码
   - mocha --growl：将测试结果在桌面显示
   - mocha --recursive：执行所有测试用例
   - mocha --reporters [reportingTpl]
   - mocha --reporters：查看所有报告格式
   - mocha -R [reporter]：采用一种格式
   - mocha -t [ms]：设置所有用例超时时间（this.timeout(ms)：单个用例特殊设置）
   - mocha --require blanket -R html-cov > coverage.html：使用blanket测试覆盖率
- 模拟数据
   - nock
   - sinon
   - faux-jax
   - MITM
- 测试覆盖率
   - istanbul 模块
   - jscover 模块
   - blanket 模块
- 模拟异常（mock）-muk
   - 对于异步方法的模拟，需要小心是否将异步方法模拟为同步
- 私有方法的测试-rewire
- 性能测试
   - 基准测试-benchmark
   - 压力测试
- 测试数据与业务数据的转换
   - UV（独立访客-unique visitor）
   - QPS（每秒查询率-query per second） = PV（页面浏览量-page view） / 主要访问量小时数



```javascript
	// package.json
	{
		"devDependencies": {
			"mocha": "*"
		},
		"scripts": {
			"blanket": {
				"pattern": "eventproxy/lib"
			}
		}
	}
	
	// TDD风格
	suite('Array', function(){
		suiteSetup(function(){
			
		});
		setup(function(){
			
		});
		suite('#indexOf()', function(){
			test('should return -1 when not present', function(){
				assert.equal(-1, [1, 2, 3].index(4));
			});
		});
		teardown(function(){
			
		});
		suiteTeardown(function(){
			
		});
	});
	// BDD风格
	describe('Array', function(){
		before(function(){
			
		});
		/*
		beforeEach(function(){
			
		});
		*/
		describe('#indexOf()', function(){                       // 测试套件
			it('should return -1 when not present', function(){  // 测试用例
				[1, 2, 3].indexOf(4).should.equal(-1);           // 断言
			});
		});
		/*
		afterEach(function(){
			
		});
		*/
		after(function(){
			
		});
	});
	
	// 异步测试
	it('fs.readFile should be ok', function(done){
		fs.readFile('filepath', 'utf-8', function(err, file){
			should.not.exist(err);
			done();
		});
	});
	// 超时设置
	it('should take less than 500ms', function(done){
		this.timeout(500);
		// ...
		done();
	});
	
	// 私有方法的测试
	var limit = function(num){
		return num < 0 ? 0 : num;
	};
	it('limit should return success', function(){
		var lib = rewire('../lib/index.js');
		var limit = lib.__get__('limit');
		limit(10).should.be.equal(10);
	});
```

<br />

<a name="EONsH"></a>
# 产品化


- 监控报警
   - 报警实现
      - 邮件报警
      - 短信或电话报警
   - 监控系统的稳定性



```javascript
	// 邮件报警
	var nodemailer = require('nodemailer');
	var smtpTransport = nodemailer.createTransport("SMTP", {
		service: "Gmail",
		auth: {
			user: "user@gmail.com",
			pass: "userpass"
		}
	});
	var mailOptions = {
		from: "sender@bar.com",
		to: "receiver1@bar.com, receiver2@bar.com",
		subject: "Hello", // 主题
		text: "Hello World", // 纯文本内容
		html: "<b>Hello World<b>" // HTML内容
	};
	smtpTransport.sendMail(mailOptions, function(err, response){
		if(err){
			console.log(err);
			return;
		}
		console.log(response.message);
	});
```

<br />

<a name="P8kf7"></a>
# 调试Node


- debugger
- Node Inspector
   - V8的调试除了在命令行中通过debug启用外，对于已经运行的进程，可以通过向其发送SIGUSR1信号启用调试。
   - kill -s USR1 [PID]
