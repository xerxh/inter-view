### 面试题
#### 一面 / 二面

准备充分 知识系统 沟通简介 内心诚实 态度谦虚 回答灵活

###### 页面布局
1. 假设高度已知,请写出三栏布局，其中左栏,右拦宽度各为300px,中间自适应。
```
<section class="layout">
    <article left-right-center>
        <div class="left"></div>
        <div class="right"></div>
        <div class="center">
            这是自适应中间部分
        </div>
    </article>
</section>
.layout article div{ // 高度固定
    min-height: 200px;
}

// 浮动
// 优缺点：浮动脱离文档流，需要清除浮动，兼容性比较好
.layout .left{
    float: left;
    width: 300px;
    background: red;
}
.layout .right{
    float: right;
    width: 300px;
    background: blue;
}
.layout .center{
    background: yellow;
}

// 绝对定位
// 优缺点：布局快捷，很少出现问题，布局脱离文档流，下方的元素也需要脱离文档流，否则会被覆盖在下面，可使用性比较差
.layout article > div{ // 设置容器绝对定位
    position: absoult;
}
.layout .left{
    left: 0;
    width: 300px;
    background: red;
}
.layout .right{
    right: 0;
    width: 300px;
    background: blue;
}
.layout .center{
    right: 300xp;
    left: 300px;
    background: yellow;
}

// flexbox
// 优缺点：现在主流布局，兼容性比较差，比较新的标准
.layout article{
    display: flex;
}
.left{
    width: 300px;
    background: red;
}
.right{
    background: blue;
    width: 300px;
}
.center{
    flex: 1;
    background: yellow;
}

// table布局
// 优缺点：表格布局的兼容性很好，当其中的某一个单元格高度超出的时候其他二个单元格也会超出，有些场景不需要进行超出
.layout article{
    width: 100%;
    display: table;
    height: 100px;
}
.layout article > div{ // 给所有元素设置table-cell
    display: table-cell;
}
.layout .left{
    width: 300px;
    background: red;
}
.layout .right{
    width: 300px;
    background: blue;
}
.layout .center{
    background: yellow;
}

//网格布局 grid  css3的下一代标准
// 优缺点：新技术，很灵活，布局简单
.layout article{
    display: grid;
    width: 100%;
    grid-template-rows: 100px;
    grid-template-columns: 300px auto 300px;
}
.layout .left{
    background: red;
}
.layout .right{
    backgroungd: blue;
}
.layout .center{
    background: yellow;
}
```
2. 如果去掉高度已知
    在不改动的情况下,flex布局和table布局是可以适用的
浮动布局,绝对定位布局,网格布局需要进行调整
3. 三栏布局
    左右宽度固定,中间自适应
    上下高度固定,中间自适应
4. 二栏布局
    左宽度固定,右自适应
    右宽度固定,左自适应
    上高度固定,下自适应
    下高度固定,上自适应
###### css盒模型
1. 谈谈你对css盒模型的认识
基本概念：标准模型+IE模型
标准模型和IE模型的区别：
    1. 计算高度和宽度的不同
    2. IE模型宽和高会把 margin padding 计算在内
    3. 标准模型的宽度和高度只计算内容 content 的宽高
2. css如何设置这两种模型
    1. 修改 box-sizing属性
    2. box-sizing: content-box; 标准盒模型  浏览器默认为标准盒模型
    3. box-sizing: border-box; IE盒模型
3. JS如何设置获取盒模型对应的宽和高
    1. dom.style.width/height 只能取到内联样式的宽和高
    2. dom.currentStyle.width/height 获取渲染完成后的计算属性宽和高(有兼容性问题，只有ie支持)
    3. window.getComputedStyle(dom).width/height 同样是获取渲染完成后的计算属性宽和高(兼容性更好，基本都适用)
    4. dom.getBoundingClientRect().width/height 可以获取dom元素的根据视窗计算的绝对位置,可以获取到top left width height
4. 根据盒模型解释边距重叠
    块级元素内嵌一个块级元素，子元素高度设定为 100px, 子元素的 margin-top 10px
    父元素高度为100px 距离上部 10px
    给定属性 overflow: hidden 创建了BFC(格式化上下文) 父元素高度变为 110px
