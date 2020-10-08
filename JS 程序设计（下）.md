# JS 程序设计（下）


<br />

<a name="7KDO4"></a>
# DOM
<br />

- Node类型 (DOM1级)
   - 【元素节点（1）】Node.ELEMENT_NODE
   - 【特性节点（2）】Node.ATTRIBUTE_NODE
   - 【文本节点（3）】Node.TEXT_NODE
   - 【（4）】Node.CDATA_SECTION_NODE
   - 【（5）】Node.ENTITY_REFERENCE_NODE
   - 【（6）】Node.ENTITY_NODE
   - 【（7）】Node.PROCESSING_INSTRUCTION_NODE
   - 【注释节点（8）】Node.COMMENT_NODE
   - 【文档节点（9）】Node.DOCUMENT_NODE
   - 【文档类型节点（10）】Node.DOCUMENT_TYPE_NODE
   - 【文档片段节点（11）】Node.DOCUMENT_FRAGMENT_NODE
   - 【（12）】Node.NOTATION_NODE
- Node类型
   - someNode.nodeType
   - someNode.nodeName
   - someNode.nodeValue
   - ---绝对路径(base标签)---
   - someNode.baseURI
   - ---节点关系---
   - someNode.ownerDocument
   - someNode.childNodes
   - someNode.hasChildNodes()
   - someNode.parentNode
   - someNode.previousSibling
   - someNode.nextSibling
   - someNode.firstChild
   - someNode.lastChild
   - ---操作节点---
   - someNode.appendChild(newNode)
   - someNode.insertBefore(newNode, refNode)
   - someNode.replaceChild(newNode, repNode)
   - someNode.removeChild(remNode)
   - 【注】在 appendChild、insertBefore 中，如果传入的 newNode 已经是文档的一部分，那么会将其从原位置移动到新位置。
   - ------
   - someNode.cloneNode(boolean)
   - someNode.normalize()
   - 【注】在 cloneNode 中，传入的参数表示是否克隆子节点。
- Element类型
   - someElement.tagName
   - ---HTML元素---
   - someElement.id
   - someElement.className
   - someElement.title
   - someElement.lang
   - someElement.dir
   - someElement.htmlFor(对应html for属性)
   - ---特性操作---
   - someElement.getAttribute()
   - someElement.setAttribute()
   - someElement.removeAttribute()
   - someElement.hasAttribute()
   - someElement.dataset
   - 【注1】style, onclick 特性通过 someElement.attrName 和 someElement.getAttribute(attrName) 返回值不同。
   - 【注2】dataset的键名转成属性名时，所有大写字母都会被转成连词线+该字母的小写形式。比如，dataset.helloWorld会转成data-hello-world。
   - ---attributes属性---
   - someElement.attributes.getNamedItem(name)
   - someElement.attributes.removeNamedItem(name)
   - someElement.attributes.setNamedItem(attrVar)
   - someElement.attributes.item(index)
   - ---操作元素---
   - someElement.closest()
   - someElement.match()
   - someElement.remove()
   - ---创建元素---
   - document.createElement()
   - 【注】在 IE 中也可以另一种方式创建元素，即为此元素传入完整的元素标签，也可包含属性。这种方式有助于避开在 IE<8 时动态创建元素的问题。
- Text类型
   - someElement.firstChild.length
   - someElement.firstChild.data
   - someElement.firstChild.appendData(text)
   - someElement.firstChild.deleteData(index, length)
   - someElement.firstChild.insertData(index, text)
   - someElement.firstChild.replaceData(index, length, text)
   - someElement.firstChild.splitText(index)
   - someElement.firstChild.substringData(index, length)
   - ---创建文本节点---
   - document.createTextNode()
- Document类型
   - ---子节点---
   - document.childNodes
   - document.documentElement
   - document.body
   - document.doctype
   - ---文档信息---
   - document.title
   - document.documentURI
   - document.URL
   - document.domain
   - document.referrer
   - document.lastModified
   - document.characterSet
   - document.location
   - document.readyState [loading|interactive加载外部资源|complete]
   - 【注】document.domain仅支持在二级域名情况下，写入对应的一级域名。
   - ---查找元素---
   - document.getElementById()
   - document.getElementsByTagName()
   - document.getElementsByName()
   - document.elementFromPoint(x, y)
   -   DOM扩展
   - document.querySelector(CSSSelector)
   - document.querySelectorAll(CSSSelector)
   -   HTML5
   - document.getElementsByClassName()
   - ---特殊集合---
   - document.links (包含文档中所有带 href 属性的 a 元素)
   - document.anchors (包含文档中所有带 name 属性的 a 元素)
   - document.forms
   - document.images
   - document.embeds
   - document.styleSheets
   - document.scripts
   - ---文档写入---
   - document.open()
   - document.close()
   - document.write()
   - document.writeln()
   - 【注】document.writeln会在写入内容后追加换行符。
   - ---DOM一致性检测---
   - document.implementation.hasFeature(DOMname, DOMversion)
- DocumentFragment类型
   - ---创建文档片段节点---
   - document.createDocumentFragment()
- 动态创建Stylesheet和Javascript
   - someStyle.sheet (someStyle.styleSheet)
   - someScript.text
- HTMLCollection与NodeList的区别
   - HTMLCollection实例对象的成员只能是Element节点，NodeList实例对象的成员可以包含其他节点。
   - HTMLCollection实例对象都是动态集合，节点的变化会实时反映在集合中。NodeList实例对象可以是静态集合。
   - HTMLCollection实例对象可以用id属性或name属性引用节点元素，NodeList只能使用数字索引引用。
```javascript
	// 类数组转换为数组
	function convertToArray(nodes){
		var array = null;
		try{
			// >=IE9可运行
			array = Array.prototype.alice.call(nodes, 0);
		}catch(e){
			array = [];
			for(var i = 0, len = nodes.length; i < len; i++){
				array.push(nodes[i]);
			}
		}
		return array;
	}
	
	var images = document.getElementsByTagName("img");
	console.log(images[0]);
	console.log(images.item(0));
	console.log(images["myImage"]);
	console.log(images.namedItem("myImage"));
	images[0].attributes.getNamedItem("id");
	images[0].attributes["id"].nodeName;
	images[0].attributes["id"].nodeValue;
	images[0].attributes["id"].specified;
	
	// form id="myForm" name="uniqueForm" data-id="test"
	document.forms.myForm
	document.forms.uniqueForm
	delete document.forms.myForm.dataset.id
	
	// 兼容DOM浏览器将空格算作文本节点
	for(var i = 0, len = someElement.childNodes.length; i < len; i++){
		// 确保是元素节点
		if(someElement.childNodes[i].nodeType == 1){
			
		}
	}
```

