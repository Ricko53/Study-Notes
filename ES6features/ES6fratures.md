#ES6新特性概览
本文基于[WaYong](http://wayou.github.io/)和[lukehoban/es6features](https://github.com/lukehoban/es6features)参考了博客资料修改记录

###1. 箭头操作符
我们知道在JS中回调是经常的事，而一般回调又以匿名函数的形式出现，每次都需要写一个function，甚是繁琐。当引入箭头操作符后可以方便地写回调了

    var array = [1, 2, 3];
    //传统写法
    array.forEach(function(v, i, a) {
	    console.log(v);
	    });
    //ES6写法
    array.forEach(v = > console.log(v));
###2. 类的支持
ES6中添加了对类的支持，引入了class关键字（其实class在JavaScript中一直是保留字，目的就是考虑到可能在以后的新版本中会用到，现在终于派上用场了）。JS本身就是面向对象的，ES6中提供的类实际上只是JS原型模式的包装。现在提供原生的class支持后，对象的创建，继承更加直观了，并且父类方法的调用，实例化，静态方法和构造函数等概念都更加形象化

	//类的定义
	class Animal {
		//ES6中新型构造器
	    constructor(name) {
	        this.name = name;
	    }
	    //实例方法
	    sayName() {
	        console.log('My name is '+this.name);
	    }
	}
	//类的继承
	class Programmer extends Animal {
	    constructor(name) {
	    	//直接调用父类构造器进行初始化
	        super(name);
	    }
	    program() {
	        console.log("I'm coding...");
	    }
	}
	//测试我们的类
	var animal=new Animal('dummy'),
	wayou=new Programmer('wayou');
	animal.sayName();//输出 ‘My name is dummy’
	wayou.sayName();//输出 ‘My name is wayou’
	wayou.program();//输出 ‘I'm coding...’

###3. 增强的对象字面量
对象字面量被增强了，写法更加简洁与灵活，同时在定义对象的时候能够做的事情更多了。具体表现在：

- 可以在对象字面量里面定义原型
- 定义方法可以不用function关键字
- 直接调用父类方法

**example**

	//通过对象字面量创建对象
	var human = {
		breathe() {
			console.log('breathing...');
		}
	};
	var worker = {
		__proto__: human, //设置此对象的原型为human,相当于继承human
		company: 'freelancer',
		work() {
			console.log('working...');
		}
	};
	human.breathe();//输出 ‘breathing...’
	//调用继承来的breathe方法
	worker.breathe();//输出 ‘breathing...’

###4. 字符串模板

	//产生一个随机数
	var num=Math.random();
	//将这个数字输出到console
	console.log(`your num is ${num}`);

###5. 结构
自动解析数组或对象中的值。比如若一个函数要返回多个值，常规的做法是返回一个对象，将每个值做为这个对象的属性返回。但在ES6中，利用解构这一特性，可以直接返回一个数组，然后数组中的值会自动被解析到对应接收该值的变量中

	var [x,y]=getVal(),//函数返回值的解构
    [name,,age]=['wayou','male','secrect'];//数组解构

	function getVal() {
	    return [ 1, 2 ];
	}
	
	console.log('x:'+x+', y:'+y);//输出：x:1, y:2 
	console.log('name:'+name+', age:'+age);//输出： name:wayou, age:secrect 

###6. 参数默认值，不定参数，拓展参数
**默认参数值**

    function sayHello(name){
    	//传统的指定默认参数的方式
    	var name=name||'dude';
    	console.log('Hello '+name);
    }
    //运用ES6的默认参数
    function sayHello2(name='dude'){
    	console.log(`Hello ${name}`);
    }
    sayHello();//输出：Hello dude
    sayHello('Wayou');//输出：Hello Wayou
    sayHello2();//输出：Hello dude
    sayHello2('Wayou');//输出：Hello Wayou

**不定参数**

	//将所有参数相加的函数
	function add(...x){
		return x.reduce((m,n)=>m+n);
	}
	//传递任意个数的参数
	console.log(add(1,2,3));//输出：6
	console.log(add(1,2,3,4,5));//输出：15

**拓展参数**

	var people=['Wayou','John','Sherlock'];
	//sayHello函数本来接收三个单独的参数人妖，人二和人三
	function sayHello(people1,people2,people3){
		console.log(`Hello ${people1},${people2},${people3}`);
	}
	//但是我们将一个数组以拓展参数的形式传递，它能很好地映射到每个单独的参数
	sayHello(...people);//输出：Hello Wayou,John,Sherlock 
	
	//而在以前，如果需要传递数组当参数，我们需要使用函数的apply方法
	sayHello.apply(null,people);//输出：Hello Wayou,John,Sherlock 

###7. let与const 关键字
可以把let看成var，只是它定义的变量被限定在了特定范围内才能使用，而离开这个范围则无效

	for (let i=0;i<2;i++)console.log(i);//输出: 0,1
	console.log(i);//输出：undefined,严格模式下会报错

###8. for of 值遍历

	var someArray = [ "a", "b", "c" ];
 
	for (v of someArray) {
	    console.log(v);//输出 a,b,c
	}

###9. 模块
在ES6标准中，JavaScript原生支持module了。这种将JS代码分割成不同功能的小块进行模块化的概念是在一些三方规范中流行起来的，比如CommonJS和AMD模式

	// point.js
	module "point" {
	    export class Point {
	        constructor (x, y) {
	            public x = x;
	            public y = y;
	        }
	    }
	}
	 
	// myapp.js
	//声明引用的模块
	module point from "/point.js";
	//这里可以看出，尽管声明了引用的模块，还是可以通过指定需要的部分进行导入
	import Point from "point";
	 
	var origin = new Point(0, 0);
	console.log(origin);

###10. Map，Set 和 WeakMap，WeakSet
	
	// Sets
	var s = new Set();
	s.add("hello").add("goodbye").add("hello");
	s.size === 2;
	s.has("hello") === true;
	
	// Maps
	var m = new Map();
	m.set("hello", 42);
	m.set(s, 34);
	m.get(s) == 34;

有时候我们会把对象作为一个对象的键用来存放属性值，普通集合类型比如简单对象会阻止垃圾回收器对这些作为属性键存在的对象的回收，有造成内存泄漏的危险。而WeakMap,WeakSet则更加安全些，这些作为属性键的对象如果没有别的变量在引用它们，则会被回收释放掉

	// Weak Maps
	var wm = new WeakMap();
	wm.set(s, { extra: 42 });
	wm.size === undefined
	
	// Weak Sets
	var ws = new WeakSet();
	ws.add({ data: 42 });//因为添加到ws的这个临时对象没有其他变量引用它，所以ws不会保存它的值，也就是说这次添加其实没有意思

###11. Proxies
Proxy可以监听对象身上发生了什么事情，并在这些事情发生后执行一些相应的操作。一下子让我们对一个对象有了很强的追踪能力，同时在数据绑定方面也很有用处
	
	//定义被侦听的目标对象
	var engineer = { name: 'Joe Sixpack', salary: 50 };
	//定义处理程序
	var interceptor = {
	  set: function (receiver, property, value) {
	    console.log(property, 'is changed to', value);
	    receiver[property] = value;
	  }
	};
	//创建代理以进行侦听
	engineer = Proxy(engineer, interceptor);
	//做一些改动来触发代理
	engineer.salary = 60;//控制台输出：salary is changed to 60

###12. Symbols
我们知道对象其实是键值对的集合，而键通常来说是字符串。而现在除了字符串外，我们还可以用symbol这种值来做为对象的键。Symbol是一种基本类型，像数字，字符串还有布尔一样，它不是一个对象。Symbol 通过调用symbol函数产生，它接收一个可选的名字参数，该函数返回的symbol是唯一的。之后就可以用这个返回值做为对象的键了。Symbol还可以用来创建私有属性，外部无法直接访问由symbol做为键的属性值

	(function() {

	  // 创建symbol
	  var key = Symbol("key");
	
	  function MyClass(privateData) {
	    this[key] = privateData;
	  }
	
	  MyClass.prototype = {
	    doStuff: function() {
	      ... this[key] ...
	    }
	  };
	
	})();
	
	var c = new MyClass("hello")
	c["key"] === undefined//无法访问该属性，因为是私有的

###13. Math，Number，String，Object 的新API
	
	Number.EPSILON
	Number.isInteger(Infinity) // false
	Number.isNaN("NaN") // false
	
	Math.acosh(3) // 1.762747174039086
	Math.hypot(3, 4) // 5
	Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2
	
	"abcde".contains("cd") // true
	"abc".repeat(3) // "abcabcabc"
	
	Array.from(document.querySelectorAll('*')) // Returns a real Array
	Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
	[0, 0, 0].fill(7, 1) // [0,7,7]
	[1,2,3].findIndex(x => x == 2) // 1
	["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
	["a", "b", "c"].keys() // iterator 0, 1, 2
	["a", "b", "c"].values() // iterator "a", "b", "c"
	
	Object.assign(Point, { origin: new Point(0,0) })

###14. Promises
Promises是处理异步操作的一种模式，之前在很多三方库中有实现，比如jQuery的deferred 对象。当你发起一个异步请求，并绑定了.when(), .done()等事件处理程序时，其实就是在应用promise模式

	//创建promise
	var promise = new Promise(function(resolve, reject) {
	    // 进行一些异步或耗时操作
	    if ( /*如果成功 */ ) {
	        resolve("Stuff worked!");
	    } else {
	        reject(Error("It broke"));
	    }
	});
	//绑定处理程序
	promise.then(function(result) {
		//promise成功的话会执行这里
	    console.log(result); // "Stuff worked!"
	}, function(err) {
		//promise失败会执行这里
	    console.log(err); // Error: "It broke"
	});