5. BFC(边距重叠解决方案)
    ###### 基本概念：
        Block level 的box会参与形成BFC，比如display值为block，list-item,table的元素。
        一个块格式化上下文（block formatting context）它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用
    ###### BFC的原理：
        1. 内部的盒子会在垂直方向，一个个地放置；
        2. 盒子垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的上下margin会发生重叠；
        3. 每个元素的左边，与包含的盒子的左边相接触，即使存在浮动也是如此；
        4. BFC的区域不会与float重叠；
        5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此；
        6. 计算BFC的高度时，浮动元素也参与计算。
    ###### 如何创建BFC：
        1. float的属性不为none；
        2. position为absolute或fixed；
        3. display为inline-block，table-cell，4. table-caption，flex；
        5. overflow不为visible
    ###### BFC的适用场景：
        1. BFC两栏布局 
            假设左边高度为 100px 右边高度为 200px 会发现右边溢出了 溢出的部分与左边接触 满足第三条
            解决创建BFC利用第四条，BFC的区域不会与float区域重叠，给右边元素创建BFC
        2. 清除内部浮动 
            计算BFC的高度时,浮动元素也会参与计算
        3. 消除margin边距重叠
            属于同一个BFC的相邻box的上下margin会发生重叠
            因此将上下两个发生重叠的元素中的一个创建BFC就不会发生重叠了，因为第五条：BFC就是页面上的一个隔离的容器，容器里面的子元素不会影响到外面的元素，反之也如此
    ##### IFC
        Inline-level的box会参与形成IFC，比如display为inline,inline-table,inline-block的元素。
        行级格式化上下文
        1. 框会从包含快的顶部开始，然后一个接一个的排列。
        2. 在放置这些框的时候，他们在水平方向上的外边距，边框和内边距所占的空间都会被考虑在内（这样就得到了一个一个的框）。然而在垂直方向上，这些框可能会以不同的方式来对齐，比如他们可以顶部对齐，或者底部对齐也有可能按照文本的基线对齐。而把这一行上的框都包含进去的一个大框我们称之为改行的行框。水平的padding,border,margin有效，竖直方向则无效。不能指定框高。
        行宽的宽度由包含块和存在的浮动决定的。行框的高度至少会高到足以包含他内部的所有框。
        3. 当一行上的行级总宽度（某一个小框的宽度或者若干个小框的总宽度）小于行宽的时候，他们在行宽内的水平方向上的排布由text-align决定。
        4. 当一个行内框的宽度超过了该行的行宽的时候，就会被分成几个框。(ex.文字换行的时候 字都不在同一行了，那换行的时候自然就会多一份框，自然也就多了一份行宽)但是如果设置这个框就不能被分割的话（比如，文字强制不给换行white-space设置为nowrap）那么这时候该行内框就会溢出该行的行宽。
        5. 一般情况下行宽的左边紧贴在他的包含块的左边，同样他的右边也是紧贴在其包含块的右边。但是也不一定，比如出现浮动的话，浮动元素可能会插在包含块和行框之间。所以一般在同一个IFC中行框通常有相同的宽度（包含快的宽度）但是某一行的行宽的宽度也可能受浮动元素影响，减少了水平可用的宽度了。在同一个IFC中，行框的高度通常是变化的，不一定的，比如某一行的某个框是个很高的图片，而改行框中其他框只是文字。
        6. 计算行框内各个框的高度，对于非替换元素就是起line-height,而对于替换元素就是边界框的高度了。 行框的高是最顶端框的顶边到最底端框的底边的距离。
    ##### 影响IFC的css样式
        font-size
        line-height
        height
        vertical-align
    ##### GFC网格布局格式化上下文
        1. 还有另一个布局是GFC（GridLayout Formatting Contexts）布局，即网格布局格式化上下文，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。 

        2. 那么GFC有什么用呢，和table又有什么区别呢？首先同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。这个布局我感觉不怎么经常用到，所以就提出来，大家知道有这个东西就好了。
    ##### FFC自适应格式化上下文
        1. FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container），可惜这个牛逼的属性只有谷歌和火狐支持，不过在移动端也足够了，至少safari和chrome还是OK的，毕竟这俩在移动端才是王道。
        2. Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。
        伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局。