<br />

<a name="hMuLW"></a>
# DOM扩展
<br />

- 专有扩展
   - someElement.children
   - someElement.childElementCount
   - someElement.contains(otherElement)
   - someElement.compareDocumentPosition(otherElement)
   - someElement.innerText (someElement.textContent)
   - someElement.outerText
   - 【注1】document.body.contains(document.body) === true
   - 【注2】compareDocumentPosition [0(相同), 1(无关), 2(居前), 4(居后), 8(包含), 16(被包含), 32(私有用途)] [20 = 16 + 4, value & 4]
- 与类相关的扩充
   - someElement.classList.contains(value)
   - someElement.classList.add(value)
   - someElement.classList.remove(value)
   - someElement.classList.toggle(value, isAdd)
- 焦点管理
   - document.activeElement
   - document.hasFocus()
   - 【注】默认情况下，document.activeElement 保存的是 document.body 元素的引用。
- HTMLDocument变化
   - document.readyState [loading, complete]
   - document.compatMode [CSS1Compat, BackCompat]
   - document.head
- 字符集属性
   - document.charset
- 插入标记
   - someElement.innerHTML
   - someElement.outerHTML
   - someElement.insertAdjacentHTML(identifier, string)
   - 【注01】identifier
      - beforebegin: 在当前元素节点的前面
      - afterbegin: 在当前元素节点的里面，插在它的第一个子元素之前
      - beforeend: 在当前元素节点的里面，插在它的最后一个子元素之后
      - afterend: 在当前元素节点的后面
   - 【注02】内存与性能问题：最好先手工删除要被替换元素的所有事件处理程序和 JavaScript 对象属性。
- 页面滚动
   - someElement.scrollIntoView(boolean)
```javascript
	function contains(refNode, otherNode){
		if(typeof refNode.contains == "function" && (!client.engine.webkit || client.engine.webkit >= 522)){
			return refNode.contains(otherNode);
		}else if(typeof refNode.compareDocumentPosition == "function"){
			return !!(refNode.compareDocumentPosition(otherNode) & 16);
		}else{
			var node = otherNode.parentNode;
			do{
				if(refNode == node){
					return true;
				}else{
					node = node.parentNode;
				}
			}while(node !== null);
			return false;
		}
	}
```

<br />

<a name="UoBtG"></a>
# DOM2和DOM3(部分或全部浏览器支持)


- 综述
   - document.adoptNode(oldNode)
   - document.importNode(oldNode, copyType)
   - document.defaultView [document.parentWindow (IE)]
   - document.implementation.createDocumentType(name, publicId, systemId)
   - document.implementation.createDocument(namespaceURI, tagName, documentType)
   - document.implementation.createHTMLDocument(titleName)
   - someNode.isSupported(name, version)
   - someNode.isSameNode(node)
   - someNode.isEqualNode(node)
   - someNode.setUserData(key, value, function(operationType, key, value, sourceNode, targetNode){})
   - someNode.getUserData(key)
   - someIFrame.contentDocument [someIFrame.contentWindow.document (all browser)]
- window.matchMedia
   - window.matchMedia(mediaQuery)
   - window.matchMedia(mediaQuery).addListener(function)
   - window.matchMedia(mediaQuery).removeListener(function)
- CSS事件
   - transitionend 事件
   - animationstart 事件
   - animationend 事件
   - animationiteration 事件
- MutationObserver
   - new MutationObserver(callback)
   - mo.observe(DOM, options)：指定所要观察的DOM节点，以及所要观察的特定变动
      - childList：子节点的变动
      - attributes：属性的变动
      - characterData：节点内容或节点文本的变动
      - subtree：所有后代节点的变动
   - mo.disconnect()：用来停止观察，再发生相应变动，就不再调用回调函数
   - mo.takeRecords()：用来清除变动记录，即不再处理未处理的变动。该方法返回变动记录的数组
- 访问元素的样式
   - someElement.style.cssFloat [someElement.style.styleFloat (IE)]
   - ------
   - someElement.style.length
   - someElement.style.cssText
   - someElement.style.getPropertyValue(propertyName)
   - someElement.style.getPropertyCSSValue(propertyName)
   - someElement.style.getPropertyPriority(propertyName)
   - someElement.style.item(index)
   - someElement.style.setProperty(propertyName, value, ['important'|''])
   - someElement.style.removeProperty(propertyName)
   - someElement.style.parentRule
   - ------
   - (document.defaultView.)getComputedStyle(node, [':before'|':after']) [someElement.currentStyle (IE)]
- 操作样式表
   - document.styleSheets
   -   StyleSheet接口
   - document.styleSheets[i].disabled
   - document.styleSheets[i].type
   - document.styleSheets[i].href
   - document.styleSheets[i].media
   - document.styleSheets[i].title
   - document.styleSheets[i].parentStyleSheet
   - document.styleSheets[i].ownerNode [(IE不支持)]
   -   CSSStyleSheet接口
   - document.styleSheets[i].cssRules [rules (IE)]
   - document.styleSheets[i].ownerRule [(IE不支持)]
   - document.styleSheets[i].deleteRule(index) [removeRule(index) (IE)]
   - document.styleSheets[i].insertRule(rule, index) [addRule(selector, CSSText, index) (IE)]
- 元素大小
   - someElement.offsetParent
   - someElement.offsetLeft：左外边框到包含元素的左内边框之间的像素距离。
   - someElement.offsetTop
   - someElement.offsetWidth：包括元素的宽度、（可见的）垂直滚动条的宽度、左边框和右边框宽度。
   - someElement.offsetHeight
   - someElement.clientLeft：包括元素左边框宽度、包含滚动条宽度（除非排版从右到左）。display:inline一律为0。
   - someElement.clientTop
   - someElement.clientWidth：包括元素内容区宽度、内边距宽度、不包含滚动条宽度。
   - someElement.clientHeight
   - someElement.scrollLeft：通过设置本属性可以改变元素滚动位置。
   - someElement.scrollTop
   - someElement.scrollWidth：在没有滚动条的情况下，元素内容的总宽度。
   - someElement.scrollHeight
   - someElement.getBoundingClientRect() ({json} [left, top, right, bottom])
   - someElement.getClientRects()
