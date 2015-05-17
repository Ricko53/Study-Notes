# 编写可维护性代码

### 1 可维护性代码的特点
1. 可理解性：其他人可以接手代码并理解它的意图，无需原开发人员花太多时间解释！
1. 直观性：代码中的东西一看就能明白，尽管其操作过程复杂。
1. 可适应性：代码以一种数据上的变化不要求完全重写方法。
1. 可扩展性：在代码架构上可对核心功能的扩展。
1. 可调式性：出错时，代码可以给你足够的信息来直接确定问题所在。

### 2 代码约定
##### 2.1代码可读性
1. **缩进**：在项目中统一代码缩进，更易于阅读，一种不错也很常见的缩进大小为四个空格，当然可以是其他数量。
1. **注释**：其实我们在编写一些后台代码会经常把一个功能，一个业务逻辑，一个函数的代码注释起来，但我们却在编写javascript却经常忽略这些习惯。一般而言，在js中有以下这些地方需要注释：
1. **函数和方法** ：描述其目的和参数代表，返回值等。
1. **大段的代码** ：大段代码通常是一个任务的代码，应注释描述任务。
1. **复杂的算法** ：不是所有人都会你的算法，你需要把你的算法思路简单描述下。
1. **Hack** ：因各浏览器的差异，javascript的hack用于解决什么问题应该描述清楚。
##### 2.2 变量和函数的命名
1. 变量名应该名词：如car，person
1. 函数名应以动词开始：如getName(),返回布尔类型值的函数一般以is开头，如：isEnable();
1. 只要合乎逻辑，不用担心变量或者函数名的长度，长度问题可以通过在你发布代码压缩的时候得以解决。
#####2.3 变量类型透明
1.初始化变量：通过初始化指定变量类型，但通过这种方式命名的对象无法用于函数声明中的参数
	
	var car = null; //car是对象
    var person = ''; //person是字符串
    var count = -1; //count是整数
    var found = false; //found是布尔型

2.匈牙利标记法：在变量名之前加上一个或多个字符表示数据类型。o - 对象，s - 字符串，i - 整数，f - 浮点数，b - 布尔型*

	var oCar; //car是对象
	var sPerson; //person是字符串
	var iCount; //count是整数
	var bFound; //found是布尔型

### 3 松散耦合
##### 3.1 解耦HTML/JavaScript
在javascript中也不能包含过多的HTMl
##### 3.2 解耦CSS/Javascript

##### 3.3 解耦应用逻辑和事件处理程序
每个web应用程序都有相当多的事件处理程序，监听着大量不同的事件，然而，很少能有仔细将应用逻辑从事件处理程序中分离的，如下：
    
	function checkValue(e){
       e = e || window.event;
       var target = e.target || e.srcElement;
       if(target.value.length < 5){
      		//省略一堆逻辑处理代码...
       }
    }
将上面的代码重写为：

    function checkValue(value){
	       if(value.length < 5){
	     		//省略一堆逻辑处理代码...
	       }
       }
    function handleBlur(e){
       e = e || window.event;
       var target = e.target || e.srcElement;
       checkValue(target.value); //调用应用逻辑处理函数
    }

这样写有什么好处呢？

1. 当有不同的方式触发相同的相同过程事件的时候，这时只需调用应用逻辑处理函数即可。
1. 应用逻辑处理函数可以在不添加到事件的情况下单独测试。

### 4 编程实践
#####4.1 避免全局变量
在一个js文件中，最多允许有一个全局变量，让其他对象和函数包含在其中，因为过多的全局变量会消耗大量内存

    //两个全局变量
    var name = 'jozo';
    function sayName(){
      alert('jozo');
    }
上面的代码有两个全局变量，变量name和函数sayName()，其实可以创建一个变量包含它们：

    //一个全局变量 --推荐
    var person = {
	    name:'jozo',
	    sayName : function(){
	    alert(this.name);
	    }
    }
这样一个全局变量延伸开来就是‘空间的概念’了，不会与其他功能产生冲突，有助于消除功能作用域之间的混淆

##### 4.2 避免与null进行比较

    function sortArray(values){
	    if(values != null){
	    values.sort(比较函数);
	    }
    }
上面代码中的if语句应该检测values是否是数组，但如果与null作比较的话，字符串，数字等都会通过，导致函数抛出错误。这里是数组，那么我们就应该检测其是否为数组：

    function sortArray(values){
	    if(values instanceof Array){
	    values.sort(比较函数);
	    }
    }
所于当我们遇到与null作比较的代码的时候，我们应该用下面的技术替换：

1. 如果值为引用类型，则用instanceof 操作符检查其构造函数。
1. 如果值是基本类型，则用typeof操作符检查其类型。

##### 4.3 使用常量
    function validate(value){
	    if(!value){
		    alert('不存在');
		    location.href = '/errors/invalid.php';
	    }
    }
现在，我想把‘不存在’改为‘该数据不存在！’，把跳转路径也改了，我得回到函数中找到对应的代码去修改，而每次修改逻辑代码，都有可能引入错误的风险。所以我们可以把数据单独定义为常量，与应用逻辑分离：

    var Constans = {
	    ERRORMSG: '不存在',
	    ERRORPAGE:'/errors/invalid.php'
    };
    function validate(value){
	    if(!value){
		    alert(Constans.ERRORMSG );
		    location.href = Constans.ERRORPAGE ;
    	}
    }
这样我们修改静态文本内容的时候就无需去动逻辑函数了，甚至可以把Constans单独一个文件。那么什么样的数据需要抽出来做常量呢？

1. 重复值：任何在多处用到的值。
1. 用户界面字符串：任何用于显示给用户提示信息的字符串。
1. URLs：在WEB项目中，资源路径可能经常变更。在一个公共的地方存起来，修改起来更容易！
1. 任何可能在以后会改变的值。