###### Dom事件
    1. Dom事件的级别
        Dom0 element.onclick = function () {}
        Dom2 element.addEventListener('click', function(){}, false)  true 捕获 false 冒泡
        Dom3 element.addEventListener('keyup', function(){}, false)  添加了新的事件类型 如鼠标事件 键盘事件
    2. Dom事件模型
        捕获和冒泡
    3. Dom事件流
        捕获  -> 目标阶段 -> 冒泡
    4. 描述Dom时间捕获的具体流程
        window -> document -> html标签 -> body -> ... ->  目标元素(IE10以下没有捕获阶段)
        目标元素 -> ... -> body -> html -> document -> window
    5. Event对象的常见应用
        event.preventDefault()
        event.stopPropagation()  阻止冒泡
        绑定了两个同样的事件A / B , 可以在A中使用API阻止B事件的触发 (事件的优先级)
        event.stopImmediatePropagation() 
        event.currentTarget 获取事件委托真正点击的目标元素
        event.target
    6. 自定义事件
        event = new Event(typeArg, eventInit);
        1. typeArg：指定事件类型，传递一个字符串。这里的事件类型指的是像点击事件（click）、提交事件（submit）、加载事件（load）等等。
　　    2. eventInit：可选，传递EventInit类型的字典。实际上这个EventInit类型的字典也就是我们使用InitEvent()时需要传递的参数，以键值对的形式传递，不过它可以多选一个参数：
　　　　  1. bubbles：事件是否支持冒泡，传递一个boolean类型的参数，默认值为false。
　　　　  2. cancelable：是否可取消事件的默认行为，传递一个boolean类型的参数，默认值为false。
　　　　  3. composed：事件是否会触发shadow DOM（阴影DOM）根节点之外的事件监听器，传递一个boolean类型的参数，默认值为false。
        不足：
            只能指定事件名称,不能指定数据。
            CustomEvent除了指定事件名称可以跟一个object作为指定参数。
```
    // event
    var eve = new Event('custom')          
    ev.addEventListener('custom', funtion(){
        console.log('custom')
    })
    ev.dispatchEvent(eve)
    /* 创建一个事件对象，名字为newEvent，类型为build */
    var newEvent = new CustomEvent('build',{ 
        detail: {
            dog:"wo",cat:"mio"
        }
    });
    /* 将自定义事件绑定在document对象上 */
    document.addEventListener("build",function(){
        alert(" event.detail.dog:" + event.detail.dog
            + "\n event.detail.cat:" + event.detail.cat );
    },false) 
    /* 触发自定义事件 */
    document.dispatchEvent(newEvent);  
```
###### 类型转换
    1. 数据类型
        最新的ECMAScript标准定义了7种数据类型
        原始类型：
            Boolean Null Undefined Number String Symbol
        复合类型：
            Object
    2. 显式类型转换
        Number 函数
            原始类型转换：
                数值：转换后还是原来的值
                字符串：如果可以解析为数值,则转换为对应的数值,否则得到NaN,空字符串转换为0
                布尔值：true -> 1 false -> 0
                undefiend: 转换为NaN
                null: 转换成0
            对象类型转换：
                先调用对象自身的valueOf方法,如果该方法返回原始类型的值(数值,字符串,布尔值),则直接对该值适用Number方法,不再进行后续步骤。
                如果valueOf返回复合类型的值,再调用对象自身的toString方法,如果toString方法返回原始类型的值,则对该值适用Number方法,不再进行后续步骤。
                如果toString方法返回的是复合类型的值,则报错。
        String 函数
            原始类型：
                数值：转为相应的字符串
                字符串： 还是原来的值
                布尔值：true转为'true' false转为'false'
                undefined: 'undefined'
                null: 'null'
            对象类型转换：
                先调用toString方法,如果toString方法返回的是原始类型的值,则对该值适用String方法,不再进行下列步骤。
                如果toString方法返回的是复合类型的值,再调用valueOf方法,如果valueOf方法返回的是原始类型的值,则对该值适用toString方法,不再进行以下步骤。
                如果valueOf方法返回的是复合类型的值则会报错。
        Boolean 函数
            原始类型转换：
                undefined
                null
                -0          => false 其他都为 true
                +0
                NaN
                ''
    3. 隐式类型转换
        会发生隐式类型转换的行为
            四则运算
            判断语句
            Native调用(cosole / alert)
    4. [] + []
       [] + {}
       {} + []
       {} + {}
       true + true
       1 + { a:1 }
    5. typeOf
        Undefined => "undefined"
        Null      => "object"
        Boolean   => "boolean"
        Number    => "number"
        String    => "string"
        Symbol    => "symbol"
        Function  => "function"
        object    => "object"
