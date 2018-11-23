---


---

<h1 id="w3c-笔记">W3C 笔记</h1>
<h2 id="细节">细节</h2>
<ul>
<li><strong>对代码行进行折行</strong></li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript">document<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span><span class="token string">"Hello \
World!"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li><strong>重新声明 JavaScript 变量</strong><br>
如果重新声明 JavaScript 变量，该变量的值不会丢失：<br>
在以下两条语句执行后，变量 carname 的值依然是 “Volvo”：</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">var</span> carname<span class="token operator">=</span><span class="token string">"Volvo"</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> carname<span class="token punctuation">;</span>
</code></pre>
<ul>
<li><strong>向未声明的 JavaScript 变量来分配值</strong><br>
如果您把值赋给尚未声明的变量，该变量将被自动作为全局变量声明。<br>
将声明一个_全局_变量 carname，即使它在函数内执行。</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript">carname<span class="token operator">=</span><span class="token string">"Volvo"</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li><strong>with的基本用法</strong><br>
with 语句的原本用意是为逐级的对象访问提供命名空间式的速写方式. 也就是在指定的代码区域, 直接通过节点名称调用对象。<br>
with 通常被当做重复引用同一个对象中的多个属性的快捷方式，可以不需要重复引用对象本身。<br>
with的弊端:导致数据泄、性能下降</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token operator">&lt;</span>html<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>head<span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span>script type<span class="token operator">=</span><span class="token string">"text/javascript"</span><span class="token operator">&gt;</span>
        <span class="token keyword">function</span> <span class="token function">validate_email</span><span class="token punctuation">(</span>field<span class="token punctuation">,</span>alerttxt<span class="token punctuation">)</span>
        <span class="token punctuation">{</span>
            <span class="token keyword">with</span> <span class="token punctuation">(</span>field<span class="token punctuation">)</span>
            <span class="token punctuation">{</span>
                apos<span class="token operator">=</span>value<span class="token punctuation">.</span><span class="token function">indexOf</span><span class="token punctuation">(</span><span class="token string">"@"</span><span class="token punctuation">)</span>
                dotpos<span class="token operator">=</span>value<span class="token punctuation">.</span><span class="token function">lastIndexOf</span><span class="token punctuation">(</span><span class="token string">"."</span><span class="token punctuation">)</span>
                <span class="token keyword">if</span> <span class="token punctuation">(</span>apos<span class="token operator">&lt;</span><span class="token number">1</span><span class="token operator">||</span>dotpos<span class="token operator">-</span>apos<span class="token operator">&lt;</span><span class="token number">2</span><span class="token punctuation">)</span>
                <span class="token punctuation">{</span><span class="token function">alert</span><span class="token punctuation">(</span>alerttxt<span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">}</span>
                <span class="token keyword">else</span> <span class="token punctuation">{</span><span class="token keyword">return</span> <span class="token boolean">true</span><span class="token punctuation">}</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span>
        <span class="token keyword">function</span> <span class="token function">validate_form</span><span class="token punctuation">(</span>thisform<span class="token punctuation">)</span>
        <span class="token punctuation">{</span>
            <span class="token keyword">with</span> <span class="token punctuation">(</span>thisform<span class="token punctuation">)</span>
            <span class="token punctuation">{</span>
                <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">validate_email</span><span class="token punctuation">(</span>email<span class="token punctuation">,</span><span class="token string">"Not a valid e-mail address!"</span><span class="token punctuation">)</span><span class="token operator">==</span><span class="token boolean">false</span><span class="token punctuation">)</span>
                <span class="token punctuation">{</span>email<span class="token punctuation">.</span><span class="token function">focus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">}</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span>
    <span class="token operator">&lt;</span><span class="token operator">/</span>script<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>head<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>body<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>form action<span class="token operator">=</span><span class="token string">"submitpage.htm"</span>onsubmit<span class="token operator">=</span><span class="token string">"return validate_form(this);"</span> method<span class="token operator">=</span><span class="token string">"post"</span><span class="token operator">&gt;</span>
    Email<span class="token punctuation">:</span> <span class="token operator">&lt;</span>input type<span class="token operator">=</span><span class="token string">"text"</span> name<span class="token operator">=</span><span class="token string">"email"</span> size<span class="token operator">=</span><span class="token string">"30"</span><span class="token operator">&gt;</span>
    <span class="token operator">&lt;</span>input type<span class="token operator">=</span><span class="token string">"submit"</span> value<span class="token operator">=</span><span class="token string">"Submit"</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>form<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>body<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>html<span class="token operator">&gt;</span>
</code></pre>