- 遍历和范围
   - document.createNodeIterator(root, whatToShow, filter, entityReferenceExpansion)
   - document.createTreeWalker(root, whatToShow, filter, entityReferenceExpansion)
   - document.createRange()
   - 【注01】兼容检测：document.implementation.hasFeature("Range", "2.0")
   - 【注02】兼容检测：typeof document.createRange == "function"
   - 【注03】document.createTreeWalker方法返回一个DOM的子树遍历器，与document.createNodeIterator方法的区别在于，后者只遍历子节点，而它遍历整个子树
```javascript
	// 变动观察器
	var callback = function(records){
		records.map(function(record){
			console.log("Mutation type: " + record.type);
			console.log("Mutation target: " + record.target);
		});
	};
	var mo = new MutationObserver(callback);
	mo.observe(document.body, {
		"childList": true,
		"subtree": true
	});
	
	// 获取样式表对象
	function getStyleSheet(ele){
		return ele.sheet || ele.styleSheet;
	}
	// 添加规则
	function insertRule(sheet, selector, CSSText, index){
		if(sheet.insertRule){
			sheet.insertRule(selector + "{" + CSSText + "}", index);
		}else if(sheet.addRule){
			sheet.addRule(selector, CSSText, index);
		}
	}
	
	// CSS规则
	var sheet = document.styleSheets[0];
	var rules = sheet.cssRules || sheet.rules;
	var rule = rules[0];
	console.log(rule.selectorText);
	console.log(rule.style.cssText);
	
	// 客户区大小
	var viewportWidth = document.documentElement.clientWidth || document.body.clientWidth,
	    viewportHeight = document.documentElement.clientHeight || document.body.clientHeight;
	
	// 滚动大小
	var docWidth = Math.max(document.documentElement.scrollWidth, document.documentElement.clientWidth),
	    docHeight = Math.max(document.documentElement.scrollHeight, document.documentElement.clientHeight);
	
	// 确定元素大小
	// 假定所有元素都适合它的容器，不存在内容溢出
	function getElementLeftTop(ele){
		var actualLeft = 0,
			actualTop = 0;
		while(ele !== null){
			actualLeft += ele.offsetLeft;
			actualTop += ele.offsetTop;
			ele = ele.offsetParent;
		}
		return {
			left: actualLeft,
			top: actualTop
		};
	}
	function getBoundingClientRect(ele){
		// 兼容Safari浏览器
		var scrollLeft = document.documentElement.scrollLeft || document.body.scrollLeft,
		    scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		if(ele.getBoundingClientRect){
			if(typeof arguments.callee.offset != "number"){
				var temp = document.createElement("div");
				temp.style.cssText = "position:absolute;left:0;top:0;";
				document.body.appendChild(temp);
				arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
				document.body.removeChild(temp);
				temp = null;
			}
			var rect = ele.getBoundingClientRect();
			var offset = arguments.callee.offset;
			return {
				left: rect.left + offset,
				top: rect.top + offset,
				right: rect.right + offset,
				bottom: rect.bottom + offset
			};
		}else{
			var actualLeft = getElementLeft(ele);
			var actualTop = getElementTop(ele);
			return {
				left: actualLeft - scrollLeft,
				top: actualTop - scrollTop,
				right: actualLeft + ele.offsetWidth - scrollLeft,
				bottom: actualTop + ele.offsetHeight - scrollTop
			};
		}
	}
```

<br />

<a name="qcTcI"></a>
# 事件
<br />

- DOM0 级事件处理程序
   - 将一个函数赋值给一个事件处理程序属性。
   - 将事件处理程序属性的值设置为 null 也可以删除通过 DOM0 级方法指定的事件处理程序。
- DOM2 级事件处理程序
   - 可以添加多个事件处理程序。
   - 通过 addEventListener() 添加的匿名函数无法移除，它们不是同一个函数。除非使用 arguments.callee 来引用该函数。
   - addEventListener(eventName, eventHandler, boolean)
   - removeEventListener(eventName, eventHandler, boolean)
   - ------
   - 在使用 attachEvent() 方法时，事件处理程序会在全局作用域中运行，this 等于 window。
   - 多个事件处理程序以与 DOM 事件处理程序相反的顺序执行。
   - attachEvent(eventName, eventHandler)
   - detachEvent(eventName, eventHandler)
- 事件类型
   - UI事件
   - 焦点事件
   - 鼠标事件
   - 滚轮事件
   - 文本事件
   - 键盘事件
   - 合成事件：当为IME(Input Method Editor, 输入法编辑器)输入字符时触发。
   - 变动事件：当底层DOM结构发生变化时触发。
- 焦点事件
   - focus 事件
   - blur 事件
   - focusin 事件
   - focusout 事件
- HTML5事件
   - contextmenu 事件
   - DOMContentLoaded 事件
   - pageshow 和 pagehide 事件
   - hashchange 事件
- Touch对象
   - target
   - identifier
   - screenX|screenY
   - clientX|clientY
   - pageX|pageY
   - radiusX|radiusY
   - rotationAngle
   - force
- TouchList对象
   - length
   - identifiedTouch()
- TouchEvent对象
   - ctrlKey
   - altKey
   - shiftKey
   - metaKey
   - touches
   - targetTouches
   - changedTouches
- 模拟事件
   - document.createEvent(eventName)
   - someElement.dispatchEvent(event)
   - ------
   - UIEvent
   - MouseEvent
   - MutationEvent
   - CustomEvent
   - 【注1】dispatchEvent方法返回一个布尔值，只要有一个监听函数调用了Event.preventDefault()，则返回值为false。
   - 【注2】Event.stopImmediatePropagation可以阻止同一节点上同一事件类型的所有监听函数。