###### Http协议
    1. http协议的主要特点
        简单快速 灵活  无连接 无状态
    2. http报文的组成部分
        请求报文
            请求行 请求头 空行 请求体
            请求行 HTTP方法 请求地址 http版本
            请求行 key -> value键值对 告诉服务端需要的信息
            请求体 数据部分
        响应报文
            状态行 响应头 空行 响应体
    3. http方法
        GET  ->  获取资源
        POST ->  传输资源
        PUT  ->  更新资源
        DELETE -> 删除资源
        HEAD ->  获取报文首部
    4. POST和GET的区别
        GET在浏览器回退是无害的,而POST会再次提交请求
        GET产生的URL地址可以被收藏,而POST不可以
        GET请求会被浏览器主动缓存,而POST不会,除非手动设置
        GET只能进行URL编码,而POST支持多种编码方式
        GET请求的参数会被完整保留在浏览器历史记录里,而POST中的参数不会保留
        GET在URL种传递的参数会收到长度限制,而POST参数放在请求体种没有长度限制
        GET比POST相对不安全,因为参数直接暴露在URL上
        GET参数通过URL传递,而POST放在请求体中
    5. HTTP状态码
        1XX 指示信息-表示请求已经接收,正在继续处理
        2XX 成功-表示请求已经被成功接收并响应
        3XX 重定向-表示要完成请求需要进行进一步操作
        4XX 客户端错误-请求有语法错误或者请求无法实现
        5XX 服务器错误-服务器未能实现合法请求
        200 客户端请求成功
        301 永久重定向
        302 临时重定向
        304 客户端有缓存文档并发出了一个条件性的请求，服务器告诉客户,原来的缓存还可以继续使用
        400 客户端请求有语法错误，不能被服务器理解
        401 请求未经授权，这个状态码必须和WWW-Authentication报头域一起使用
        403 对请求页面的访问被禁止
        404 请求资源不存在
        500 服务器发生了错误原来的缓存文档还可以继续使用
        503 请求未完成,服务器临时过载或当机,一段时间后可能恢复正常
    6. 什么是持久化连接
        HTTP协议采用 请求-应答模式，当使用普通模式,即非keep-Alive模式时,每个请求/应答客户端和服务器端都要重新建立一个连接，完成后立即会断开连接
        当使用keep-alive模式,keep-alive功能使客户端到服务器端的连接持续有效,当出现对服务器的后续请求时，避免了重新建立请求，提升了性能
    7. 什么是管线化
        在使用持久连接的情况下，某个连接上的消息传递类似于
            请求1 -> 响应1 -> 请求2 -> 响应2 -> ....
        当使用管线化,连接上的消息变成  请求打包一次进行传输 -> 响应打包一次返回
            请求1 -> 请求2 -> 响应1 -> 响应2
        管线化机制通过持久化连接完成,仅HTTP/1.1支持此技术
        只有GET和HEAD请求可以进行管线化,而POST会有限制
        初次建立连接时不应该启动管线化,因为服务器不一定支持HTTP/1.1版本的协议
        管线化不会影响到来的顺序
        HTTP/1.1要求服务器支持管线化,但并不要求服务器也对响应进行管线化处理，只是要求管线化的请求不失败就可以
        由于上面提到服务器问题,开启管线化可能不会带来大幅度的性能提升,并且很多服务器和代理程序对管线化的支持并不好,因此现在浏览器如Chrome 和 Firefox 默认不开启管线化支持
###### 面向对象
    1. 类的声明和实例
```
function Animal (name) {
    this.name = name
}
class Animal2 {
    construtor (name) {
        this.name = name 
    }
}
console.log(new Animal('o1'), new Animal('o2'))
```
    2. 如何实现继承，继承的几种方式
