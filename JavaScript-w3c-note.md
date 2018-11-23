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
如果您把值赋给尚未声明的变量，该变量将被自动作为全局变量声明。</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript">carname<span class="token operator">=</span><span class="token string">"Volvo"</span><span class="token punctuation">;</span>
</code></pre>
<p>将声明一个_全局_变量 carname，即使它在函数内执行。</p>