```javascript
	var event = new Event('look', {
		"bubbles": true,
		"cancelable": true // 是否可以取消事件默认行为
	});
	someElement.dispatchEvent(event);
	var event2 = new CustomEvent('look', {
		"detail": {
			"foo": "bar"
		},
		"bubbles": true,
		"cancelable": false
	});
	var event3 = new ProgressEvent(type, {
		"lengthComputable": false,
		"loaded": 0,
		"total": 0
	});
	
	document.getElementById("ele").onclick = function(e){
		    // 事件对象
		var e = e || event,
		    // 事件类型
		    type = e.type,
		    // 事件目标
		    target = e.target || e.srcElement;
		// 取消事件默认行为
		e.preventDefault ? e.preventDefault() : e.returnValue = false;
		// 取消事件冒泡
		e.stopPropagation ? e.stopPropagation() : e.cancelBubble = true;
		
		// 客户区坐标
		var clientX = e.clientX,
		    clientY = e.clientY;
		// 页面坐标
		var pageX = e.pageX,
		    pageY = e.pageY;
		// 屏幕坐标
		var screenX = e.screenX,
		    screenY = e.screenY;
		// 修改键，键盘事件也支持
		var isAltKey = e.altKey,
		    isCtrlKey = e.ctrlKey,
		    isMetaKey = e.metaKey,
		    isShiftKey = e.shiftKey;
		
		// 在发生mouseout和mouseover事件时，还会涉及更多元素
		var relatedEle = e.relatedTarget;
		
		// 鼠标按钮，针对mousedown和mouseup
		var button = e.button;
			// 0 -- 按下主鼠标按钮
			// 1 -- 按下鼠标滚轮按钮
			// 2 -- 按下次鼠标按钮
		
		// 鼠标滚轮
		Firefox浏览器：DOMMouseScroll -- e.detail * (-40)
		其他浏览器：mousewheel -- e.wheelDelta
		
		// 在发生keydown和keyup事件时(keyCode)或keypress事件时(charCode)
		var keyCode = e.keyCode || e.which || e.charCode;
	};
```

<br />

<a name="Gp9v7"></a>
# 表单脚本
<br />

- 公共事件
   - focus 事件
   - blur 事件
   - change 事件
- 公共属性
   - someElement.form
   - someElement.type
   - someElement.value
   - someElement.name
   - someElement.disabled
   - someElement.readOnly
   - someElement.tabIndex
- 公共方法
   - someElement.focus()
   - someElement.blur()
- 选择文本
   - select 事件
   - someElement.selectionStart
   - someElement.selectionEnd
   - someElement.select()
   - someElement.setSelectionRange(start, end)
- 操作剪贴板
   - beforecopy 事件
   - beforecut 事件
   - beforepaste 事件
   - copy 事件
   - cut 事件
   - paste 事件
   - e.clipboardData.items
   - e.clipboardData.setData()
   - e.clipboardData.getData()
   - e.clipboardData.clearData()
- 选择框脚本
   - selectElement.options
   - selectElement.multiple
   - selectElement.size
   - selectElement.selectedIndex
   - selectElement.add(newOption, relaOption)
   - selectElement.remove(index)
   - ------
   - optionElement.text
   - optionElement.value
   - optionElement.index
   - optionElement.label
   - optionElement.selected
   - optionElement.defaultSelected
   - 【注】设为多选时，value只返回选中的第一个选项。
- 富文本编辑
   - contenteditable 特性
   - document.designMode [on|off]
   - document.execCommand(commandName, boolean, commandValue)
   - document.queryCommandEnabled(commandName)
   - document.queryCommandState(commandName)
   - document.queryCommandValue(commandName)
```javascript
	function serialize(form){
		var parts = [],
		    field = null,
		    option,
		    optValue;
		for(var i = 0, len = form.elements.length; i < len; i++){
			field = form.elements[i];
			if(field.name.length && !field.disabled){
				switch(field.type){
					case "select-one":
					case "select-multiple":
				    	for(var j = 0, optLen = field.options.length; j < optLen; j++){
				    		option = field.options[j];
				    		if(options.selected){
				    			optValue = "";
				    			if(option.hasAttribute){
				    				optValue = (option.hasAttribute("value") ? option.value : option.text);
				    			}else{
				    				optValue = (option.attributes["value"].specified ? option.value : option.text);
				    			}
				    			parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(field.optValue));
				    		}
				    	}
					    break;
					case undefined:
					case "file":
					case "submit":
					case "reset":
					case "button":
					    break;
					case "radio":
					case "checkbox":
					    if(!field.checked){
					    	break;
					    }
					default:
					    parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(field.value));
				}
			}
		}
		return parts.join("&");
	}
```

<br />

<a name="8H4n8"></a>
# canvas绘图
<br />

- canvasElement.toDataURL("image/jpeg", "1.0")
- canvasElement.getContext("2d")
- 2D上下文
   - ----填充和描边
   - ctx.fillStyle
   - ctx.strokeStyle
   - ----绘制矩形
   - ctx.fillRect(x, y, width, height)
   - ctx.strokeRect(x, y, width, height)
   - ctx.clearRect(x, y, width, height)
   - ctx.lineWidth
   - ctx.lineCap [butt, round, square]
   - ctx.lineJoin [round, bevel, miter]
   - ----绘制路径
   - ctx.beginPath()
   - ctx.closePath()
   - ctx.fill()
   - ctx.stroke()
   - ctx.clip()
   - ctx.moveTo(x, y)
   - ctx.lineTo(x, y)
   - ctx.rect(x, y, width, height)
   - ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise)
   - ctx.arcTo(x1, y1, x2, y2, radius)
   - ctx.bezierCurveTo(c1x, c1y, c2x, c2y, x, y)
   - ctx.quadraticCurveTo(cx, cy, x, y)
   - ctx.isPointInPath(x, y)
   - ----绘制文本
   - ctx.font
   - ctx.textAlign [start, end, center, left, right]
   - ctx.textBaseline [top, bottom, middle, hanging, alphabetic, ideographic]
   - ctx.fillText(text, x, y[, maxWidth])
   - ctx.strokeText(text, x, y[, maxWidth])
   - ctx.measureText(text)
   - ----变换
   - ctx.translate(x, y)
   - ctx.rotate(angle)
   - ctx.scale(scaleX, scaleY)
   - ctx.transform(m1_1, m1_2, m2_1, m2_2, dx, dy)
   - ctx.setTransform(m1_1, m1_2, m2_1, m2_2, dx, dy)
   - ----上下文环境
   - ctx.save()
   - ctx.restore()
   - ----绘制图像
   - ctx.drawImage(image|canvas, tX, tY[, tWidth, tHeight])
   - ctx.drawImage(image|canvas, sX, sY, sWidth, sHeight, tX, tY, tWidth, tHeight)
   - ----阴影
   - ctx.shadowColor
   - ctx.shadowOffsetX
   - ctx.shadowOffsetY
   - ctx.shadowBlur
   - ----渐变
   - ctx.createLinearGradient(startX, startY, endX, endY)
   - ctx.createRadialGradient(startX, startY, startRadius, endX, endY, endRadius)
   - gradient.addColorStop(position, color)
   - ----模式
   - ctx.createPattern(image|video|canvas, repeat)
   - 【注】需要注意的是，模式和渐变一样，都是从画布的原点(0, 0)开始的。将填充样式设置为模式对象不是要从某个位置开始绘制重复的图像。
   - ----图像数据
   - ctx.getImageData(x, y, width, height)
   - ctx.putImageData(imageData, x, y)
   - ----合成
   - 设置透明度：ctx.globalAlpha
   - 设置图形结合方式：ctx.globalCompositionOperation


