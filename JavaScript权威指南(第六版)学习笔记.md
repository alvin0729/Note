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
<li><strong>检测属性</strong><br>
可以通过in运算符、hasOwnPreperty()和<br>
propertyIsEnumerable()方法来完成这个工作，甚至仅通过属性查询也可以做到这一点。<br>
<code>var o = {x:1}; var hasX = "x" in o;</code><br>
除了使用in运算符之外，另一种更简便的方法是使用"!=="判断一个属性是否是undefined。</li>
<li><strong>枚举属性</strong></li>
</ul>

