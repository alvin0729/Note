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
  function f() {};            //定义一个空构造函数  
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
<pre><code>var o = {x:1,y:1};  
  
Object.defineProperty(o, "x", {  
    value : 2,  
    writable: true,  
    enumerable: false,  
    configurable: true  
});  
  
Object.extend(o);  
console.log(Object.x);  
for(var k in Object){  
    console.log(k);  
}
</code></pre>
<h3 id="对象的三个属性">对象的三个属性</h3>
<ul>
<li><strong>prototype</strong><br>
通过对象直接量或0bject.create()创建的对象包含一个名为constructor的属性，这个属性指代0bject()构造函数。因此，constructor.prototype才是对象直接量的真正的原型，但对于通过0bject·create()创建的对象则往往不是这样。</li>
</ul>
<pre><code>var p = {x:1};  
console.log(Object.getPrototypeOf(p));  
var o = Object.create(p);  
console.log(p.isPrototypeOf(o));;  
console.log(Object.prototype.isPrototypeOf(o));
</code></pre>
<ul>
<li><strong>类属性</strong><br>
classof函数可以返回传递给它的任意对象的类：</li>
</ul>
<pre><code>function classof(o) {
    if (o === null) return "Null";
    if (o === undefined) return "Undefined";
    return Object.prototype.toString.call(o).slice(8,-1);
}
</code></pre>
<ul>
<li><strong>可扩展性</strong><br>
ECMAScript5定义了用来查询和设置对象可扩展性的函数。通过将对象传人0bject.esExtenble()，来判断该对象是否是可扩展的。如果想将对象转换为不可扩展的，需要调用0bject.preventExtensions(),将待转换的对象作为参数传进去。注意，一旦将对象转换为不可扩展的，就无法再将其转换回可扩展的了。同样需要注意的是，<br>
preventExtensions()只影响到对象本身的可扩展性。如果给一个不可扩展的对象的原型添加属性，这个不可扩展的对象同样会继承这些新属性。</li>
</ul>
<pre><code>//创建一个封闭对象，包括一个冻结的原型和一个不可枚举的属性  
var o=Object.seal(Object.create(Object.freeze({x:1}), {y:{value:2,writable:true}}));
</code></pre>
<ul>
<li><strong>序列化对象</strong></li>
</ul>
<pre><code>o ={x:1,y:{z:[false,null,""]}};    //定义一个测试对象  
s = JSON.stringify(o);             //{ x: 1, y: { z: [ false, null, '' ] } }  
p = JSON.parse(s);                 //p是o的深拷贝
</code></pre>
<ul>
<li><strong>对象方法</strong><br>
toString()<br>
toLocaleString()<br>
toJSON()<br>
valueOf()</li>
</ul>
<h1 id="数组">数组</h1>
<pre><code>var base = 1024;  
var table = [base, base+1, base+2,base+3];

var count = [1,,3];  //数组看3个元素，中间的那个元素值为undefined  
var undefs = [,,];   //数组有2个元素，都是undefined

a[-1.23] = true;  //这将创建一个名为"-1.23"的属性  
a["1000"] = 0;    //这是数组的第1001个元素  
a[1.000] = 2;     //和a[1]相等
</code></pre>
<ul>
<li><strong>稀疏数组</strong></li>
</ul>
<pre><code>a = [1,2,3,4,5];   //从5个元素的数组开始  
a.length = 3;      //现在a为[1，2，3]  
a.length = 0;      //删除所有的元素。a为[]  
a.length = 5;      //长度为5，但是没有元素，就像neArray(5)

Object.defineProperty(a,"length",{writable:false});  //让length属性只读
</code></pre>
<ul>
<li><strong>数组元素的添加与删除</strong></li>
</ul>
<pre><code>a = [];  
a[0] = "zero";  
a.push("one");  
a.push("two","three");

a = [1,2,3];  
delete a[1];   //a在索引1的位置不再有元素  
1 in a;         //=〉false:数组索引1并未在数组中定义  
a.length;       //3：delete操作并不影响数组长度
</code></pre>
<pre><code>var o = {x:1,y:2};  
var keys = Object.keys(o);          //获得o对象属性名组成的数组  
var values = [];                    //在数组中存储匹配属性的值  
for(var i = 0; i&lt; keys.length; i++){//对于数组中每个索引  
  var key = keys[i];              //获得索引处的键值  
  values[i] = o[key];              //在values数组中保存属性值  
}
</code></pre>
<pre><code>for(var i = 0; i&lt; a.length; i++){  
    if(!a[i]) continue;             //跳过null、undefined和不存在的元素  
  if(a[i] === undefined) continue;//跳过undefined和不存在的元素  
  if(!(i in a)) continue;         //跳过不存在的元素  
}  
  
//不存在的索引将不会遍历到  
for(var index in a){  
    if(!a.hasOwnProperty(i)) continue; //跳过继承的属性  
 //跳过不是非负整数的i  if(String(Math.floor(Math.abs(Number(index)))) !== index) continue;  
    var value = a[index];  
}  
var data = [1,2,3,4,5];  
var sumOfSquares = 0;  
//按照索引的顺序按个传递
data.forEach(function (x) {  
    sumOfSquares += x*x;  
})
</code></pre>
<ul>
<li><strong>数组方法</strong></li>
</ul>
<pre><code>var a = [1,2,3];  
console.log(a.join(" "));  
var b = new Array(10);  
console.log(b.join("-"));  
  
console.log(a.reverse().join()); //3,2,1  
console.log(a);                  //[ 3, 2, 1 ]  
  
console.log(a.sort());  
console.log(a.sort(function (a, b) {  
    return a - b; //负数升序  
}));  
  
console.log(a.concat(4, 5));  
  
//参数一1指定了最后一个元素，而一3指定了倒数第三个元素。  
console.log(a.slice(1, -1));
</code></pre>
<p>splice()<br>
Array·splice()方法是在数组中插人或删除元素的通用方法。</p>
<pre><code>var a = [1,2,3,4,5,6,7,8];  
console.log(a.splice(4));   //[ 5, 6, 7, 8 ]  
console.log(a); //[ 1, 2, 3, 4 ]  
console.log(a.splice(1, 2));//[ 2, 3 ]  
console.log(a); //[ 1, 4 ]  
console.log(a.splice(1, 1));//[ 4 ]  
console.log(a); //[ 1 ]
</code></pre>
<pre><code>var a = [1,2,3,4];  
console.log(a.splice(2, 0, 'a', 'b')); //[]  
console.log(a); //[ 1, 2, 'a', 'b', 3, 4 ]  
console.log(a.splice(2, 2, [1, 2], 3)); //[ 'a', 'b' ]  
console.log(a); //[ 1, 2, [ 1, 2 ], 3, 3, 4 ]
</code></pre>
<p>unshift()和shift()方法的行为非常类似于push()和pop()，不一样的是前者是在数组的头部而非尾部进行元素的插人和删除操作。</p>
<pre><code>var data = [1,2,3,4,5];  
data.forEach(function (v,i,a) {  
    a[i] = v + 1;  
});  
console.log(data); //[ 2, 3, 4, 5, 6 ]  
/*注意，forEach()无法在所有元素都传递给调用的函数之前终止遍历。也就是说，没有  
像for循环中使用的相应的break语句。如果要提前终止，必须把forEach()方法放在一个  
try块中，并能抛出一个异常。如果forEach()调用的函数抛出foreach.break异常，循环  
会提前终止：  
 function  foreach(a,f,t) { try{a.forEach(f,t);} catch(e) { if (e === foreach.break) return; else throw e; } } foreach.break = new Error("StopIteration");
 */
</code></pre>
<pre><code>//稀疏数组  
var array = new Array(3);  
array[2] = "name";  
  
for(var a in array)  
{  
    console.log("index=" + a + ",value=" + array[a]);  
}  
  
// 密集数组  
var dense = Array.apply(null, Array(3));  
dense[2] = "name";  
for(var a in dense)  
{  
    console.log("index=" + a + ",value=" + dense[a]);  
}
</code></pre>
<pre><code>var a = [1,2,3];  
b = a.map(function (x) {  
    return x*x;  
});  
  
smallValues = a.filter(function (x,i) {  
    return i%2==0;  
})  
  
saprse = new Array();  
saprse[2] = 4;  
saprse[10] = 2;  
//返回密集数组  
var dense = saprse.filter(function () {  
    return true;  
});  
//压缩空缺并删除undefined和null元素  
a.filter(function (x) {  
    return x !== undefined &amp;&amp; x != null;  
});
</code></pre>
<p>注意，一旦every()和some()确认该返回什么值它们就会停止遍历数组元素。some()在判定函数第一次返回true后就返回true，但如果判定函数一直返回false，它将会遍历整个数组。every()恰好相反：它在判定函数第一次返回false后就返回false，但如果判定函数一直返回true，它将会遍历整个数组。注意，根据数学上的惯例，在空数组上调用时，every()返回true,some()返回false。</p>
<pre><code>reduce() reduceRight()  
  
var a = [1,2,3,4,5];  
var sum = a.reduce(function (x,y) {  
    return x+y  
}, 0);  
var product = a.reduce(function (x,y) {  
    return x*y  
},1);  
var max = a.reduce(function (x,y) {  
    return (x&gt;y)?x:y;  
});
</code></pre>
<pre><code>/*
 * Copy the enumerable properties of p to o, and return o.
 * If o and p have a property by the same name, o's property is overwritten.
 * This function does not handle getters and setters or copy attributes.
 */
function extend(o, p) {
    for(prop in p) {                         // For all props in p.
        o[prop] = p[prop];                   // Add the property to o.
    }
    return o;
}

/*
 * Copy the enumerable properties of p to o, and return o.
 * If o and p have a property by the same name, o's property is left alone.
 * This function does not handle getters and setters or copy attributes.
 */
function merge(o, p) {
    for(prop in p) {                           // For all props in p.
        if (o.hasOwnProperty[prop]) continue;  // Except those already in o.
        o[prop] = p[prop];                     // Add the property to o.
    }
    return o;
}

/*
 * Remove properties from o if there is not a property with the same name in p.
 * Return o.
 */
function restrict(o, p) {
    for(prop in o) {                         // For all props in o
        if (!(prop in p)) delete o[prop];    // Delete if not in p
    }
    return o;
}

/*
 * For each property of p, delete the property with the same name from o.
 * Return o.
 */
function subtract(o, p) {
    for(prop in p) {                         // For all props in p
        delete o[prop];                      // Delete from o (deleting a
                                             // nonexistent prop is harmless)
    }
    return o;
}