<br />

<a name="IG6Xz"></a>
# HTML5脚本编程
<br />

- 跨文档消息传递(XDM, Web Messaging)
   - message 事件
   - e.source：发送消息的窗口
   - e.origin: 消息发向的网址
   - e.data: 消息内容
   - iframeNode.contentWindow.postMessage(data, domain)
- 原生拖放
   - ----拖放特性
   - draggable
   - ----拖放事件
   - dragstart
   - drag
   - dragend
   - dragenter
   - dragover
   - drop或者dragleave
   - ----拖放事件对象
   - e.dataTransfer.types
   - e.dataTransfer.files
   - e.dataTransfer.setData(MIME, text)
   - e.dataTransfer.getData(MIME)
   - e.dataTransfer.clearData(MIME)
   - e.dataTransfer.setDragImage(image|canvas, x, y)
   - e.dataTransfer.effectAllowed [uninitialized|none|copy|link|move|copyMove|copyLink|linkMove|all]
   - e.dataTransfer.dropEffect [none|copy|link|move]
   - 【注】MIME
      - text/plain
      - text/uri-list
      - text/html


<br />

<a name="QQuTY"></a>
# 错误处理与调试
<br />

- try-catch语句
   - Error
   - EvalError (eval错误)
   - RangeError (范围错误)
   - ReferenceError (对象错误)
   - SyntaxError (语法错误)
   - TypeError (类型错误)
   - URIError (URI错误)
- throw操作符
- 调试技术
   - console.log(message)
   - console.error(message)
   - console.info(message)
   - console.warn(message)
   - console.dir(object)：查看对象所有属性和方法
```javascript
	try{
		
	}catch(error){
		console.log(error.message);
	}finally{
		
	}
	
	throw 123;
	throw new SyntaxError("test");
	
	function logError(iden, msg){
		var img = new Image();
		img.src = "log.php?iden=" + encodeURIComponent(iden) + "&msg=" + encodeURIComponent(msg);
	}
```

<br />

<a name="KDeN0"></a>
# JSON
<br />

- 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
- 简单类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
- 序列化JavaScript对象时，如果原始对象中，有一个成员的值是undefined、函数或XML对象，这个成员会被省略。如果数组的成员是undefined、函数或XML对象，则这些值被转成null。正则对象会被转成空对象。
- JSON字符串必须使用双引号。
- toJSON()
- JSON.stringify(json[, array|function(过滤器), number|string](缩进选项))
- JSON.parse(jsonText[, function])
```javascript
	var book = {
		"title": "Profession",
		"authors": [
		    "Nicholas"
		],
		"edition": 3,
		"year": 2011,
		"date": new Date(2011, 11, 1),
		
		"reg": /^\d$/,
		
		get func(){
			return this.year + '-' + this.edition;
		},
		
		//"toJSON": function(){
		//	return undefined;
		//}
	};
	RegExp.prototype.toJSON = RegExp.prototype.toString;
	
	JSON.stringify(book, function(key, value){
		console.log(key);// 正常地，本例执行六次，第一次键名为空，键值是整个对象
		
		switch(key){
			case "authors":
			    return value.join();
			case "edition":
			    return undefined;// 返回undefined，忽略本属性
			case "year":
			    return 5000;
			default:
			    return value;
		}
	});
	JSON.stringify(book, null, "\t");
	
	JSON.parse(JSON.stringify(book), function(key, value){
		if(key === "edition"){
			return undefined;// 返回undefined，忽略本属性
		}
		if(key === "date"){
			return new Date(value);
		}
		return value;
	});
```

<br />

<a name="89m0r"></a>
# Ajax


- 1999年，微软公司发布IE浏览器5.0版，第一次引入新功能：允许JavaScript脚本向服务器发起HTTP请求。
- XHR 用法
   - var xhr = new XMLHttpRequest()
   - xhr.onreadystatechange = function(){}
   - xhr.open(type, url, async)
   - xhr.setRequestHeader(name, value)
   - xhr.send(data|null)
   - xhr.getResponseHeader(name)
   - xhr.getAllResponseHeaders()
   - ------
   - xhr.abort()
   - xhr = null
   - ------
   - xhr.readyState
      - XMLHttpRequest.UNSENT（0）：未初始化
      - XMLHttpRequest.OPENED（1）：启动
      - XMLHttpRequest.HEADERS_RECEIVED（2）：发送
      - XMLHttpRequest.LOADING（3）：接收
      - XMLHttpRequest.DONE（4）：完成
   - xhr.status [成功：2xx|304]
   - xhr.response
   - xhr.responseText
   - xhr.responseXML
   - xhr.responseType
      - text
      - json
      - document
      - blob
      - arraybuffer
   - xhr.withCredentials：表示跨域请求时，用户信息（比如Cookie和认证的HTTP头信息）是否会包含在请求之中，默认为false。
   - xhr.overrideMimeType(MIME)
   - xhr.statusText
- XHR Level 2
   - 进度事件：
   - xhr.onloadstart
   - xhr.upload.onloadstart
   - xhr.upload.onprogress
   - xhr.upload.onload
   - xhr.upload.onabort
   - xhr.upload.ontimeout
   - xhr.upload.onerror
   - xhr.upload.onloadend
   - xhr.onprogress
   - xhr.onload
   - xhr.onabort
   - xhr.ontimeout
   - xhr.onerror
   - xhr.onloadend
   - ------
   - var formdata = new FormData(formElement)
   - formdata.append(name, value)
   - formdata.append(name, file[, filename])
   - formdata.append(name, blob[, filename])
- SSE(Server-Sent Events, 服务器发送事件)
   - new EventSource(url)
   - source.readyState
   - source.close()
   - open 事件
   - message 事件
      - e.data
      - e.origin
      - e.lastEventId
   - error 事件
   - 自定义事件
   - 数据格式
      - field: value\n[\n]
      - --field
      - data：数据栏
      - event：自定义信息类型
      - id：数据标识符
      - retry：最大间隔时间
