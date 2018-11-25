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
<li><strong>继承</strong><br>
<img src="http://resouce.dongdongwedding.com/9ADB17F31302F9253DD1D210FBE40E26.png" alt="enter image description here"></li>
</ul>