/*
 * Return a new object that holds the properties of both o and p.
 * If o and p have properties by the same name, the values from o are used.
 */
function union(o,p) { return extend(extend({},o), p); }

/*
 * Return a new object that holds only the properties of o that also appear
 * in p. This is something like the intersection of o and p, but the values of
 * the properties in p are discarded
 */
function intersection(o,p) { return restrict(extend({}, o), p); }

/*
 * Return an array that holds the names of the enumerable own properties of o.
 */
function keys(o) {
    if (typeof o !== "object") throw TypeError();  // Object argument required
    var result = [];                 // The array we will return
    for(var prop in o) {             // For all enumerable properties
        if (o.hasOwnProperty(prop))  // If it is an own property
            result.push(prop);       // add it to the array.
    }
    return result;                   // Return the array.
}
</code></pre>
<pre><code>var a = [1,2,3,4,5];  
var sum = a.reduce(function (x,y) {  
    return x+y  
}, 0);  
var product = a.reduce(function (x,y) {  
    return x*y  
},1);  
var max = a.reduce(function (x,y) {  
    return (x&gt;y)?x:y;  
});  
  
var objects = [{x:1,a:1},{y:2,a:2}];  
var leftunion = objects.reduce(union);       //{ x: 1, a: 2, y: 2 }  
var rightunion = objects.reduceRight(union); //{ y: 2, a: 1, x: 1 }
</code></pre>
<pre><code>a = [0,1,2,1,0];  
console.log(a.indexOf(1));        //1  
console.log(a.lastIndexOf(1));    //3  
console.log(a.indexOf(3));        //-1
</code></pre>
<pre><code>//在数组中查找所有出现的x，并返回一个包含匹配索引的数组  
function findall(a,x) {  
    var results = [],  
        len = a.length,  
        pos = 0;  
    while(pos &lt; len){  
        pos = a.indexOf(x,pos);  
        if(pos === -1) break;  
        results.push(pos);  
        pos = pos + 1;  
    }  
    return results;  
}
</code></pre>
<ul>
<li><strong>类数组对象</strong></li>
</ul>
<pre><code>console.log(Array.isArray([]));  
console.log(Array.isArray({}));
  
//判定o是否是一个类数组对象  
//字符串和函数有length属性，但是它们  
//可以用type检测将其排除。在客户端JavaScript中，DOM文本节点  
//也有length属性，需要用额外判断o.nodeType =3将其排除  
function isArrayLike(o)  
{  
    if (o &amp;&amp;                         //o非null、undefined等  
  typeof o === "object" &amp;&amp;     //o是对象  
  isFinite(o.length) &amp;&amp;        //o.length是有限数值  
  o.length &gt;= 0 &amp;&amp;             //o.length为非负值  
  o.length === Math.floor(o.length) &amp;&amp;  //o.length是整数  
  o.length &lt; 4294967296)       //o.length &lt; 2^32  
  return true;                 //o是类数组对象  
  else  
 return false;                //否则它不是  
}
</code></pre>
<blockquote>
<p>console.log(typeof(a));<br>
console.log(Object.prototype.toString.call(a));<br>
既然类数组对象没有继承自Array·prototype，那就不能在它们上面直接调用数组方法。尽管如此，可以间接地使用Functn·call方法调用：</p>
</blockquote>
<pre><code>var a = {"0":"a","1":"b","2":"c",length:3};  
console.log(Array.prototype.join.call(a, "+"));  
console.log(Array.prototype.slice.call(a, 0));  
console.log(Array.prototype.map.call(a, function (x) {  
    return x.toUpperCase();  
}));

Array.join = Array.join || function (a,sep) {  
        return Array.prototype.join.call(a,sep);  
}  
Array.slice = Array.slice || function (a, from, to) {  
        return Array.prototype.slice.call(a,from,to);  
}  
Array.map = Array.map || function (a, f, thisArg) {  
        return Array.prototype.map.call(a,f,thisArg)  
}

s = "JavaScript";  
console.log(Array.prototype.join.call(s, " "));  
console.log(Array.prototype.filter.call(s, function (x) {  
    return x.match(/[^aeiou]/);  
}).join(""));
</code></pre>
<h1 id="函数">函数</h1>
<h3 id="函数调用">函数调用</h3>
<p>有4种方式来调用JavaScript函数：</p>
<p>作为函数</p>
<p>作为方法</p>
<p>作为构造函数</p>
<p>通过它们的ca11()和apply()方法间接调用</p>
<p>//定义并调用一个函数来确定当前脚本运行时是否为严格模式</p>
<p><code>var stlict = (function(){return!this;)());</code></p>
<pre><code>
var calculator = {

operand1:1,

operand2:1,

add: function () {

this.result = this.operand1 + this.operand2;

}

};

calculator.add();

console.log(calculator.result);

</code></pre>
<p>和变量不同，关键字this没有作用域的限制，嵌套的函数不会从调用它的函数中继承</p>
<p>this.如果嵌套函数作为方法调用，其this的值指向调用它的对象。如果嵌套函数作为</p>
<p>函数调用，其this值不是全局对象〈非严格模式下）就是undefined（严格模式下）。</p>
<pre><code>
var o = { //对象o

m: function () { //对象中的方法m()

var self = this; //将this的值保存至一个变量中

console.log((this === o)); //输出true，this就是这个对象o

f(); //倜用辅助函数f()

function f() { //定义一个嵌套函数f()

console.log((this === o)); //"false":this的值是全局对象undefined

console.log((self === o)); //"true"：self指外部函数的this值

}

}

}

</code></pre>
<ul>
<li><strong>构造函数调用</strong></li>
</ul>
<pre><code>
var o = new Object();

var o = new Object;

</code></pre>
<h3 id="函数的实参和形参">函数的实参和形参</h3>
<ul>
<li><strong>可选形参</strong></li>
</ul>
<pre><code>
function getPropertyNames(o, /* optional */a) {

if(a === undefined) a = [];

//a = a || [];

for(var property in o) a.push(property);

return a;

}

</code></pre>
<p>当数的实参可选时往往传人一个无意义的占位符，慣用做法是传入null作为占垃符，</p>
<p>当然也可以使用undefined作为占位符．</p>
<pre><code>
function f(x,y,z) {

if(arguments.length != 3){

throw new Error("function f called with" + arguments.length +

"arguments, but if expects 3 arguments.");

}

}

</code></pre>
<p>arguments并不是真正的数组，它是一个实参对象</p>
<pre><code>
function f(x) {

console.log(x);

arguments[0] = null;

console.log(x);

}

</code></pre>
<p>callee属性在某些时候会非常有用，比如在匿名函数中通过callee来递归地调用</p>
<p>自身。</p>
<pre><code>
var factorial = function (x) {

if(x &lt;= 1) return 1;

return x * arguments.callee(x-1);

};

</code></pre>
<ul>
<li><strong>将对象属性用作实参</strong></li>
</ul>
<pre><code>
function sum(a) {

if(isArrayLike(a)){

var total = 0;

for(var i = 0; i &lt; a.length; i++){

var element = a[i];

if(element == null) continue;

if(isFinite(element)) total += element;

else throw new Error("sum(): elements must be finite numbers");

}

return total;

}

else throw new Error("sum() argument must be array-like");

}

</code></pre>
<pre><code>
function flexisum(a) {

var total = 0;

for(var i = 0; i &lt; a.length; i++){

var element = a[i], n;

if(element == null) continue;

if(isArrayLike(element)){

n = flexisum.apply(this,element);

}else if(typeof element === "function"){

n = Number(element());

}else{

n = Number(element);

}

if(isNaN(n)) throw new Error("flexisum(): can't convert" + element + "to number");

total += n;

}

return total;

}

</code></pre>
<h3 id="作为值的函数">作为值的函数</h3>
<pre><code>
var o = {square: function(x) {

return x*x;

}};

var y = o.square(16);

</code></pre>
<ul>
<li><strong>自定义函数属性</strong></li>
</ul>
<pre><code>
//初始化函数对象的计数器属性

//由于函数声明被提前了，因此这里是可以在函数声明

//之前给它的成员賦值的

uniquelnteger.counter = 0;

//每次调用这个函数都会返回一个不同的整数

//它使用一个属性来记住下一次将要返回的值

function uniquelnteger() {

return uniquelnteger.counter++; //先返回计数器的值，然后计数器自增1

}

</code></pre>
<pre><code>
//计算阶乘，并将结果缓存至函数的属性中

function factorial(n) {

if(isFinite(n) &amp;&amp; n &gt; 0 &amp;&amp; n == Math.round(n)){ //有限的正整数

if(!(n in factorial)){ //如果没有缓存结果

factorial[n] = n * factorial(n-1); //计算结果并缓存之

}

return factorial[n]; //返回緩存结果

}else {

return NaN; //如果输人有误

}

}

factorial[1] = 1; //初始化缓存以保存这种基本情况

</code></pre>
<ul>
<li><strong>作为命名空间的函数</strong></li>
</ul>
<pre><code>
// 例8-3：特定场景下返回带补丁的extend()版本

//定义一个扩展函数，用来将第二个以及后续参数复制至第一个参数

//这里我们处理了IE bug：在多数IE版本中

//如果o的属性拥有一个不可枚举的同名属性，则for／in循环

//不会枚举对象o的可枚举属性，也就是说,将不会正确地处理诸如toString的属性

//除非我们显式检测它

var extend = (function() { // 将这个函数的返回值賦值给extend

// 在修复它之前·首先检查是否存在bug

for(var p in {toString:null}) {

// 如果代码执行到这里，那么for/in循环会正确工作并返回

// extend()函数

return function extend(o) {

for(var i = 1; i &lt; arguments.length; i++) {

var source = arguments[i];

for(var prop in source) o[prop] = source[prop];

}

return o;

};

}

//如果代码执行到这里，说明for/in循环不会枚举测试对象的toString属性

//因此返回另一个版本的extend()函数，这个函数显式测试

//Object.prototype中的不可枚举属性

return function patched_extend(o) {

for(var i = 0; i &lt; arguments.length; i++) {

var source = arguments[i];

//复制所有的可枚举属性

for(var prop in source) o[prop] = source[prop];

//现在检查特殊属性

for(var j = 0; j &lt; protoprops.length; j++) {

prop = protoprops[j];

if (source.hasOwnProperty(prop)) o[prop] = source[prop];

}

}

return o;

};

//这个列表列出了需要检查的特殊属性

var protoprops = ["toString", "valueOf", "constructor", "hasOwnProperty",

"isPrototypeOf", "propertyIsEnumerable","toLocaleString"];

}());