```
// 借助构造函数实现继承
// 缺点：只继承了构造函数的属性 无法继承原型对象上属性和方法
function Parent1 () {
    this.name = 'parent1'
}
Parent1.prototype.say = function () {
    console.log('say')
}
function Child1 () {
    Parent1.call(this) // apply
    this.type = 'child1'
}

// 借助原型链实现继承
// 缺点：如果child2new多个实例,所有实例都会公用一个父类属性,改变一个所有的都改变没有实现属性隔离
function Parent2 () {
    this.name = 'parent2'
}
Parent2.prototype.say = function () {
    console.log('say2')
}
function Child2 () {
    this.type = 'child2'
}
child2.prototype = new Parent2() 

// 组合方式实现继承  （写继承最通用的方式）
// 组合两种实现了属性隔离
// 缺点：父集构造函数执行了2次 没有必要
function Parent3 () {
    this.name = 'parent3'
}
function Child3 () {
    Parent3.call(this)
    this.type = 'child3'
}
Child3.prototype = new Parent3()

// 组合继承优化1
// 没有执行2次
// 缺点：判断实例的真正构造函数会出现问题
function Parent4 () {
    this.name = 'parent4'
}
function Child4 () {
    Parent4.call(this)
    this.type = 'child4'
}
Child4.prototype = Parent4.prototype // 只进行了引用

// 组合继承优化2
// 完美写法
function Parent5 () {
    this.name = 'parent5'
}
function Child5 () {
    Parent5.call(this)
    this.type = 'child5'
}
Child5.prototype = Object.create(Parent5.prototype) // 只进行了引用
Child5.construtor = Child5

// Es6 Class
class Person{  
    constructor(name, age) {
        this.name = name
        this.age = age
    }

    sayName(){
        console.log("the name is:"+this.name)
    }
}
 
class Worker extends Person{
    constructor(name, age,job) {
            super(name, age)
            this.job = job
    }
    sayJob(){
        console.log("the job is:"+this.job)
    }
}

var worker = new Worker('tcy',20,'teacher')
worker.sayJob() //the job is:teacher
worker.sayName() //the name is:tcy
```
    4. Object.create
```
Object.create =  function (o) {
    var F = function () {}
    F.prototype = o
    return new F()
}
```
###### 原型链
    1. 创建对象有几种方法
```
## 1
    var o1 = {name: 'o1'}
    var o2 = new Object({name: 'o2'})
## 2
    var obj = function(name){
        this.name = name
    }
    var o1 = new obj('o1')
## 3
    var p = {name: 'o1'}
    var o1 = Object.create(p)
```
    2. 原型对象，构造函数，实例，原型链
        每一个函数在声明时js都会生成一个对应的原型对象,函数通过prototype属性指向对应的原型对象
        原型对象则通过constructor来指向对应的构造函数
        构造函数可以通过new实例化生成一个实例对象,实例对象上有一个属性__proto__指向对应的原型对象
        而原型对象也是一个对象也有自己的__proto__指向上一级别的原型对象,这样逐级一直往上一直到null,当查找一个属性或者方法时会一直向上查找这就叫原型链。
    3. instanceof的原理
        instanceof 用来判断 实例对象的__proto__和构造函数的prototype是否是同一个引用  判断是否是构造函数的实例
        由于原型链的逐级查找,只要是原型链上满足，就会返回true,因此不能严格判断是否是构造函数的实例
        严格判断需要使用constructor属性来进行判断
    4. new运算符
        1. 一个新的对象被创建,它继承foo.prototype
        2. 构造函数foo被执行。执行的时候,对应的参数会被传入,同时上下文(this)指向这个新实例。new foo 等同于 new foo(),只能用在不传递任何参数的情况
        3. 如果构造函数返回了一个"对象",那么这个对象会取代整个new出来的结果,如果构造函数没有返回对象,那么new出来的结果为 1创建的对象
```
# new 的原理
var new2 = function (func) {
    var o = Object.create(func.prototype)
    var k = func.call(o)
    if(typeof k === 'object'){
        return k
    }else{
        return o
    }
}
```
###### 通信
    1. 什么是同源策略和限制
        同源策略限制了从一个源加载的文档或者脚本如何与来自另一个源的资源进行交互。
        这是一个隔离潜在恶意文件的关键安全机制。
        同源：协议,域名,端口相同
        Cookie, localStorage 和 IndexDB 无法读取
        DOM 无法获得
        AJAX 无法发送
    2. 前后端如何通信
        AJAX
        webSocket
        CORS
    3. 如何创建ajax
        XMLHttpRequest对象的工作流程
        兼容性处理
        事件的触发条件
        事件的触发顺序
