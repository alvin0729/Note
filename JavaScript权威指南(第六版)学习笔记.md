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

