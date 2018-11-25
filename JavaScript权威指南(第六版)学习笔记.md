---


---

<h1 id="基础部分">基础部分</h1>
<ul>
<li><strong>函数作用域和申明提前</strong></li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">function</span> <span class="token function">test</span><span class="token punctuation">(</span>o<span class="token punctuation">)</span><span class="token punctuation">{</span>  
    <span class="token keyword">var</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
    <span class="token keyword">if</span><span class="token punctuation">(</span><span class="token keyword">typeof</span> o <span class="token operator">==</span> <span class="token string">"object"</span><span class="token punctuation">)</span><span class="token punctuation">{</span>  
        <span class="token keyword">var</span> j <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
        <span class="token keyword">for</span><span class="token punctuation">(</span><span class="token keyword">var</span> k<span class="token operator">=</span><span class="token number">0</span><span class="token punctuation">;</span> k <span class="token operator">&lt;</span> <span class="token number">10</span><span class="token punctuation">;</span> k<span class="token operator">++</span><span class="token punctuation">)</span><span class="token punctuation">{</span>  
            console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>k<span class="token punctuation">)</span><span class="token punctuation">;</span>  
        <span class="token punctuation">}</span>  
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>k<span class="token punctuation">)</span><span class="token punctuation">;</span>  
    <span class="token punctuation">}</span>  
    console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>i<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<pre><code>var scope = "global";  