```
function ajax (opt) {
    if(opt.url) {
        var xhr = XMLHttpRequest ? new XMLHttpRequest() : new Window.ActiveXObject('Microsoft.XMLHTTP')
        var data = opt.data
            url = opt.url
            type = opt.type.toUpperCase()
            dataArr = []
        for (var k in data) {
            dataArr.push(k + "=" + data[k])
        }
        if (type == 'GET') {
            url = url + '?' + dataArr.join("&")
            xhr.open(type, url, true)
            xhr.send()
        }
        if (type == 'POST') {
            xhr.open(type, url, true)
            xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
            xhr.send(dataArr.join('&'))
        }
        xhr.onload = function () {
            if (xhr.status === 200 || xhr.status === 304) {
                var res
                if (opt.success && opt.success instanceof Function) {
                    res = xhr.responseText
                    if (typeof res == 'string') {
                        res = JSON.parse(res)
                        opt.success.call(xhr, res)
                    }
                } else {
                    if (opt.error && opt.error instanceof Function) {
                        opt.error.call(xhr, res)
                    }
                }
            }
        }
    }
}
ajax({
    url: '',
    type: 'get',
    success: function() {},
    error: function() {}
})
```
    4. 跨域通信的几种方式
        JSONP
            通过动态添加script标签 加载对应的代码块进行执行
```
<script src="http:www.baidu.com?data=name&callback=jsonp"></script>
// 服务器返回
    callback('数据')
```
        Hash  // hash 改变不触发页面更新 所以可以用来进行跨域通信
```
// 场景：当前页面 A 通过 iframe 嵌入了跨域的页面 B
// A中伪代码
    var B = document.getElementByTagName('iframe')
    B.src = B.src + '#' + 'data'
// B中伪代码
    window.onhashchange = function () {
        var data = window.location.hash
    }
```
        postMessage
```
// 窗口A 向 B窗口发送消息
// A窗口
    window.postMessage('data', 'http://B.com') // B窗口的window对象
// 在B 窗口监听
    window.addEventListener('message', function (event) {
        console.log(event.origin) // http://A.com
        console.log(event.source) // Awindow
        console.log(event.data) // 'data'
    }, false)
```
        WebScocket
```
var ws = new WebSocket('wss://ech.websocke.org')
ws.open = function (evt) {
    ws.send("hello websocket")
}
ws.onmessage = function (evt) {
    console.log(evt.data)
    ws.close()
}
wx.onclose = function (evt) {
    console.log('close')
}
```
        CORS
            跨域资源共享 浏览器会拦截AJAX请求 如果是跨域会在请求头中加上origin
###### web安全
        1. CSRF
            基本概念：跨站请求伪造
            攻击原理：用户登陆网站A获取了身份cookie,用户之后又访问了一个网站B,在B中又引诱点击指向A的一个接口,对A网站请求时会自动在请求头中携带cookie
            防御措施
                Token 验证
                Referer 验证 检验来源
                隐藏令牌 在url中携带验证
        2. XSS
            基本概念：跨站脚本攻击
            攻击原理：利用评论或者输入区向页面注入JS脚本代码
            防御措施：使注入脚本不能执行
###### 算法
        1. 排序
            快速排序
            选择排序
            希尔排序
            冒泡排序
        2. 堆栈,队列,链表
            堆栈
            队列
            链表
        3. 递归