</code></pre>
<ul>
<li><strong>闭包</strong></li>
</ul>
<pre><code>
var scope = "global scope"; //全局变量

function checkscope() {

var scope = "local scope"; //局部变量

function f() {

return scope; //在作用域中返回这个值

}

return f();

}

console.log(checkscope()); //local scope

var scope = "global scope"; //全局变量

function checkscope() {

var scope = "local scope"; //局部变量

function f() {

return scope; //在作用域中返回这个值

}

return f;

}

console.log(checkscope()()); //local scope

</code></pre>
<p>闭包可以捕捉到单个函数调用的局部变量，并将这些局部变</p>
<p>量用做私有状态。我们可以利用闭包这样来重写uniqueInteger()函数：</p>
<pre><code>
var uniqueInteger = (function () { //定义函数并立即返回

var counter = 0; //函数的私有状态

return function () {

return counter++;

};

})();

</code></pre>
<p>嵌套的函数是可以访问作用域内的变量的，而且可以访问外部函数中</p>
<p>定义的counter变量。当外部函数返回之后，其他任何代码都无法访问counter变量，只</p>
<p>有内部的函数才能访问到它。</p>
<p>像counter一样的私有变量不是只能用在一个单独的闭包内，在同一个外部函数内定义的</p>
<p>多个嵌套函数也可以访问它，这多个嵌套函数都共享一个作用域链，看一下这段代码：</p>
<pre><code>
function counter() {

var n = 0;

return {

count: function () {

return n++;

},

reset: function () {

n = 0;

}

}

}

var c = counter(),d=counter();

console.log(c.count());

console.log(d.count());

c.reset();

console.log(c.count());

console.log(d.count());

</code></pre>
<pre><code>
function counter(n) { //函数参数n是一个私有变量

return {

get count() { return n++;},

set count(m){

if(m &gt;= n) n = m;

else throw Error("count can only be set to a larger value");

}

};

}

var c = counter(1000);

console.log(c.count);

console.log(c.count);

c.count = 2000;

console.log(c.count);

c.count = 2000;

</code></pre>
<pre><code>
//利用闭包实现的私有属性存馭器方法

//这个函数给对象o增加了属性存取器方法

//方法名称为get&lt;name&gt;和set&lt;name&gt;.如果提供了一个判定函数

//setter方法就会用它来检测参数的合法性，然后在存储它

//如果判定函数返回false，setter方法抛出一个异常

//

//这个函数有一个非同寻常之处。就是getter和“tt舡函数

//所操作的属性值并没有存储在对象o中

//相反，这个值仅仅是保存在函数中的局部变量中

//getter和setter方法同样是局部函数，因此可以访同这个局部变量

//也就是说,对于两个存取器方法说这个变量是私有的

//没有办法绕过存取器方法来设置或修改这个值

function addPrivateProperty(o, name, predicate) {

var value; // 这是一个属性值

// getter方法簡单地将其返回

o["get" + name] = function() { return value; };

// setter方法首先检查值是否合法，若不合法就抛出异常

// 否则就将其存储起来

o["set" + name] = function(v) {

if (predicate &amp;&amp; !predicate(v))

throw Error("set" + name + ": invalid value " + v);

else

value = v;

};

}

var o = {};

addPrivateProperty(o, "Name", function (x) {

return typeof x == "string";

});

o.setName("Frank");

console.log(o.getName());

o.setName(o);

</code></pre>
<pre><code>
function constfunc(v) {

return function () {

return v;

};

}

var funcs = [];

for(var i = 0; i &lt; 10; i++){

funcs[i] = constfunc(i);

}

console.log(funcs[5]());

</code></pre>
<pre><code>
function constfuncs() {

var funcs = [];

for(var i = 0; i &lt; 10; i++){

funcs[i] = function () {

return i;

}

}

return funcs;

}

var funcs = constfuncs();

console.log(funcs[5]());

</code></pre>
<p>上面这段代码创建了10个闭包，并将它们存储到一个数组中。这些闭包都是在同一个函</p>
<p>数调用中定义的，因此它们可以共享变量i。当constfuncs()返回时，变量的值是10，</p>
<p>所有的闭包都共享这一个值，因此，数组中的函数的返回值都是同一个值，这不是我们</p>
<p>想要的结果。关联到閉包的作用域链都是“活动的“，记住这一点非常重要。嵌套的函</p>
<p>数不会将作用域内的私有成员复制一份，也不会对所綁定的变量生成静态快照(static snapshot)。</p>
<h3 id="函数属性、方法和构造函数">函数属性、方法和构造函数</h3>
<ul>
<li><strong>length属性</strong></li>
</ul>
<pre><code>
//这个函数使用arguments.callee，因此它不能在严格模式下工作

function check(args) {

var actual = args.length; //实参的真实个数

var expected = args.callee.length; //期望的实参个数

if(actual !== expected) {

throw Error("Expected " + expected + "args; got" + actual);

}

}

function f(x,y,z) {

check(arguments); //枪查实参个数和期望的实参个数是否一致

return x+y+z; //再执行函数的后续逻辑

}

</code></pre>
<ul>
<li>
<p><strong>prototype属性</strong></p>
</li>
<li>
<p><strong>call()方法和apply()方法</strong></p>
</li>
</ul>
<p>以对象o的方法的形式调用函数f()，并传入两个参数，可以使用这样的代码：f.call(o,1,2);</p>
<p>apply()方法和call()类似，但传入实参的形式和call()有所不同，它的实参都放入一个数组当中：f.apply(o,[1,2]);</p>
<p>需要注意的是，传入apply()的参数数组可以是类数组对象也可以是真实数组。实际上，可以将当前函数的arguments数组直接传入（另一个函数的）apply()来调用另一个函数，参照如下代码：</p>
<pre><code>
//将对象o中名为m()的方法替換为另一个方法

//可以在调用原始的方法之前和之后记录日志消息

function trace(o, m){

var original = o[m]; //在闭包中保存原始方法

o[m] = function(){ //定义新的方法

console.log(new Date(), "Entering:", m); //输出日志消息

var result = original.apply(this, arguments); //调用原始函数

console.log(new Date(), "Exiting:", m); //输出日志消息

return result; //返回结果

}

}

</code></pre>
<ul>
<li><strong>bind()方法</strong></li>
</ul>
<pre><code>
function f(y) { //这个是待绑定的函数

return this.x + y;

}

var o = {x:1}; //将要绑定的对象

var g = f.bind(o); //通过调用g(x)来调用o.f(x)

console.log(g(2));

</code></pre>
<pre><code>
//返回一个函数，通过调用它来调用o中的方法f()，传递它所有的实参

function bind(f, o) {

if(f.bind) return f.bind(); //如果bind()方法存在的话，使用bind()方法

else return function () { //否则，这样绑定

return f.apply(o, arguments);

};

}

</code></pre>
<pre><code>
var sum = function (x, y) { return x + y }; //返回两个实参的和值

//创建一个类似sum的新函数，但this的值绑定到null

//并且第一个参数绑定到1，这个新的函数期望只传人一个实参

var succ = sum.bind(null,1);

console.log(succ(2)); //=&gt;3：x绑定到1，并传人2作为实参y

function f(y, z) {return this.x + y + z}; //另外一个做累加计算的函数

var g = f.bind({x:1},2); //绑定this和y

console.log(g(3)); //=&gt;6：this.x绑定到1，y绑定到2，z绑定到3

</code></pre>
<ul>
<li><strong>Function()构造函数</strong></li>
</ul>
<pre><code>
function isFunction(x) {

return Object.prototype.toString.call(x) == "[object Function]";

}

</code></pre>
<h3 id="函数式编程">函数式编程</h3>
<ul>
<li><strong>使用函数处理数组</strong></li>
</ul>
<pre><code>
//对于每个数组元素调用函数f()，并返回一个结果数组

//如果Array.prototype.map定义了的话，就使用这个方法

var map = Array.prototype.map

? function (a,f) { return a.map(f);} //如果已经存在map()方法，就直接使用它

:function (a, f) { //否则，自己实现一个

var results = [];

for(var i = 0, len = a.length; i &lt; len; i++){

if(i in a ) results[i] = f.call(null, a[i], i, a)

}

return results;

}

//使用函数f()和可选的初始值将数组a减至一个值

//如果Array.prototype.reduce存在的话，就使用这个方法

var reduce = Array.prototype.reduce

? function (a,f,initial) { //如果reduce()方法存在的话

if(arguments.length &gt; 2)

return a.reduce(f,initial); //如果传人了一个初始值

else return a.reduce(f); //否则没有初始值

}

: function (a,f,initial) { //这个算法来自ES5规范

var i = 0, leng = a.length, accumulator;

//以特定的初始值开始，否则第一个值取自a

if(arguments.length &gt; 2) accumulator = initial;

else{ //找到数组中第一个已定义的索引

if(len == 0) throw TypeError();

while (i &lt; len){

if(i in a){

accumulator = a[i++];

break;

}

else i++;

}

if (i == len) throw TypeError();

}

//对于数组中剩下的元素依次调用f()

while (i &lt; len){

if(i in a)

accumulator = f.call(undefined, accumulator, a[i], i, a);

i++;

}

return accumulator;

}

var data = [1,1,3,5,5];

var sum = function (x, y) {

return x+y;

};

var square = function (x) {

return x*x;

}

var mean = reduce(data, sum)/data.length;

var deviations = map(data, function (x) {

return x - mean;

});

var stddev = Math.sqrt(reduce(map(deviations, square),sum) / (data.length - 1));

console.log(stddev);

</code></pre>
<ul>
<li><strong>高阶函数</strong></li>
</ul>
<pre><code>
//这个高阶函数返回一个新的函数，这个新函数将它的实参传人f()

//并返回f的返回值的逻辑非

function not(f) {

return function () { //返回一个新的函数

var result = f.apply(this,arguments); //调用f()

return !result; //对结果求反

};

}

var even = function (x) { //判断a是否为偶数的函数

return x % 2 === 0;

};

var odd = not(even); // 一个新函数，所做的事情和even()相反

[1,1,3,5,5].every(odd); //=&gt;true:每个元素都是奇数

</code></pre>
<pre><code>
//所返回的函数的参数应当是一个实参数组，并对每个数组元素执行函数f()

//并返回所有计算结果组成的数组

//可以对比一下这个函数和上文提到的map()函数