- Fetch API
   - 传统的XMLHttpRequest对象不支持数据流，所有的数据必须放在缓存里，等到全部获取后，再一次性输出。
   - fetch
      - fetch(request).then(function).catch(function)
   - Headers
      - var headers = new Headers(object)
      - headers.has()
      - headers.get()
      - headers.getAll()
      - headers.set()
      - headers.delete()
      - headers.append()
   - Request
      - var request = new Request(url, {headers: headers})
      - request.referrer（只读）
      - request.context（只读）
      - request.url
      - request.method
      - request.mode [same-origin|no-cors(默认：允许图片脚本等)|cors]
      - request.headers
      - request.body （body属性数据类型可为：ArrayBuffer|ArrayBufferView|Blob/File|URLSearchParams|FormData|String）
      - ----数据流读取器
      - request.clone()
      - request.bodyUsed
      - request.text()
      - request.json()
      - request.formData()
      - request.blob()
      - request.arrayBuffer()
   - Response
      - var response = new Response(jsonstring, {status: 200, headers: headers})
      - ----Response 方法
      - Response.error()
      - Response.redirect(url, status)
      - ----Response 实例方法
      - response.ok （status为2xx）
      - response.status
      - response.type [default|error|basic：正常的同域请求|cors：CORS机制下的跨域请求|opaque：非CORS机制下的跨域请求，这时无法读取返回的数据，也无法判断是否请求成功]
      - response.url
      - response.statusText
      - response.headers.get(name)
      - response.body.getReader()
      - ----数据流读取器
      - response.clone()
      - response.bodyUsed
      - response.text()
      - response.json()
      - response.formData()
      - response.blob()
      - response.arrayBuffer()
- form 表单
   - GET
   - POST - application/x-www-form-urlencoded
   - POST - text/plain
   - POST - multipart/form-data
- 同源策略
   - 通过监听hashchange事件，使用window.location.hash。
   - 各个浏览器对window.name值的储存容量有所不同，但是一般来说，可以高达几MB。缺点是必须监听window.name属性改变影响性能。（与iframe窗口通信时非常有用。）
   - XDM通信
   - AJAX
      - 架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务）
      - JSONP：网页通过添加一个script元素，向服务器请求JSON数据，服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。
      - WebSocket
      - CORS