###### 浏览器渲染机制
        1. 什么是 DOCTYPE 及其作用
            DTD 用来定义XML或者(X)HTML的文件类型。浏览器会根据它来判断文档类型,决定使用何种协议来解析以及切换浏览器模式
            DOTYPE是用来声明文档类型和DTD规范的,一个主要的用途就是验证文件的合法性。如果文件代码不合法,那么浏览器解析就会出现一些错误。（指定DTD那个类型）
            <!DOTYPE html>  html5的DTD规范
            4.0 版本有两个DTD模式  
                一个是严格模式 该DTD包含所有的 HTML 元素和属性,但不包括展示性和弃用的元素 
                一个是传统模式 该DTD包含所有的 HTML 元素和属性,包括展示性和弃用元素
        2. 浏览器的渲染过程
            浏览器获取到 HTML 和 CSS,根据 HTML 通过 HTML 解析器 生成 DOM 树,根据 CSS 通过 CSS 解析器生成 CSS 规则树,
            二者完成后会整合形成 Render 树,此时形成了整个 html 的结构还没有完成位置的布局，因此在绘制整个页面之前会先进行一次 Layout 获取最终的 Dom 宽高 位置 背景  颜色等属性并合并到 Render 树中,然后通过GUI进行绘制整个页面,绘制完成后显示给用户
        3. 重排Reflow
            重排一定会引起浏览器的重绘
            如果变化涉及元素尺寸重新计算 , 浏览器则抛弃原有属性, 重新计算并把结果传递给 render 以重新描绘页面元素, 此过程称为 重排reflow。
            引起DOM树重新计算的行为
            触发Reflow的行为:
                增加,删除,修改 DOM 元素节点,会导致 Reflow 或者 Repaint
                移动 DOM 元素的位置,或者实现动画的时候
                修改 CSS 样式的时候
                当修改 Resize 宽口的时候(移动端没有此问题),或者是滚动的时候
                当修改网页的默认字体时
            1.分离读写操作,如
                var curLeft=div.offsetLeft;
                var curTop=div.offsetTop;
                div.style.left=curLeft+1+'px';
                div.style.top=curTop+1+'px';
            2. 将多次改变样式属性的操作合并成一次操作

            3.将需要多次重排的元素，position属性设为absolute或fixed，这样此元素就脱离了文档流，它的变化不会影响到其他元素。例如有动画效果的元素就最好设置为绝对定位。

            4. 由于display属性为none的元素不在渲染树中，对隐藏的元素操作不会引发其他元素的重排。如果要对一个元素进行复杂的操作时，可以先隐藏它，操作完成后再显示。这样只在隐藏和显示时触发2次重排。

            5.在内存中多次操作节点，完成后再添加到文档中去。例如要异步获取表格数据，渲染到页面。可以先取得数据后在内存中构建整个表格的html片段，再一次性添加到文档中去，而不是循环添加每一行。

            6. 在需要经常取那些引起浏览器重排的属性值时，要缓存到变量，如窗口的offsetTop、offsetLeft事先缓存
        4. 重绘Repaint
            重绘不会带来重新布局，并不一定伴随重排
            当 DOM 元素的属性发生变化 (如 color) 时, 浏览器会通知 render 重新描绘相应的元素, 此过程称为 重绘repaint
            触发Repaint的行为
            一个元素外观的改变（如color）所触发的浏览器行为
                DOM改动
                CSS改动
###### JS运行机制
        如何理解 JS 单线程
        异步任务
            定时器 Dom 事件 es6 promise
        什么是任务队列
        什么是事件循环
###### 页面性能
        1. 提升页面性能的方法有哪些？
            资源压缩合并,减少http请求
            非核心代码进行异步加载(会在同步加载之后执行)
```
<script src="defer.js" defer></script>
<script src="async" async></script>
```
                异步加载的方式
                    1.动态脚本加载
                    2.defer
                    3.async
                异步加载的区别
                    1.defer是在HTML解析完成后才会执行,如果是多个会按照加载的顺序依次执行
                    2.async是在加载完成之后立即执行,如果是多个,执行顺序只和返回顺序有关与加载顺序无关
            利用浏览器进行缓存
                缓存的分类 
                    1.强缓存
                        Cache-Control > Expires 优先级
                        Expires  过期时间   服务器的绝对时间
                            Expires：Thu, 21 Jan 2017 23:59:02 GMT
                        Cache-Control  过期时间 相对事件MS
                            Cache-Contorl:max-age=3600
                    2.协商缓存
                        Last-Modified 服务器下发携带时间
                        If-Modified-Since   浏览器请求协商缓存是否有效携带上次服务器下发时间回去进行判断
                        Etag 服务器下发携带资源的hash值
                        If-None-Match 浏览器请求协商缓存是否有效携带上次服务器下发hash回去进行判断缓存是否生效
            使用CDN
            预解析DNS
```
<meta http-equiv="x-dns-prefetch-control"  content="on"/> // 强制打开所有A标签的预解析 HTTPS默认关闭预解析

<link rel="dns-prefetch" href="//host_name_to_prefetch.com">
```
###### 错误监控
        1. 前端错误的分类
            即时运行错误：代码错误
            资源加载错误
        2. 错误的捕获方式
            即时运行错误的捕获方式
                1. try...catch
                2. window.onerror
            资源加载错误(不会冒泡,但会触发捕获)
                1. object.onerror (image, script标签可以添加onerror事件)
                2. performance.getEntries (高级浏览器,可以获取加载所有资源的集合,通过和浏览器中需要加载资源的对比可以得到加载错误的资源)
                3. Error事件捕获(可以通过事件捕获阶段获取错误)
```
window.addEventListener('error', function (e) {
    console.log('捕获错误',e)
},true)
```
            延伸：跨域的JS运行错误可以捕获吗,错误提示什么,该怎么处理？
        3. 上报错误的基本原理
            1. 采用AJAX通信方式上报
            2. 使用Image标签进行上报