function mapper(f) {

return function (a) {

return map(a,f);

}

}

var increment = function (x) {

return x+1;

}

var incrementer = mapper(increment);

console.log(incrementer([1, 2, 3])); //=&gt;[2，3，4]

//返回一个新的可以计算f(g(···))的函数

//返回的函数h()将它所有的实参传人g()，然后将g()的返回值传人f()

//调用f()和g()时的this值和调用h()时的this值是同一个this

function compose(f, g) {

return function () {

//需要给f()传人一个参数，所以使用f()的ca11()方法

//需要给g()传人很多参数，所以使用g()的apply()方法

return f.call(this, g.apply(this, arguments));

};

}

var square = function (x) {

return x*x;

};

var sum = function (x, y) {

return x+y;

};

var squareofsum = compose(square, sum);

console.log(squareofsum(2, 3));

</code></pre>
<ul>
<li><strong>不完全函数</strong></li>
</ul>
<pre><code>
//实现一个工具函数将类数组对象（或对象）转换为真正的数组

//在后面的示例代码中用到了这个方法将arguments对象转换为真正的数组

function array(a,n) {

return Array.prototype.slice.call(a, n || 0);

}

//这个函数的实参传递至左侧

function partialLeft(f /*,...*/) {

var args = arguments; //保存外部的实参数组

return function () { //并返回这个函数

var a = array(args,1); //开始处理外部的第1个args

a = a.concat(array(arguments)); //然后增加所有的内部实参

return f.apply(this, a); //然后基于这个实参列表调用f()

};

}

//这个函数的实参传递至右侧

function partialRight(f /*,...*/) {

var args = arguments; //保存外部的实参数组

return function () { //并返回这个函数

var a = array(arguments); //从内部参数开始

a = a.concat(array(args,1)); //然后从外部第1个args开始添加

return f.apply(this, a); //然后基于这个实参列表调用f()

};

}

//这个函数的实参被用做模板

//实参列表中的undefined值都被填充

function partial(f /*,...*/) {

var args = arguments; //保存外部的实参数组

return function () {

var a = array(args,1); //开始处理外部的第1个args

var i = 0, j = 0;

//遍历args,从内部实参填充undefined值

for(;i &lt; a.length; i++)

if(a[i] === undefined) a[i] = arguments[j++];

//现在将剩下的内部实参都追加进去

a = a.concat(array(arguments,j))

return f.apply(this,a);

}

}

//这个函数带有三个实参

var f = function (x, y, z) {

return x*(y-z);

};

//注意这三个不完全调用之间的区别

console.log(partialLeft(f, 2)(3, 4)); // -2: 绑定第一个实参：2 *（3-4）

console.log(partialRight(f, 2)(3, 4)); // 6: 绑定最后一个实参：3 *（4-2）

console.log(partial(f, undefined, 2)(3, 4)); //=〉-6：绑定中间的实参：3 *（2-4）

</code></pre>
<pre><code>
var data = [1,1,3,5,5];

var sum = function (x, y) {

return x+y;

};

var product = function (x, y) {

return x*y;

};

var neg = partial(product,-1);

var square = partial(Math.pow, undefined, 2);

var sqrt = partial(Math.pow, undefined, 0.5);

var reciprocal = partial(Math.pow, undefined, -1);

//现在计算平均值和标准差，所有的函数调用都不带运算符

//这段代码看起来很像lisp代码

var mean = product(reduce(data, sum), reciprocal(data.length));

var stddev = sqrt(product(reduce(map(data,

compose(square, partial(sum,neg(mean)))),

sum),

reciprocal(sum(data.length, -1))));

console.log(stddev);

</code></pre>
<ul>
<li><strong>记忆</strong></li>
</ul>
<pre><code>
//返回f()的带有记忆功能的版本

//只有当f()的实参的字符串表示都不相同时它才会工作

function memorize(f) {

var cache = {}; //将值保存在闭包内

return function () {

//将实参转换为字符串形式，并将其用做缓存的键

var key = arguments.length + Array.prototype.join.call((arguments,","));

if(key in cache) return cache[key];

else return cache[key] = f.apply(this, arguments);

}

}

//返回两个整数的最大公约数

//使用欧几里德算法

function gcd(a,b) { //这里省略对a和b的类型检查

var t; //临时变量用来存储交换数值

if(a&lt;b) t=b,b=a,a=t; //确保a〉=b

while(b != 0) t=b, b=a%b,a=t; //这是求最大公约数的欧几里算法

return a;

}

var gcdmemo=memorize(gcd);

// console.log(gcdmemo(85, 187));

//注意，当我们写一个递归函数时，往往需要实现记忆功能

//我们更希望调用实现了记忆功能的递归函数，而不是原递归函数

var factorial = memorize(function (n) {

return (n &lt;= 1) ? 1 : n * factorial(n-1);

});

console.log(factorial(5)); //=〉120.对于4一1的值也有缓存

console.log(factorial(2));

</code></pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
<h1 id="类和模块">类和模块</h1>
<h3 id="类和原型">类和原型</h3>
<pre><code>
function inherit(p) {

if (p == null) throw TypeError(); //p是一个对象，但不能是null

if (Object.create) //如果Object.create()存在

return Object.create(p); //直接使用它

var t = typeof p; //否则进行进一步检测

if (t !== "object" &amp;&amp; t !== "function") throw TypeError();

function f() {}; //定义一个空构造函数

f.prototype = p; //将其原型属性设置为p

return new f(); //使用f()创建p的继承对象

}

// 例9.1：一个的单的JavaScript类

//这个工厂方法返回一个新的"范围对象"

function range(from,to) {

//使用inherit()函数来创建对象．这个对象继承自在下面定义的原型对象

//原型对象作为函数的一个属性存储，并定义所有"范围对象"所共享的方法（行为）

var r = inherit(range.methods);

//存储新的"范围对象"的起始位置和结束位置（状态）

//这两个属性是不可承的，每个对象都有唯一的属性

r.from = from;

r.to = to;

//返回这个新创建的对象

return r;

}

//原对象定又方法，这些方法为每个范围对象所继承

range.methods = {

//这个方法可以比较数字范围．也可以比较宇符串和日期范围

includes: function (x) {

return this.from &lt;=x &amp;&amp; x &lt;= this.to;

},

//对于范围内的每个整数都倜调用一次f

//这个方法只可用做数字范围

foreach: function (f) {

for (var x = Math.ceil(this.from); x &lt;= this.to; x++) f(x);

},

//返回表示这个范围的字符串

toString: function () {

return "(" + this.from + "..." + this.to + ")";

}

};

var r = range(1,3);

console.log(r.includes(2));

r.foreach(console.log);

console.log(r);

</code></pre>
<h3 id="类和构造函数">类和构造函数</h3>
<pre><code>
//这是一个构造函数，用以初始化新创建的"范围对象"

//注意，这里并没有创建井返回一个对象，仅仅是初始化

function Range(from,to){

//存储的"范围对象"的起始位置和结束位置（状态）

//这两个属性是不可继承的每个对象都拥有唯一的風性

this.from = from;

this.to = to;

}

//所有的"范围对象"继承自这个对象

//注意，属性的名字必頃是protorype

Range.prototype = {

includes: function(x) { return this.from &lt;= x &amp;&amp; x &lt;= this.to; },

foreach: function(f) {

for(var x = Math.ceil(this.from); x &lt;= this.to; x++) f(x);

},

toString: function() { return "(" + this.from + "..." + this.to + ")"; }

};

var r = new Range(1,3);

console.log(r.includes(2));

r.foreach(console.log());

console.log(r);

</code></pre>
<ul>
<li><strong>构造函数和类的标识</strong></li>
</ul>
<p><code>console.log(r instanceof Range);</code></p>
<p>实际上instanceof运算符并不会检查r是否是由Range()构造函数初始化而来，而会栓查r是否继承自Range.prototype.不过，instanceof的语法则强化了“构造函数是类的公有标识”的溉念。</p>
<ul>
<li><strong>constructor属性</strong></li>
</ul>
<pre><code>
var F = function(){}; //这是一个函数对象

var p = F.prototype; //这是F相关联的原对象

var c = p.constructor; //这是与原型相关联的函数

console.log(c === F); //true：对于任意函数F.prototype.constructor==F

var o = new F(); //创建类F的一个对象

console.log(o.constructor === F); //true.constructor属性指代这个类

</code></pre>
<p>显式给原型添加一个构造函数：</p>
<pre><code>
Range.prototype = {

constructor: Range, //显示设置构造函数反向引用

includes: function(x) { return this.from &lt;= x &amp;&amp; x &lt;= this.to; },

foreach: function(f) {

for(var x = Math.ceil(this.from); x &lt;= this.to; x++) f(x);

},

toString: function() { return "(" + this.from + "..." + this.to + ")"; }

}

</code></pre>
<p>另一种常见的解决办法是使用预定义的原型对象，预定义的原型对象包含constructor属性，然后依次给原型对象舔加方法：</p>
<pre><code>
//扩展预定义的Range.prototype对象．而不重写之

//这样就自动创建Range.prototype.constructor属性

Range.prototype.includes = function(x) { return this.from &lt;= x &amp;&amp; x &lt;= this.to; };

Range.prototype.foreach = function(f) {

for(var x = Math.ceil(this.from); x &lt;= this.to; x++) f(x);

};

Range.prototype.toString = function() { return "(" + this.from + "..." + this.to + ")";};