function f() {  
    console.log(scope);  
    var scope = "local";  
    console.log(scope);  
}
</code></pre>
<ul>
<li><strong>for/in</strong></li>
</ul>
<pre><code>var o = {x:1, y:2, z:3};  
var a = [], i = 0;  
for(a[i++] in o);  
for(i in a) console.log(i);
</code></pre>
<h1 id="对象">对象</h1>
<h3 id="创建对象">创建对象</h3>
<ul>
<li><strong>对象直接量</strong><br>
<code>var point = {x:0, y:0};</code></li>
<li><strong>通过new创建对象</strong><br>
<code>var o = new Object();</code></li>
<li><strong>通过new创建对象</strong></li>
</ul>
<pre><code>var o1 = Object.create(point);  
var o2 = Object.create(null);  
var o3 = Object.create(Object.prototype);  //{}
</code></pre>
<h3 id="属性的查询与设置">属性的查询与设置</h3>
<p><code>var len = book &amp;&amp; book.subtitle &amp;&amp; book.subtitle.length</code></p>
<ul>
<li><strong>继承</strong></li>
</ul>
<pre><code>  
//例6一1：通过原型继承创建一个新对象  
//inherit()返回了一个继承自原型对象p的属性的新对象  
//这里使用ECMAScript 5中的Object.create()函数（如果存在的话）  
//如果不存在Object.create()，则退化使用其他方法  
function inherit(p) {  
  if (p == null) throw TypeError();  //p是一个对象，但不能是null  
  if (Object.create)          //如果Object.create()存在  
  return Object.create(p);    //直接使用它  
  var t = typeof p;           //否则进行进一步检测  
  if (t !== "object" &amp;&amp; t !== "function") throw TypeError();  
    function f() {};          //定义一个空构造函数  
  f.prototype = p;            //将其原型属性设置为p  
  return new f();             //使用f()创建p的继承对象  
}
</code></pre>
<blockquote>
<p>o中的属性p是只读的：不能给只读属性重新赋值(defineproperty()方法中有一个例外，可以对可配置的只读属性重新赋值）。<br>
o中的属性p是继承属性，且它是只读的：不能通过同名自有属性覆盖只读的继承属性。<br>
o中不存在自有属性p：o没有使用setter方法继承属性p，并且o的可扩展性(extensible attribute)是false（参照6.8.3节）。如果o中不存在p，而且没有setter方法可供调用，则p一定会添加至o中。但如果o不是可扩展的，那么在o中不能定义新属性。</p>
</blockquote>
<ul>
<li><strong>删除属性</strong></li>
</ul>
<blockquote>
<p>delete运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象上删除它，而且这会影响到所有继承自这个原型的对象）。</p>
</blockquote>
<pre><code>delete Object.prototype; //不能删除，属性是不可配置的
var x=1;            //声明一个全局变量
delete this.x;      //不能删除这个属性
functionf(){}       //声明一个全局函数
delete this.f;      //也不能删除全局函数
</code></pre>
<ul>
<li>
<p><strong>检测属性</strong><br>
可以通过in运算符、hasOwnPreperty()和<br>
propertyIsEnumerable()方法来完成这个工作，甚至仅通过属性查询也可以做到这一点。<br>
<code>var o = {x:1}; var hasX = "x" in o;</code><br>
除了使用in运算符之外，另一种更简便的方法是使用"!=="判断一个属性是否是undefined。</p>
</li>
<li>
<p><strong>枚举属性</strong><br>
除了for/in循环之外，ECMAScript5定义了两个用以枚举属性名称的函数。第一个是0bject.keys()，它返回一个数组，这个数组由对象中可枚举的自有属性的名称组成。第二个枚举属性的函数是0bject.get0wnPropertyNames(),它和0jbect.keys()类似，只是它返回对象的所有自有属性的名称，而不仅仅是可枚举的属性。</p>
</li>
<li>
<p><strong>属性getter和setter</strong></p>
</li>
</ul>
<pre><code>var p = {  
    //x和y是普通的可读写的数据属性  
  x:1.0,  
    y:1.0,  
  
    //r是可读写的存取器属性，它有getter和setter.  
 //函数体结束后不要忘记带上逗号  get r() {return Math.sqrt(this.x*this.x + this.y*this.y);},  
    set r(newvalue) {  
        var oldvalue = Math.sqrt(this.x*this.x + this.y*this.y);  
        var ratio = newvalue/oldvalue;  
        this.x *= ratio;  
        this.y *=ratio;  
    },  
    //theta是只读存取器属性，它只有getter方法  
  get theta() {return Math.atan2(this.y, this.x);},  
}
</code></pre>
<pre><code>var serialnum = {  
    //这个数据属性包含下一个序列号  
 //$符号暗示这个属性是一个私有属性  $n: 0,  
  
    //返回当前值，然后自增  
  get next() {return this.$n++; },  
  
    //给n设置新的值，但只有当它比当前值大时才设置成功  
  set next(n) {  
        if (n &gt;= this.$n) this.$n = n;  
        else  throw "序列号的值不能比当前值小";  
    }  
};
</code></pre>
<ul>
<li><strong>属性的特性</strong><br>
通过调用0bject.getOwnpropertyDecriptor()可以获得某个对象特定属性的属性描述符：</li>
</ul>
<pre><code>var random = {  
    get octet() {return Math.floor((Math.random() * 256));}  
};  
  
//返回{value:1，writable:true，enumerable:true,configurable:true}  
Object.getOwnPropertyDescriptor({x:1},"x");  
  
//查询上文中定义的randam对象的octet属性  
//返回{get：/*func*/,set:undefined,enumerable:true,configurable：true}  
Object.getOwnPropertyDescriptor(random,"octet");  
//对于继承属性和不存在的属性，返回undefined  
Object.getOwnPropertyDescriptor({},"x");         //undefined   没有这个属性  
Object.getOwnPropertyDescriptor({},"toString");  //undefined   继承属性
</code></pre>
<pre><code>var o ={};//创建一个空对象  
//添加一个不可枚举的数据属性x，并赋值为1  
  
Object.defineProperty(o, "x", {  
    value : 1,  
    writable: true,  
    enumerable: false,  
    configurable: true  
});  
  
//属性是存在的，但不可枚举  
o.x;             //=&gt; 1  
Object.keys(o);  //=&gt; []  
  
//现在对属性×做修改，让它变为只读  
Object.defineProperty(o, "x", {writable: false});  
  
//试图更改这个属性的值  
o.x=2;           //操作失败但不报错，而在严格模式中抛出类型错误异常  
o.x;             //=&gt; 1  
  
//属性依然是可配置的，因此可以通过这种方式对它进行修改：  
Object.defineProperty(o, "x", {value:2});  
o.x;             //=&gt; 2  
  
//现在将x从数据属性修改为存取器属性  
Object.defineProperty(o, "x", {get: function () {  
    return 0;  
}});  
o.x;             //=&gt; 0
</code></pre>
<pre><code>var p = Object.defineProperties({},{  
    x:{value:1,writable:true,enumerable:true,configuable:true},  
    r:{  
        get: function () {  
            return 0;  
        },  
        enumerable:true,  
        configurable:true  
  }  
});
</code></pre>
<blockquote>
<p>如果对象是不可扩展的，则可以编辑已有的自有属性，但不能给它添加新属性。<br>
如果属性是不可配置的，则不能修改它的可配置性和可枚举性。<br>
如果存取器属性是不可配置的，则不能修改其getter和setter方法，也不能将它转换为数据属性。<br>
如果数据属性是不可配置的，则不能将它转换为存取器属性。<br>
如果数据属性是不可配置的，则不能将它的可写性从false修改为true，但可以从true修改为false。<br>
如果数据属性是不可配置且不可写的，则不能修改它的值。然而可配置但不可写属性的值是可以修改的（实际上是先将它标记为可写的，然后修改它的值，最后转换为不可写的）。</p>
</blockquote>
<p>复制属性的特性</p>
<pre><code>/*  
*给Object.prototype添加一个不可枚举的extend()方法
*这个方法继承自调用它的对象，将作为参数传人的对象的属性一一复制 
*除了值之外，也复制属性的所有特性，除非在目标对象中存在同名的属性， 
*参数对象的所有自有对象（包括不可枚举的属性）也会一一复制。 
*/
Object.defineProperty(Object.prototype,  
    "extend",                  // 定义 Object.prototype.extend  
  {  
        writable: true,  
        enumerable: false,     // 将其定义为不可枚举的  
  configurable: true,  
        value: function(o) {   // 值就是这个函数  					
        // 得到所有的自有属性，包括不可枚举属性  
		var names = Object.getOwnPropertyNames(o);  
		// 遍历它们  
		for(var i = 0; i &lt; names.length; i++) {  
		    // 如果属性已经存在，则跳过  
		  if (names[i] in this) continue;  
		    // 获得o中的属性的描述符  
		  var desc = Object.getOwnPropertyDescriptor(o,names[i]);  
		    // 用它给this创建一个属性  
		  Object.defineProperty(this, names[i], desc);  
		}  
        }  
    });
</code></pre>