- CORS
   - 简单请求 (simple request)
      - Access-Control-Allow-Origin: [http://api.bob.com](http://api.bob.com)
      - Access-Control-Allow-Credentials: true
      - Access-Control-Expose-Headers: FooBar
      - 【注】如果要发送Cookie，Access-Control-Allow-Origin就不能设为星号，必须指定明确的、与请求网页一致的域名。
   - 非简单请求 (not-so-simple request)
      - 增加一次HTTP查询请求（预检请求）：
      - Access-Control-Request-Method: PUT
      - Access-Control-Request-Headers: X-Custom-Header
      - Access-Control-Allow-Origin: [http://api.bob.com](http://api.bob.com)
      - Access-Control-Allow-Credentials: true
      - Access-Control-Allow-Methods: GET, POST, PUT
      - Access-Control-Allow-Headers: X-Custom-Header
      - Access-Control-Max-Age: 1728000（预检请求有效期）
```javascript
	// XMLHttpRequest
	let xhr = new XMLHttpRequest();
	xhr.onreadystatechange = function(){
		if(xhr.readyState == 4){
			if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
				console.log(xhr.response);
			}else{
				
			}
		}
	};
	xhr.open('get', url, true);
	xhr.send(null);
	// XMLHttpRequest 2
	let xhr = new XMLHttpRequest();
	xhr.onload = function(){
		if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
			console.log(xhr.response);
		}else{
			
		}
	};
	xhr.open('get', url, true);
	xhr.send(null);
	
	var blob = new Blob(['hello world'], {type: "text/plain"});
	
	// CORS跨域请求
	fetch(url, {
		mode: 'cors',
		credentials: 'include' // 指定cookie连同HTTP请求一起发出
	}).then(function(res){
		return res.json();
	}).then(function(res){
		
	});
	
	// POST请求
	fetch(url, {
		method: "POST",
		headers: {
			"Content-Type": "application/x-www-form-urlencoded"
		},
		body: "a=b&c=d"
	}).then(function(res){
		
	});
	
	// 数据流处理
	var url = 'largeFile.txt';
	var progress = 0;
	var contentLength = 0;
	var getStream = function(reader){
		return reader.read().then(function(res){
			if(res.done) return;
			var chunk = res.value;
			var text = '';
			// 假定数据是UTF-8编码，前三字节是数据头，而且每个字符占据一个字节（即都为英文字符）
			for(var i = 3, len = chunk.byteLength; i < len; i++){
				text += String.fromCharCode(chunk[i]);
			}
			document.getElementById('content').innerHTML += text;
			progress += chunk.byteLength;
			console.log((progress / contentLength * 100) + '%');
			return getStream(reader);
		});
	};
	fetch(url).then(function(res){
		if(res.ok){
			contentLength = res.headers.get('Content-Length');
			return getStream(res.body.getReader());
		}
	}).catch(function(){
		
	});
	
	function addUrlParam(url, name, value){
		url += (url.indexOf("?") == -1 ? "?" : "&");
		url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
		return url;
	}
```

<br />

<a name="7QHE6"></a>
# 高级技巧
<br />

- 高级函数
   - 较安全的类型检测
   - 作用域安全的构造函数
   - 函数绑定
   - 函数柯里化
- 防篡改对象
   - ----不可扩展属性和方法
   - Object.isExtensible()
   - Object.preventExtensions()
   - ----不可扩展、删除属性和方法
   - Object.isSealed()
   - Object.seal()
   - ----不可扩展、删除、修改属性和方法
   - Object.isFrozen()
   - Object.freeze()
- 高级定时器
   - 数组分块(array chunking)技术
   - 函数节流
- 自定义事件
- 拖放
```javascript
	// 较安全的类型检测
	function isTypeof(value){
		switch(typeof value){
			case "boolean":
				return "boolean";
			case "number":
				return "number";
			case "string":
				return "string";
			case "undefined":
				return "undefined";
			case "function":
				return "function";
			case "object":
				switch(Object.prototype.toString.call(value)){
					case "[object Null]":
						return "null";
					case "[object Array]":
						return "array";
					case "[object RegExp]":
						return "regexp";
					case "[object Date]":
						return "date";
					case "[object Math]":
						return "Math";
					case "[object JSON]":
						return "JSON";
					case "[object global]":// Safari
					case "[object Window]":// IE,Chrome,Firefox
						return "Global";
					case "[object HTMLDocument]":// Chrome,Safari,Firefox
					case "[object Document]":// IE
						return "Document";
					default:
						return "object";// 非原生构造函数的实例,{}等
				}
			default:
				return "unknown";
		}
	}
	
	// 作用域安全的构造函数
	function Person(name, age){
		if(this instanceof Person){
			this.name = name;
			this.age = age;
		}else{
			return new Person(name, age);
		}
	}
	
	// 函数绑定
	function bind(fn, scope){
		return function(){
			return fn.apply(scope, arguments);
		};
	}
	Function.prototype.bind = function(){
		var fn = this,
			scope = arguments[0],
			args = Array.prototype.slice.call(arguments, 1);
		return function(){
			return fn.apply(scope, args);
		};
	};
	
	// 函数柯里化
	function curry(fn){
		var args = Arrar.prototype.slice.call(arguments, 1);
		return function(){
			var innerArgs = Array.prototype.slice.call(arguments);
			var resArgs = args.concat(innerArgs);
			return fn.apply(null, resArgs);
		};
	}
	function curry(fn, scope){
		var args = Arrar.prototype.slice.call(arguments, 2);
		return function(){
			var innerArgs = Array.prototype.slice.call(arguments);
			var resArgs = args.concat(innerArgs);
			return fn.apply(scope, resArgs);
		};
	}
	
	// 函数节流
	function throttle(fn, scope){
		clearTimeout(fn.tId);
		fn.tId = setTimeout(function(){
			fn.call(scope);
		}, 100);
	}
	
	// 自定义事件
	function EventTarget(){
		this.handlers = {};
	}
	EventTarget.prototype = {
		constructor: this,
		addHandler: function(type, handler){
			if(typeof this.handlers[type] === "undefined"){
				this.handlers[type] = [];
			}
			this.handlers[type].push(handler);
			return this;
		},
		fire: function(event){
			if(!event.target){
				event.target = this;
			}
			if(this.handlers[event.type] instanceof Array){
				var handlers = this.handlers[event.type];
				for(var i = 0, len = handlers.length; i < len; i++){
					if(typeof handlers[i] === "function"){
						handlers[i](event);
					}
				}
			}
			return this;
		},
		removeHandler: function(type, handler){
			var handlers = this.handlers[type];
			if(handlers instanceof Array){
				if(typeof handler === "function"){
					for(var i = 0, len = handlers.length; i < len; i++){
						if(handlers[i] === handler){
							handlers.splice(i, 1);
							break;
						}
					}
				}else{
					delete this.handlers[type];
				}
			}
			return this;
		}
	};
	function Person(options){
		// 参见`寄生组合继承`
		EventTarget.call(this);
		this.name = options.name || "";
		this.age = options.age || 0;
	}
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
	inheritPrototype(EventTarget, Person);
	Person.prototype.say = function(message){
		this.fire({
			"type": "message",
			"message": message
		});
	};
	function handlerMessage(event){
		console.log(event.target.name + " says: " + event.message);
	}
	var person = new Person({
		"name": "test",
		"age": 18
	});
	person.addHandler("message", handlerMessage);
	person.say("Hello, everyone.");
	
	// 拖放
	var DragDrop = function(){
		var dragdrop = new EventTarget(),
			dragging = null,
			diffX = 0,
			diffY = 0;
		function handleEvent(e){
			var e = e || event,
				target = e.target || e.srcElement;
			switch(e.type){
				case "mousedown":
					if(target.className.indexOf("draggable") > -1){
						dragging = target;
						diffX = e.clientX - target.offsetLeft;
						diffY = e.clientY - target.offsetTop;
						dragdrop.fire({
							"type": "dragstart",
							"target": dragging,
							"x": e.clientX,
							"y": e.clientY
						});
					}
					break;
				case "mousemove":
					if(dragging != null){
						dragging.style.left = (e.clientX - diffX) + "px";
						dragging.style.top = (e.clientY - diffY) + "px";
						dragdrop.fire({
							"type": "drag",
							"target": dragging,
							"x": e.clientX,
							"y": e.clientY
						});
					}
					break;
				case "mouseup":
					if(dragging != null){
						dragdrop.fire({
							"type": "dragend",
							"target": dragging,
							"x": e.clientX,
							"y": e.clientY
						});
						dragging = null;
					}
					break;
			}
		}
		dragdrop.enable = function(){
			document.addEventListener("mousedown", handleEvent, false);
			document.addEventListener("mousemove", handleEvent, false);
			document.addEventListener("mouseup", handleEvent, false);
			return dragdrop;
		};
		dragdrop.disable = function(){
			document.removeEventListener("mousedown", handleEvent, false);
			document.removeEventListener("mousemove", handleEvent, false);
			document.removeEventListener("mouseup", handleEvent, false);
			return dragdrop;
		};
		return dragdrop;
	}();
	DragDrop.addHandler("drag", function(e){
		console.log(e.type);
		console.log(e.target);
		console.log(e.x);
		console.log(e.y);
	});
```

<br />

<a name="GRlIj"></a>
# 离线应用与客户端存储
<br />

- 离线检测
   - window.navigator.onLine
   - online 事件( window )
   - offline 事件( window )
- 应用缓存
- Cookie
   - 名称( name )
   - 值( value )
   - 失效时间( expires )
   - 路径( path )
   - 域( domain )
   - 安全标志( secure )
- Web Storage
   - storage 事件( document )
   - e.key
   - e.oldValue
   - e.newValue
   - e.url
   - length
   - key(index)
   - clear()
   - getItem(name)
   - setItem(name, value)
   - removeItem(name)
```javascript
	var cookieUtil = {
		get: function(name){
			var reg = new RegExp("(^| )" + encodeURIComponent(String(name)) + "=([^;]*)(;|$)"),
				r = document.cookie.match(reg);
			if(r) return decodeURIComponent(r[2]);
			return null;
		},
		set: function(name, value, options){
			options = options || {};
			document.cookie = (encodeURIComponent(String(name)) + "=" + encodeURIComponent(String(value))) + (options.expires ? "; expires=" + options.expires.toUTCString() : "") + (options.path ? "; path=" + options.path : "") + (options.domain ? "; domain=" + options.domain : "") + (options.secure ? "; secure" : "");
		},
		remove: function(name, options){
			options = options || {};
			this.set(name, "", {
				expire: new Date(0),
				path: options.path,
				doamin: options.domain,
				secure: options.secure
			});
		}
	};
```

<br />

<a name="AhBY7"></a>
# 最佳实践与新兴API
<br />

- 编程实践
   - 尊重对象所有权
   - 避免全局量( 命名空间 )
   - 使用常量
- 动画循环API
   - window.requestAnimationFrame(function)
   - window.cancelAnimationFrame(id)
- Web Notifications API
   - new Notification(title, options)
   - Notification.permission
      - default：用户还没有做出任何许可，因此不会弹出通知。
      - granted：用户明确同意接收通知。
      - denied：用户明确拒绝接收通知。
   - Notification.requestPermission(function)
   - show 事件
   - click 事件
   - close 事件
   - error 事件
- Page Visibility API
   - document.hidden
   - document.visibilityState
   - visibilitychange 事件
- Fullscreen API
   - someElement.requestFullscreen()
   - someElement.exitFullscreen()
   - document.fullscreenElement
   - document.fullscreenEnabled
   - fullscreenchange 事件
   - fullscreenerror 事件
- Geolocation API
   - window.navigator.geolocation.getCurrentPosition(success, error, options)
   - window.navigator.geolocation.watchPosition(success, error, options)
   - window.navigator.geolocation.clearWatch(id)
- 文件和二进制数据操作
   - Blob 对象
      - new Blob(array, {type: string})
      - blob.type
      - blob.size
      - blob.slice(start, end)
   - URL 对象
      - window.URL.createObjectURL(file)
      - window.URL.revokeObjectURL(url)
   - File API
      - ----File对象
      - file.name
      - file.type
      - file.size
      - file.lastModified
      - file.lastModifiedDate
      - ----FileReader对象
      - new FileReader()
      - reader.readAsText(file, encoding)
      - reader.readAsDataURL(file)：返回一个基于Base64编码的data-uri对象。
      - reader.readAsBinaryString(file)
      - reader.readAsArrayBuffer(file)
      - reader.abort()
      - loadstart 事件
      - progress 事件( lengthComputable, loaded, total )
      - load 事件
      - error 事件
      - abort 事件
      - loadend 事件
- Web Timing API
   - window.performance.now()
   - window.performance.getEntries()：所有HTTP请求时间统计信息
   - window.performance
- Web Workers API
   - new Worker(JSFile)
   - worker.postMessage(json)
   - worker.terminate()：关闭子线程
   - worker.onmessage
   - worker.onerror
   - importScripts(JSFile1, JSFile2, ...)
   - self.postMessage(json)
   - self.close()
   - window.navigator.serviceWorker.register(JSFile, options).then(function).catch(function)
   - window.navigator.serviceWorker.controller
   - 【注】Service worker脚本必须与页面在同一个域，且必须在HTTPs协议下运行。
```javascript
	// Web Timeing
	var t = window.performance.timing;
	var pageload = t.loadEventEnd - t.navigationStart;
	var dns = t.domainLookupEnd - t.domainLookupStart;
	var tcp = t.connectEnd - t.connectStart;
	
	// 展开循环( 针对大数据集 )
	// 假设 values.length > 0
	var iterations = Math.floor(values.length / 8),
		leftover = values.length % 8,
		i = 0;
	if(leftover > 0){
		do{
			process(values[i++]);
		}while(--leftover > 0);
	}
	do{
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
		process(values[i++]);
	}while(--iterations > 0);
	
	// 动画循环
	var start = null,
		ele = document.getElementById("someElementToAnimate");
	ele.style.position = 'absolute';
	function step(timestamp){
		if(!start) start = timestamp;
		var progress = timestamp - start;
		ele.style.left = Math.floor(progress / 10) + 'px';
		if(progress < 2000) requestAnimationFrame(step);
	}
	requestAnimationFrame(step);
	
	// 地理定位
	var options = {
		enableHighAccuracy: false,
		timeout: 5000,
		maximumAge: Infinity
	};
	function getLocSuccess(position){
		console.log(position);
	}
	function getLocError(error){
		console.log(error);
	}
	window.navigator.geolocation.getCurrentPosition(getLocSuccess, getLocError, options);
	
	// 文件操作
	document.getElementById("getFiles").addEventListener("change", function(e){
		var output = document.getElementById("output"),
			progress = document.getElementById("progress"),
			files = e.target.files,
			type = "default",
			reader = new FileReader();
		if(/image/.test(files[0].type)){
			reader.readAsDataURL(files[0]);
			type = "image";
		}else{
			reader.readAsText(files[0]);
			type = "text";
		}
		reader.onprogress = function(e){
			if(e.lengthComputable){
				progress.innerHTML = (e.loaded / e.total * 100) + " %";
			}
		};
		reader.onload = function(){
			var html = '',
			    res = reader.result;
			switch(type){
				case "image":
					html = '<img src="' + res + '" />';
					break;
				case "text":
					html = res;
					break;
			}
			output.innerHTML = html;
		};
		reader.onerror = function(){
			output.innerHTML = 'The error code is ' + reader.error.code;
		};
		/*
		var blob = files[0].slice(100, 32),
			url = window.URL.createObjectURL(files[0]),
			i = 0,
			len = files.length;
		// window.URL.revokeObjectURL(url);
		while(i < len){
			console.log(files[i].name + '\n' + files[i].type + '\n' + files[i].size + ' bytes');
			i++;
		}
		*/
	}, false);
```

<br />

<a name="Kbf27"></a>
# ECMAScript Harmony
<br />

- 函数
   - 剩余参数与分布参数
   - 默认参数值
   - 生成器
- 数组及其他结构
   - 解构赋值
- 新对象类型
   - 映射与集合
```javascript
	// 剩余参数与分布参数
	function plus(...c){
		var res = 0;
		for(var i = 0, len = c.length; i < len; i++){
			res += c[i];
		}
		return res;
	}
	var result = plus(...[1, 2, 3]);
	
	// 生成器
	function* myNumbers(){
		console.log('运行');
		for(let i = 0, len = 10; i < len; i++){
			yield i;
		}
	}
	let generator = myNumbers(), // 函数不会运行
		next = generator.next();
	while(!next.done){
		console.log(next);
		next = generator.next();
	}
	// 计算斐波那契数列
	function* fibonacci(){
		let [prev, cur] = [0, 1];
		for(;;){
			[prev, cur] = [cur, prev + cur];
			yield cur;
		}
	}
	for(var i of fibonacci()){
		if(i >= 1000) break;
		console.log(i);
	}
	
	// 解构赋值
	var a = 1,
		b = 2;
	[b, a] = [a, b];
	var [x = 100, y = 200, z = 300] = [a, b];
	var person = {"name": "k", "age": 18};
	var {"age": testAge} = person;
	
	// 映射与集合
	var map = new Map();
	map.set("name", "test");
	map.has("name");
	map.get("name");
	map.delete("name");
	var set = new Set();
	set.add("name");
	set.has("name");
	set.delete("name");
```