</code></pre>
<ul>
<li><strong>JavaScipt中Java式的类继承</strong></li>
</ul>
<p>在JavaScript中定义类的步骤可以缩减为一个分三步的算法：</p>
<p>第一步，先定义一个构造函数。并设置初始化新对象的实例属性。</p>
<p>第二步，给构造函数的prototype对象定义实例的方法。</p>
<p>第三步，给构造函数定义类字段和类属性。</p>
<pre><code>
Object.defineProperty(Object.prototype,

"extend", // 定义 Object.prototype.extend

{

writable: true,

enumerable: false, // 将其定义为不可枚举的

configurable: true,

value: function(o) { // 值就是这个函数

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

//一个用以定义简单类的函数

function defineClass(constructor, //用以设置实例的属性的函数

methods, //实例的方法．复制至原型中

statics) //属性，复制至构造函数中

{

if(methods) extend(constructor.prototype, methods);

if(statics) extend(constructor, statics);

return constructor;

}

//这是Range类的另一个实现

var SimpleRange =

defineClass(function (f,t) {this.f = f; this.t = t;},

{

includes: function(x) { return this.f &lt;= x &amp;&amp; x &lt;= this.t; },

toString: function() { return "(" + this.f + "..." + this.t + ")"; }

},

{upto: function(t) {return new SimpleRange(o, t);}});

</code></pre>
<pre><code>
/*

*这个文件定义了Complex类，用来描述复数

*实例字段

*/

function Complex(real, imaginary) {

if (isNaN(real) || isNaN(imaginary))

throw new TypeError();

this.r = real;

this.i = imaginary;

}

Complex.prototype.add = function(that) {

return new Complex(this.r + that.r, this.i + that.i);

};

Complex.prototype.mul = function(that) {

return new Complex(this.r * that.r - this.i * that.i,

this.r * that.i + this.i * that.r);

};

Complex.prototype.mag = function() {

return Math.sqrt(this.r*this.r + this.i*this.i);

};

Complex.prototype.neg = function() { return new Complex(-this.r, -this.i); };

Complex.prototype.toString = function() {

return "{" + this.r + "," + this.i + "}";

};

Complex.prototype.equals = function(that) {

return that != null &amp;&amp;

that.constructor === Complex &amp;&amp;

this.r === that.r &amp;&amp; this.i === that.i;

};

/*

* 类字段（比如常量）和类方法直接定义为构造函数的属性

* 需要注意的是，类的方法通常不使用关键字this，

* 它们只对其参数进行操作

*/

// 这里預定义了一些对复数运算有帮助的类字段

// 它们的命名全都是大写，用以表明它们是常量

// (在ECMAScript 5中, 还能设置这些字段的属性为只读)

Complex.ZERO = new Complex(0,0);

Complex.ONE = new Complex(1,0);

Complex.I = new Complex(0,1);

//这个类方法将由实例对象的toStrig方法返回的字符串格式解析为一个Complex对象

//或者抛出一个错误异常

Complex.parse = function(s) {

try {

var m = Complex._format.exec(s);

return new Complex(parseFloat(m[1]), parseFloat(m[2]));

} catch (x) {

throw new TypeError("Can't parse '" + s + "' as a complex number.");

}

};

//定义类的"私有"字段，这个字段在Complex.parse中用了

//下划线前缀表明它是类内部使用的，而不属于类的公有API的部分

Complex._format = /^\{([^,]+),([^}]+)\}$/;

var c = new Complex(2,3);

var d = new Complex(c.i,c.r);

console.log(c.add(d).toString());

console.log(Complex.parse(c.toString()).add(c.neg()).equals(Complex.ZERO));

</code></pre>
<h3 id="类的扩充">类的扩充</h3>
<pre><code>
//多次调用这个函数f,传入一个迭代数

//比如，要输出“Hello“三次：

Number.prototype.times = function (f, context) {

var n = Number(this);

for(var i = 0; i &lt; n; i++) f.call(context, i);

};

var n = 3

n.times(function(n){console.log(n + " hello");});

//这个方法用以去除字符串开头和结尾的空格

String.prototype.trim = String.prototype.trim || function () {

if(!this) return this;

return this.replace(/^\s|\s+$/g,""); //使用正则表达式进行空格替换

}

</code></pre>
<h3 id="类和类型">类和类型</h3>
<ul>
<li><strong>instanceof运算符</strong></li>
</ul>
<p>构造函数是类的公共标识，但原型是唯一的标识</p>
<p><code>range.methods.isPrototypeOf(r); //range.method 是原型对象</code></p>
<p>instanceof运算符和isPrototypeOf()方法的缺点是，我们无法通过对象来获得类名，只能检测对象是否属于指定的类名。</p>
<ul>
<li><strong>constructor属性</strong></li>
</ul>
<pre><code>
function typeAndValue(x) {

if(x == null) return "";

switch(x.constructor){

case Number: return "Number: " + x;

case String: return "String: '" + x "'";

case Date: return "Date: " + x;

case RegExp: return "Regexp: " + x;

case Complex: return "Complex: " + x;

}

}

</code></pre>
<p>使用constructor属性检测对象属于某个类的技术的不足之处和instanceof一样。在多个执行上下文的场景中它是无法正常工作的（比如在浏览器窗口的多个框架子页面中）。在这种情况下，每个框架页面各自拥有独立的构造函数集合，一个框架页面中的Arraay构造函数和另一个框架页面的Array构造函数不是同一个构造函数。</p>
<ul>
<li><strong>构造函数的名称</strong></li>
</ul>
<pre><code>
/**

*以字符串形式返回o的类型：

* - 如果o是null，返回"null",如果o是NaN,返回"nan"

* - 如果typeof所返回的值不是"object",则返回这个值

* - （注意，有一些Javascript的实现将正则表达式识别为函数）

* - 如果o的类不是"Object"，则返回这个值

* - 如果o包含构造函数并且这个构造函数具有名称，则返回这个名称

* - 否则，一律返回"Object"

**/

function type(o) {

var t, c, n; // type, class, name

// 处理null值的特殊情形

if (o === null) return "null";

// 另外一种特殊情形：NaN和它自身不相等

if (o !== o) return "nan";

// 如果typeof的值不是"object"，则使用这个值

// 这可以识别出原始值的类型和函数

if ((t = typeof o) !== "object") return t;

// 返回对象的类名，除非值为"object"

// 这种方式可以识别出大多数的内置对象

if ((c = classof(o)) !== "Object") return c;

// 如果对象构造函数的名字存在的话，则返回它

if (o.constructor &amp;&amp; typeof o.constructor === "function" &amp;&amp;

(n = o.constructor.getName())) return n;

// 其他的类型都无法判别，一律返回"object"

return "Object";

}

// 返回对象的类

function classof(o) {

return Object.prototype.toString.call(o).slice(8,-1);

};

// 返回函数的名字（可能是空字符串），不是函数的话返回null

Function.prototype.getName = function() {

if ("name" in this) return this.name;

return this.name = this.toString().match(/function\s*([^(]*)\(/)[1];

};

</code></pre>
<ul>
<li><strong>鸭式辩型</strong></li>
</ul>
<pre><code>
//例9-5：利用鸭式辩型实现的函数

//如果o实现了除第一个参数之外的参数所表示的方法，则返回true

function quacks(o /*,...*/) {

for (var i = 1; i &lt; arguments.length; i++) { //遍历o之后的所有参数

var arg = arguments[i];

switch (typeof arg) { //如果参数是：

case 'string': //string:直接用名字做检查

if (typeof o[arg] !== "function") return false;

continue;

case 'function': //function：检查函数的原型对象上的方法

//如果实参是函数，则使用它的原型

arg = arg.prototype; //进人下一个case

case 'object': //object:检查匹配的方法

for (var m in arg) { // 遍历对象的每个属性

if(typeof arg[m] !== "function") continue; //跳过不是方法的属性

if(typeof o[m] !== "function") return false;

}

}

}

//如果程序能执行到这里，说明o实现了所有的方法

return true;

}

</code></pre>
<pre><code>
//例9-5：利用鸭式辩型实现的函数

//如果o实现了除第一个参数之外的参数所表示的方法，则返回true

function quacks(o /*,...*/) {

for (var i = 1; i &lt; arguments.length; i++) { //遍历o之后的所有参数

var arg = arguments[i];

switch (typeof arg) { //如果参数是：

case 'string': //string:直接用名字做检查

if (typeof o[arg] !== "function") return false;

continue;

case 'function': //function：检查函数的原型对象上的方法

//如果实参是函数，则使用它的原型

arg = arg.prototype; //进人下一个case

case 'object': //object:检查匹配的方法

for (var m in arg) { // 遍历对象的每个属性

if(typeof arg[m] !== "function") continue; //跳过不是方法的属性

if(typeof o[m] !== "function") return false;

}

}

}

//如果程序能执行到这里，说明o实现了所有的方法

return true;

}

</code></pre>
<p>不能通过quacks(o,Array)来检测o是否实现了Array中所有同名原因是内不可枚举的，quacks()中的for/in循坏无法遍历到它们（注意，在ECMAScript5中有一</p>
<p>个补救办法，就是使用Ojbect.getOwnPropertyNames())</p>
<h3 id="javascript中的面向对象技术">JavaScript中的面向对象技术</h3>
<ul>
<li><strong>一个例子：集合类</strong></li>
</ul>
<pre><code>
//例9-6：Set.js：值的任意集合

function Set() {//这是一个构造函数

this.values = {};//集合数据保存在对象的属性里

this.n = 0;//集合中值的个数

this.add.apply(this, arguments);//把所有参数都添加进这个集合

}

//将每个参数都添加至集合中

Set.prototype.add = function () {

for (var i = 0; i &lt; arguments.length; i++){//遍历每个参数

var val = arguments[i];//待添加到集合中的值

var str = Set._v2s(val);//把它转换为字符串

if(!this.values.hasOwnProperty(str)){//如果不在集合中

this.values[str] = val;//将字符串和值对应起来

this.n++;//集合中值的计数加一

}

}

return this;//支持链式方法调用

}

//从集合删除元素，这些元素由参数指定

Set.prototype.remove = function () {

for(var i = 0; i &lt; arguments.length; i++){

var str = Set._v2s(arguments[i]);

if(this.values.hasOwnProperty(str)){

delete this.values[str];

this.n--;

}

}

return this;

}

//如果集合包含这个值，则返回true;否则，返回false

Set.prototype.contains = function (value) {

return this.values.hasOwnProperty(Set._v2s(value));

}

//返回集合的大小

Set.prototype.size = function () {

return this.n;

}

//遍历集合中的所有元素，在指定的上下文中调用f

Set.prototype.foreach = function (f, context) {

for(var s in this.values)

if(this.values.hasOwnProperty(s))

f.call(context, this.values[s]);

};

//这是一个内部函数，用以将任意JavaScript值和唯一的字符串对应起来

Set._v2s = function (val) {

switch (val){

case undefined: return 'u';//特殊的原始值

case null: return 'n';//值只有一个字母

case true: return 't';//代码

case false: return 'f';

default: switch (typeof val){

case 'number': return '#' + val;//数字都带有#前缀

case 'string': return '"' +val;//字符串都带有"前缀

default: return '@' + objectId(val);//Objs and funcs get @

}

//对任意对象来说，都会返回一个字符申

//针对不同的对象，这个函数会返回不同的字符串

//对于同一个对象的多次调用，总是返回相同的字符串

//为了做到这一点，它给o创建了一个属性，在ES5中，这个属性是不可枚举且是只读的

function objectId(o) {

var prop = "|**objectid**|";//私有属性，用以存放id

if(!o.hasOwnProperty(prop))//如果对象没有id

o[prop] = Set._v2s.next++;//将下一个值赋给它

return o[prop];

}

}

};

Set._v2s.next = 100; //设置初始id的值

</code></pre>
<ul>
<li><strong>例9-7：JavaScript中的枚举类型</strong></li>
</ul>
<pre><code>
function inherit(p) {

if (p == null) throw TypeError();

if (Object.create)

return Object.create(p);

var t = typeof p;

if (t !== "object" &amp;&amp; t !== "function") throw TypeError();

function f() {};

f.prototype = p;

return new f();

}

//这个函数创建一个新的枚举类型，实参对象表示类的每个实例的名字和值

//返回值是一个构造函数，它标识这个新类

//注意，这个构造函数也会抛出异常：不能使用它来创建该类型的新实例

//返回的构造函数包含名/值对的映射表

//包括由值组成的数组，以及一个foreach()迭代器函数

function enumeration(namesToValues) {

//这个虚拟的构造函数是返回值

var enumeration = function (){throw "Can't Instantiate Enumerations";};

//枚举值继承自这个对象

var proto = enumeration.prototype = {

constructor: enumeration, //标识类型

toString: function(){ return this.name;}, //返回名字

valueOf: function() { return this.value;}, //返回值

toJSON: function() {return this.name;} //转换为JSON

};

enumeration.values = [];//用以存放枚举对象的数组

//现在创建新类型的实例

for (name in namesToValues){

var e = inherit(proto);//创建一个代表它的对象

e.name = name;

e.value = namesToValues[name];

enumeration[name] = e;//将它设置为构造函数的属性

enumeration.values.push(e);//将它存储到值数组中

}

//一个类方法，用来对类的实例进行迭代

enumeration.foreach = function (f,c) {

for(var i = 0; i &lt; this.values.length; i++) f.call(c,this.values[i]);

};

//返回标识这个新类型的构造函数

return enumeration;

}

var Coin = enumeration({Penny:1, Nickel:5, Dime:10, Quarter:25});

var c = Coin.Dime;

console.log(c);

console.log(c instanceof Coin);

console.log(Coin.Quarter + 3 * Coin.Nickel);

console.log(Coin.Dime == 10);

console.log(Coin.Dime &gt; Coin.Nickel);

console.log(String(Coin.Dime) + ":" + Coin.Dime);

</code></pre>
<ul>
<li>
<p><strong>例9-8：使用枚举类型来表示一副扑克牌</strong></p>
</li>
<li>
<p><strong>标准转换方法</strong></p>
</li>
</ul>
<pre><code>
/*

* Copy the enumerable properties of p to o, and return o.

* If o and p have a property by the same name, o's property is overwritten.

* This function does not handle getters and setters or copy attributes.

*/

function extend(o, p) {

for(prop in p) {

o[prop] = p[prop];

}

return o;

}

//注意extend()函数（例6-2）的用法，这里使用extend()来向Set.prototype来添加方法：

//将这些方法添加至Set类的原型对象中

extend(Set.prototype,{

toString: function () {

var s = "{",

i = 0;

this.foreach(function (v) {

s += ((i++&gt;0) ? ", " : "") + v; });

return s + "}";

},

//类似toString,但是对于所有的值都将调用toLocaleString（）

toLocaleString: function () {

var s = "{",

i = 0;

this.foreach(function (v) {

s += ((i++ &gt; 0) ? ", " : "");

if (v == null) s += v; //null 和 undefined

else s += v.toLocaleString();

});

return s + "}";

}

toArray: function () {

var a = [];

this.foreach(function (v) {

a.push(v);

});

return a;

}

});

//对于要从JSON转换为字符串的集合都被当做数组来对待

Set.prototype.toJSON = Set.prototype.toArray();

</code></pre>
<ul>
<li><strong>比较方法</strong></li>
</ul>
<pre><code>
function Range(from,to){

this.from = from;

this.to = to;

}

//Range类重写它的constructor属性，现在将它添加进去

Range.prototype.constructor = Range;

//一个Range对象和其他不是Range的对象均不相等

//当且仅当两个范围的端点相等, 它们才相等

Range.prototype.equals = function (that) {

if (that == null) return false; //处理null和undefined

if (that.constructor !== Range) return false; //处理非Range对象

//当且仅当两个端点相等．才返回true

return this.from == that.from &amp;&amp; this.to == that.to;

}

</code></pre>
<pre><code>
Set.prototype.equals = function(that){

//一些次要情况的快捷处理

if (this === that) return true;

//如果that对象不是一个集合．它和this不相等

//我们用到了instanceof.使得这个方法可以用于Set的任何子类

//如果希望采用鸭式辩型的方法，可以降低检查的严格程度

//或者可以通过this.constructor==that.constructor加强检查的严格程度

//注意，nu11和undefined两个是无法用于instanceof运算的

if (!(that instanceof Set)) return false;

//如果两个集合的大小不一样，如它们不相等

if (this.size() != that.size()) return false;

//在查两个集合中的元素是否完全一样

//如果两个集合不相等，则通过抛出异常来终止foreach循环

try{

this.foreach(function(v) {if (!that.contains(v)) throw false});

return true; //所有的元素匹配：两个集合相等

} catch (x){

if (x === false) return false; //如果集合中有元素在另外一个集合中不存在

throw x; //重新抛出异常

}

};

</code></pre>
<pre><code>
//根据下边界对Range对象推序，如下边界相等比较上边界

//如传入非Range值，则抛出异常

//当且仅当this.equals(that)时才返回0

Range.prototype.compareTo = function (that) {

if (!that instanceof Range)

throw new Error("Can't compare a Range with " + that);

var diff = this.from - that.from;

if (diff == 0) diff = this.to - that.to;

return diff;

}

</code></pre>
<pre><code>
ranges.sort(function (a, b) {

return a.compareTo(b);

});

Range.byLowerBound = function (a, b) {

return a.compareTo(b);

};

ranges.sort(Range.byLowerBound);

</code></pre>
<ul>
<li><strong>方法借用</strong></li>
</ul>
<p>把一个类的方法用到其他的中的做法也称做“多重继承”(multiple inheritance)。然而，JavaScript并不是经典的面向对象语言，我更倾向于将这种方法重用更正式地称为“方法借用“(borrowing)。</p>
<p>不仅Array的方法可以借用，还可以自定义泛型方(generic method)。</p>
<p>例9-9：方法借用的泛型实现</p>
<pre><code>
var generic = {

//返回一个字符串．这个字符串包含构造函数的名字（如果构造函数包含名字）

//以及所有非继承来的．非函数属性的名字和值

toString: function(){

var s = "[";

//如果这个对象包含构造函数,且构造数包含名字

//这个名字会作为返回字符窜的一部分

//需注意的是．函数的名字属性是非标准的,并不是在所有的环境中都可用

if (this.constructor &amp;&amp; this.constructor.name)

s += this.constructor.name + ": ";

//枚举所有非继承且非函数的属性

var n = 0;

for (var name in this){

if (!this.hasOwnProperty(name)) continue; //跳过继承来的属性

var value = this[name];

if (typeof value === "function") continue; //跳过方法

if (n++) s += ", ";

s += name + '=' + value;

}

return s + "]";

},

//通过比较this和that的构造函数和实例属性柬判断它们是否相等

//这种方法只适合于那些实例属性是原始值的情况．原始值可以通过"==="来比较

//这里还处理一种特殊情况，就是忽略由Set类添加的特殊属性

equals: function(that){

if (that == null) return false;

if (this.constructor !== that.constructor) return false;

for (var name in this){

if(name === "|**objectid**|") continue; //跳过特殊属性

if(!this.hasOwnProperty(name)) continue; //跳过承来的属性

if(this[name] !== that[name]) return false; //比较是否相等

}

return true; //如果所有属性都匹配,两个对象相等

}

};

</code></pre>
<ul>
<li><strong>私有状态</strong></li>
</ul>
<p>我们可以通过将变量（或参数）闭包在一个构造函数内来模拟实现私有实例字段，调用构造函数会创建一个实例。为了做到这一点，需要在构造函数内部定义一个函数（因此这个函数可以访问构造函数内部的参数和变量），并将这个函数賦值给新创建对象的属性。</p>
<pre><code>
//例9-1O：对Range类的读取端点方法的简单封装

function Range(from, to) {

//不要将端点保存为对象的属性，相反

//定义存取器函数返回端点的值

//这些值都保存在闭包中

this.from = function() {return from;};

this.to = function() {return to;};

}

//原型上的方法无法直接操作端点

//它们必调用存取方法

Range.prototype = {

constructor: Range,

includes: function (x) {

return this.from() &lt;= x &amp;&amp; x &lt;= this.to();

},

foreach: function (f) {

for (var x = Math.ceil(this.from()), max = this.to(); x &lt;= max; x++) f(x);

},

toString: function() { return "(" + this.from() + "..." + this.to() + ")";}

};

</code></pre>
<pre><code>
var r = new Range(1,5); //一个不可改的范围

r.from = function () { //通过方法替换来修改它

return 0;

};

</code></pre>
<ul>
<li><strong>构造函数的重载和工厂方法</strong></li>
</ul>
<pre><code>
function isArrayLike(o)

{

if (o &amp;&amp; //o非null、undefined等

typeof o === "object" &amp;&amp; //o是对象

isFinite(o.length) &amp;&amp; //o.length是有限数值

o.length &gt;= 0 &amp;&amp; //o.length为非负值

o.length === Math.floor(o.length) &amp;&amp; //o.length是整数

o.length &lt; 4294967296) //o.length &lt; 2^32

return true; //o是类数组对象

else

return false; //否则它不是

}

function Set() {

this.values = {}; //用这个对象的属性来保存这个集合

this.n = 0; //集合中值的个数

//如传入一个类数组的对象，将这个元素添加至集合中

//否则将所有的参数都加至集合中

if (arguments.length == 1 &amp;&amp; isArrayLike(arguments[0]))

this.add.apply(this, arguments[0]);

else if (arguments.length &gt; 0)

this.add.apply(this, arguments);

}

</code></pre>
<p><strong>工厂方法</strong></p>
<pre><code>
Complex.polar = function (r, theta) {

return new Complex(r*Math.cos(theta), r*Math.sin(theta));

};

Set.fromArray = function (a) {

s = new Set(); //创建一个空集合

s.add.apply(s,a); //将数组a的成员作为参数传人add()方法

return s; //返回这个新集合

}

</code></pre>
<p>可以给工厂方法定义任意的名字，不同名字的工厂方法用以执行不同的初始化。但由于构造函数是类的公有标识，因此每个类只能有一个构造函数。但这并不是一个“必须遵守“的规则。中是可以定义多个构造函数继承自一个原型对象的，如果这样做的话，由这些构造函数的任意一个所创建的对象都属于同一类型。</p>
<pre><code>
//Set类的一个辅助构造函数

function SetFromArray(a) {

//通过以函数的形式用Set()柬初始化这个新对象

//将a的元素作为参数传人

Set.apply(this,a);

}

//设置原型，以便SetFromArray能创建Set的实例

SetFromArray.prototype = Set.prototype;

var s = new SetFromArray([1,2,3]);

console.log(s instanceof Set);

</code></pre>
<h3 id="子类">子类</h3>
<ul>
<li><strong>定义子类</strong></li>
</ul>
<pre><code>
B.prototype = inherit(A.prototype); //子类派生自父类

B.prototype.constructor = B; //重载继承来的constructor属性

</code></pre>
<pre><code>
//例9-11：定义子类

//用一个简单的函数创建筒单的子类

function defineSubclass(superclass, //父类的构造函数

construcotr, //新的子类的构造函数

methods, //实例方法：复制至原型中

statics) { //类属性：复制至构造函数中

// 建立子类的原型对象

construcotr.prototype = inherit(superclass.prototype);

construcotr.prototype.constructor = construcotr;

//像对常规类一样复制方法和类属性

if(methods) extend(construcotr.prototype, methods);

if(statics) extend(construcotr, statics);

//返回这个类

return construcotr;

}

// 也可以通过父类构造函数的方法做到这一点

Function.prototype.extend = function (constructor, methods, statics) {

return defineSubclass(this, constructor, methods, statics);

};

</code></pre>
<pre><code>
//SingletonSet是一个特殊的集合，它是只读的，而且含有单独的常量成员。

//构造函数

function SingletonSet(member) {

this.member = member; //记住集合中这个唯一的成员

}

//创建一个原型对象，这个原型对象继承自Set的原型

SingletonSet.prototype = inherit(Set.prototype);

//给原型添加属性

//如果有同名的属性就覆盖Set.prototype中的同名属性

extend(SingletonSet.prototype,{

//设置合适的constructor属性

constructor: SingletonSet,

//这个集合是只读的:倜用add()和remove()都会报错

add: function() {throw "read-only set";},

remove: function() {throw "read-only set";},

//SingletonSet的实例中永远只有一个元素

size: function() { return 1;},

//这个方法只调用一次．传人这个集合的唯一成员

foreach: function(f, context) { f.call(context, this.member);},

//contains()方法非常简单：只需要传人的值是否匹配这个集合唯一的成员即可

contains:function (x) { return x === this.member;}

});

</code></pre>
<ul>
<li><strong>构造函数和方法链</strong></li>
</ul>
<p>例9-13：在子类中调用父类的构造函数和方法</p>
<pre><code>
/*

*NonNullSet 是Set的子类，它的成员不能是null和undefined

*/

function NonNullSet() {

//仅链接到父类

//作为普通函数调用父类的构造函数来初始化通过该构造函数调用创建的对象

Set.apply(this, arguments);

}

//将NonNullSet设置为set的子类

NonNullSet.prototype = inherit(Set.prototype);

NonNullSet.prototype.constructor = NonNullSet;

//为了将null和undefined排除在外，只须重写add()方法

NonNullSet.prototype.add = function () {

//检查参数是不是null或undefined

for(var i = 0; i &lt; arguments.length; i++)

if(arguments[i] == null)

throw new Error("Can't add null or undefined to a NonNullSet");

//调用父类的add()方法以执行实际插人操作

return Set.prototype.add.apply(this, arguments);

};

</code></pre>
<pre><code>
//例9-14：类工厂和方法链

/*

*这个函数返回具体Set类的子类

*并重写该类的add()方法用以对添加的元素做特殊的过滤

*/

function filteredSetSubclass(superclass, filter) {

var constructor = function () { //子类构造函数

superclass.apply(this, arguments); //调用父类构造函数

};

var proto = constructor.prototype = inherit(superclass.prototype);

proto.constructor = constructor;

proto.add = function () {

//在添加任何成员之前首先使用过滤器将所有参数进行过滤

for(var i = 0; i &lt; arguments.length; i++){

var v = arguments[i];

if (!filter(v)) throw("value " + v + " rejected by filter");

}

//调用父类的add()方法

superclass.prototype.add.apply(this, arguments);

}

return constructor;

}

//定义一个只能保存字符串的"集合"类

var StringSet = filteredSetSubclass(Set, function (x) {

return typeof x === "string";

});

//这个集合类的成员不能是null、undefined或函数

var MySet = filteredSetSubclass(NonNullSet, function (x) {

return typeof x !== "function";

});

</code></pre>
<pre><code>
var NonNullSet = (function () {

var superclass = Set;

return superclass.extend(

function(){superclass.apply(this, arguments);},

{

add:function () {

for(var i = 0; i &lt; arguments.length; i++)

if(arguments[i] == null)

throw new Error("Can't add null or undefined to a NonNullSet");

return superclass.prototype.add.apply(this, arguments);

}

});

)

}());

</code></pre>
<ul>
<li><strong>组合vs子类</strong></li>
</ul>
<p>“组合优于继承”</p>
<pre><code>
//例9-15：使用组合代替继承的集合的实现

/*

*实现一个FilteredSet，它包装某个指定的"集合"对象，

*并对传人add()方法的值应用了某种指定的过滤器

*"范围"类中其他所有的核心方法延续到包装后的实例中

*/

var FilteredSet = Set.extend(

function FilteredSet(set, filter){ //构造函数

this.set = set;

this.filter = filter;

},

{ //实例方法

add: function(){

//如果已有过滤器，直接使用它

if(this.filter){

for (var i = 0; i &lt; arguments.length; i++){

var v = arguments[i];

if(!this.filter(v))

throw new Error("FilteredSet: value " + v + " rejected by filter");

}

}

//调用set中的add()方法

this.set.add.apply(this.set, arguments);

return this;

},

//剩下的方法都保持不变

remove: function() {

this.set.remove.apply(this.set, arguments);

return this;

},

contains: function(v) {return this.set.contains(v);},

size: function() {return this.set.size();},

foreach: function(f, c) {this.set.foreach(f, c);}

});

var s = new FilteredSet(new Set(), function (x) {

return x !== null;

});

var t = new FilteredSet(s,{function(x) { return !(x instanceof Set);}});

</code></pre>
<ul>
<li><strong>类的层次结构和抽象类</strong></li>
</ul>
<h3 id="ecmascript-5中的类">ECMAScript 5中的类</h3>
<ul>
<li><strong>让属性不可枚举</strong></li>
</ul>
<pre><code>
//9-17 定又不可枚举的属性

//将代码包装在一个匿名函数中，这样定义的变量就在这个函数作用域内

(function () {

//定义一个不可枚举的属性objectId, 它可以被所有对象继承

//当读取这个属性时调用getter函数

//它没有定又setter，因此它是只读的

//它是不可配置的，因此它是不能删除的

Object.defineProperty(Object.prototype, "objectId", {

get: idGetter, //取值器

enumerable: false, //不可枚举的

configurable: false,//不可删除的

});

//当读取objectId时候直接调用这个getter函数

function idGetter() { //getter函数返回该id

if (!(idprop in this)) { //如果对象中不存在id

if (!Object.isExtensible(this)) //并且可以增加属性

throw Error("Can't define id for nonextensible objects");

Object.defineProperty(this, idprop, { //给它一个值

value: nextid++,

writable: false,

enumerable: false,

configuable: false,

});

return this[idprop];

}

};

//idGetter()用到了这些变量，这些都属于私有变量

var idprop = "|**objectId**|"; //假设这个属性没有用到

var nextid = 1; //给它设置初始值

}());

</code></pre>
<ul>
<li><strong>定义不可变的类</strong></li>
</ul>
<p>例9·18展示了一个有趣的技巧，其</p>
<p>中实现的构造函数也可以用做工厂函数，这样不论调用函数之前是否带有new关键字，</p>
<p>都可以正确地创建实例。</p>
<pre><code>
//例9·18．创建一个不可变的类，它的属性和方法都是只读的

//这个方法可以使用new调用，也可以省略new，它可以用做构造函数也可以用做工厂数

function Range(from, to) {

//这些是对from和to只读属性的描述符

var props = {

from:{value:from,enumerable:true,writable:false,configurable:false},

to:{value:to, enumerable:true,writable:false,configurable:false}

};

if(this instanceof Range) //如果作为构造函数来调用

Object.defineProperties(this,props); //定义属性

else //否则，作为工厂方法来调用

return Object.create(Range.prototype, props); //创建并返回这个新Range对象，属性由props指定

}

//如果用同样的方法给Range·prototype对象添加属性

//那么我们需要给这些属性设置它们的特性

//因为我们无法识别出它们的可枚举性、可写性或可配置性，这些属性特性默认都是false

Object.defineProperties(Range.prototype,{

includes: {

value: function(x) { return this.from &lt;= x &amp;&amp; x &lt;= this.to; }

},

foreach: {

value: function(f) {

for(var x = Math.ceil(this.from); x &lt;= this.to; x++) f(x);

}

},

toString: {

value: function() { return "(" + this.from + "..." + this.to + ")"; }

}

});

</code></pre>
<pre><code>
//例9·19：属性描述符工具函数

//将o的指定名字（或所有）的属性设置为不可写的和不可配置的

function freezeProps(o) {

var props = (arguments.length == 1) //如果只有一个参数

? Object.getOwnPropertyNames(o) //使用所有的属性

: Array.prototype.splice.call(arguments, 1);//否则传人了指定名字的属性

props.forEach(function (n) { //将它们都设置为只读的和不可变的

////忽略不可配置的属性

if (!Object.getOwnPropertyDescriptor(o, n).configurable) return;

Object.defineProperty(o, n, {writable:false, configurable:false});

});

return o; //所以我们可以继续使用它

}

//将o的指定名字（或所有）的属性设置为不可枚举的和可配置的

function hideProps(o) {

var props = (arguments.length == 1)

? Object.getOwnPropertyNames(o)

: Array.prototype.splice.call(arguments, 1);

props.forEach(function (n) {

//忽略不可配置的属性

if (!Object.getOwnPropertyDescriptor(o, n).configurable) return;

Object.defineProperty(o, n, {enumerable:false});

});

return o;

}

</code></pre>
<pre><code>
//例9·20：一个简单的不可变的类

function Range(from, to) { //不可变的类Range的构造函数

this.from = from;

this.to = to;

freezeProps(this); //将属性设置为不可变的

}

Range.prototype = hideProps({ //使用不可枚举的属性来定义原型

constructor: Range,

includes: function (x) {

return this.from &lt;= x &amp;&amp; x &lt;= this.to;},

},

foreach: function(f) {for(var x=Math.ceil(this.from);x&lt;=this.to;x++) f(x);},

toString: function() { return "(" + this.from + "..." + this.to + ")"; }

});

</code></pre>
<ul>
<li><strong>封装对象状态</strong></li>
</ul>
<pre><code>
//例9．21：将Range类的端点严格封装起来

//这个版本的Range类是可变的，但将端点变最进行了良好的封装

//但端点的大小顺序还是固定的：from〈=to

function Range(from, to) {

//如果from大于to

if (from &gt; to) throw new Error("Range: from must be &lt;= to");

//定义存取器方法以维持不变

function getFrom() {return from;}

function getTo() {return to}

function setFrom(f) { //设置from的值时，不允许from大于to

if (f &lt; to) from = f;

else throw new Error("Range: from must be &lt;= to");

}

function setTo(t) { //设置to的值时，不允许to小于from

if (t &gt;= from) to = t;

else throw new Error("Range: to must be &gt;= from");

}

//将使用取值器的属性设置为可枚举的、不可配置的

Object.defineProperties(this,{

from: {get: getFrom, set: setFrom, enumerable:true, configurable:false},

to: { get: getTo, set: setTo, enumerable:true, configurable:false }

});

}

//和前面的例子相比，原型对象没有做任何修改

//实例方法可以像读取普通的属性一样读取from和to

Range.prototype = hideProps({

constructor:Range,

includes: function(x) { return this.from &lt;= x &amp;&amp; x &lt;= this.to; },

foreach: function(f) {for(var x=Math.ceil(this.from);x&lt;=this.to;x++) f(x);},

toString: function() { return "(" + this.from + "..." + this.to + ")"; }

})

</code></pre>
<ul>
<li><strong>防止类的扩展</strong></li>
</ul>
<p>ECMAScript5: Objectbject.preventExtensions()可以将对象设置为不可扩展的，也就是说不能给对象添加任何新属性。</p>
<p>Object.sea1()则更加强大，它除了能阻止用户给对象添加新属性，还能将当前已有的属性设置为不可配置的，这样就不能删除这些属性了（但不可配置的属性可以是</p>
<p>可写的，也可以转换为只读属性）。</p>
<p><code>Object.seal(Object.prototype);</code></p>
<p>JavaScript的另外一个动态特性是“对象的方法可以随时替换"（或称为“monkey-patch"）</p>
<pre><code>
var original_sort_method = Array.prototype.sort;

Array.prototype.sort = function () {

var start = new Date();

original_sort_method.apply(this,arguments);

var end = new Date();

console.log("Array sort took " + (end - start) + " milliseconds.");

};

</code></pre>
<p>可以通过将实例方法设置为只读来防止这类修改，一种方法就是使用上面代码所定义的freezeprops()工具函数。另外一种方法是使用0bject.freeze(),它的功能和0bject·seal()完全一样，它同样会把所有属性都设置为只读的和不可配置的。</p>
<p>Object.freeze(enumeration.values);</p>
<p>Object.freeze(enumeration);</p>
<ul>
<li><strong>子类和ECMAScript5</strong></li>
</ul>
<pre><code>
//例9-22：StingSet：利用ECMAScript5的特性定义的子类

function StringSet(){

this.set = Object.create(null); //创建一个不包含原型的对象

this.n = 0;

this.add.apply(this, arguments);

}

//注意，使用Object·create()可以继承父类的原型

//而且可以定义单独调用的方法，因为我们没有指定属性的可写性、可枚举性和可配置性

//因此这些属性特性的默认值都是false

//只读方法让这个类难于子类化（被继承）

StringSet.prototype = Object.create(AbstractWritableSet.prototype, {

constructor: { value: StringSet },

contains: { value: function(x) { return x in this.set; } },

size: { value: function(x) { return this.n; } },

foreach: { value: function(f,c) { Object.keys(this.set).forEach(f,c); } },

add: {

value: function() {

for(var i = 0; i &lt; arguments.length; i++) {

if (!(arguments[i] in this.set)) {

this.set[arguments[i]] = true;

this.n++;

}

}

return this;

}

},

remove: {

value: function() {

for(var i = 0; i &lt; arguments.length; i++) {

if (arguments[i] in this.set) {

delete this.set[arguments[i]];

this.n--;

}

}

return this;

}

}

});

</code></pre>
<ul>
<li><strong>属性描述符</strong></li>
</ul>
<pre><code>
//例9-23：ECMAScript5属性操作

/*

*给Object.prototype定义properties()方法，

*这个方法返回一个表示调用它的对象上的属性名列表的对象

*（如果不带参数调用它，就表示该对象的所有属性）

*返回的对象定义了4个有用的方法：toString()、descriptors()、hide()和show()

*/

(function namespace() { //将所有逻辑闭包在一个私有函数作用域中

//这个函数成为所有对象的方法

function properties() {

var names; //属性名组成的数组

if (arguments.length == 0) //所有的自有属性

names = Object.getOwnPropertyNames(this);

else if(arguments.length == 1 &amp;&amp; Array.isArray(arguments[0]))

names = arguments[0]; //名字组成的数组

else //参数列表本身就是名字

names = Array.prototype.splice.call(arguments,0);

//返回一个新的Properties对象，用以表示属性名字

return new Properties(this, names);

}

//将它设置为Object．prototype的新的不可枚举的属性

//这是从私有函数作用域导出的唯一一个值

Object.defineProperty(Object.prototype, "properties",{

value: properties,

enumerable:false, writable:true,configurable:true

});

//这个构造函数是由上面的properties()函数所调用的

//Properties类表示一个对象的属性集合

function Properties(o, names) {

this.o = o; //属性所属的对象

this.names = names; //属性的名字

}

//将代表这些属性的对象设置为不可枚举的

Properties.prototype.hide = function () {

var o = this.o, hidden = {enumerable: false};

this.names.forEach(function (n) {

if (o.hasOwnProperty(n))

Object.defineProperty(o,n,hidden);

});

return this;

};

//将这些属性设置为只读的和不可配置的

Properties.prototype.freeze = function () {

var o = this.o, frozen = {writable:false, configurable:false};

this.names.forEach(function (n) {

if(o.hasOwnProperty(n))

Object.defineProperty(o,n,frozen);

});

return this;

};

//返回一个对象，这个对象是名字到属性描述符的映射表

//使用它来复制属性，连同属性特性一起复制

//Object.defineProperties(dest,src.properties().descriptors())；

Properties.prototype.descriptors = function () {

var o = this.o, desc = {};

this.names.forEach(function (n) {

if (!o.hasOwnProperty(n)) return;

desc[n] = Object.getOwnPropertyDescriptor(o,n);

});

return desc;

};

//返回一个格式化良好的属性列表

//列表中包含名字、值和属性特性，使用"permanent"表示不可配置

//使用"readonly"表示不可写，使用"hden"表示不可枚举

//普通的可枚举、可写和可配置属性不包含特性列表

Properties.prototype.toString = function () {

var o = this.o; //在下面嵌套的函数中使用

var lines = this.names.map(nameToString);

return "{\n " + lines.join(",\n ") + "\n}";

function nameToString(n) {

var s = "", desc = Object.getOwnPropertyDescriptor(o, n);

if (!desc) return "nonexistent " + n + ": undefined";

if (!desc.configurable) s += "permanent ";

if ((desc.get &amp;&amp; !desc.set) || !desc.writable) s += "readonly ";

if (!desc.enumerable) s += "hidden ";

if (desc.get || desc.set) s += "accessor " + n;

else s += n + ": " + ((typeof desc.value==="function")?"function"

:desc.value);

return s;

}

};

//最后，将原型对象中的实例方法设置为不可枚举的

//这里用到了刚定义的方法

Properties.prototype.properties().hide();

}());

</code></pre>
<h3 id="模块">模块</h3>
<ul>
<li><strong>用作命名空间的对象</strong></li>
</ul>
<pre><code>
var collections; //声明〈或重新声明）这个全局变量

if (!collections) //如果它原本不存在

collections = {}; //创建一个顶层的命名空间对象

collections.sets = {}; //将sets命名空间创建在它的内部

//在collections.sets内定义set类

collections.sets.AbstractSet = function(){};

</code></pre>
<ul>
<li><strong>作为私有命名空间的函数</strong></li>
</ul>
<pre><code>
//例9-24：模块函数中的Set类

//声明全局变量Set，使用一个函数的返回值给它赋值

//函数结束时紧跟的一对圆括号说明这个函数定义后立即执行

//它的返回值将賦值给Set，而不是将这个函数赋值给Set

//注意它是一个函数表达式，不是一条语句，因此函数invocation并没有创建全局变量

var Set = (function invocation() {

function Set() { //这个构造函数是局部变量

this.values = {}; //这个对象的属性用来保存这个集合

this.n = 0; //集合中值的个数

this.add.apply(this, arguments); //将所有的参数都添加至集合中

}

//给Set.prototype定义实例方法

//这里省略了详细代码.............

Set.prototype.contains = function (value) {

//注意我们调用了v2s()，而不是调用带有笨重的前缀的set·_v2s()

return this.values.hasOwnProperty(v2s(value));

};

Set.prototype.size = function() { return this.n; };

Set.prototype.add = function() { /* ... */ };

Set.prototype.remove = function() { /* ... */ };

Set.prototype.foreach = function(f, context) { /* ... */ };

//这里是上面的方法用到的一些辅助函数和变量

//它们不属于模块的共有API，但它们都隐藏在这个函数作用域内

//因此我们不必将它们定义为Set的属性或使用下划线作为其前缀

function v2s(val) { /* ... */ }

function objectId(o) { /* ... */ }

var nextId = 1;

//这个模块的共有API是Set()构造函数

//我们需要把这个函数从私有命名空间中导出来

//以便在外部也可以使用它，在这种情况下，我们通过返回这个构造函数来导出它

//它变成第一行代码所指的表达式的值

return Set;

}());

</code></pre>

