---
layout:     post
title:      Ruby关于 Array 和 String 的一些实用方法
category: rubytree
tags: [rubytree]
excerpt: 到底活成什么样，说到底都是自己的选择。
---


Ruby关于 Array 和 String 的一些实用方法
=======

> 总结了一下实际项目用到的不多，但是面试中很有用的一些方法。

--------------------------------------------------------------------------------

Array的用法
----------

<html>
<div class="show-content-free">
            <h3>Array的用法</h3>
<p>1.新建数组</p>
<pre class="hljs ruby"><code class="ruby">ary = [<span class="hljs-number">1</span>, <span class="hljs-string">"two"</span>, <span class="hljs-number">3.0</span>] <span class="hljs-comment">#=&gt; [1, "two", 3.0]</span>
ary = Array.new<span class="hljs-comment">#=&gt; []</span>
Array.new(<span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; [nil, nil, nil]</span>
Array.new(<span class="hljs-number">3</span>, <span class="hljs-literal">true</span>) <span class="hljs-comment">#=&gt; [true, true, true]</span>
Array.new(<span class="hljs-number">4</span>) { Hash.new } <span class="hljs-comment">#=&gt; [{}, {}, {}, {}]</span>
empty_table = Array.new(<span class="hljs-number">3</span>) { Array.new(<span class="hljs-number">3</span>) } <span class="hljs-comment">#=&gt; [[nil, nil, nil], [nil, nil, nil], [nil, nil, nil]]</span>
Array({<span class="hljs-symbol">:a</span> =&gt; <span class="hljs-string">"a"</span>, <span class="hljs-symbol">:b</span> =&gt; <span class="hljs-string">"b"</span>}) <span class="hljs-comment">#=&gt; [[:a, "a"], [:b, "b"]]</span>
</code></pre>
<p>2.元素访问</p>
<pre class="hljs ruby"><code class="ruby">arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>]  arr[<span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; 3</span>
arr[<span class="hljs-number">100</span>] <span class="hljs-comment">#=&gt; nil </span>
arr[-<span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; 4 </span>
arr[<span class="hljs-number">2</span>, <span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; [3, 4, 5] </span>
arr[<span class="hljs-number">1</span>..<span class="hljs-number">4</span>] <span class="hljs-comment">#=&gt; [2, 3, 4, 5]</span>
arr.at(<span class="hljs-number">0</span>) <span class="hljs-comment">#=&gt; 1</span>
arr = [<span class="hljs-string">'a'</span>, <span class="hljs-string">'b'</span>, <span class="hljs-string">'c'</span>, <span class="hljs-string">'d'</span>, <span class="hljs-string">'e'</span>, <span class="hljs-string">'f'</span>]
arr.fetch(<span class="hljs-number">100</span>) <span class="hljs-comment">#=&gt; IndexError: index 100 outside of array bounds: -6...6 </span>
arr.fetch(<span class="hljs-number">100</span>, <span class="hljs-string">"oops"</span>) <span class="hljs-comment">#=&gt; "oops" </span>
arr.first <span class="hljs-comment">#=&gt; 1 </span>
arr.last <span class="hljs-comment">#=&gt; 6</span>
arr.take(<span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; [1, 2, 3]</span>
arr.drop(<span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; [4, 5, 6]</span>
</code></pre>
<p>3.获取数组信息</p>
<pre class="hljs ruby"><code class="ruby">browsers = [<span class="hljs-string">'Chrome'</span>, <span class="hljs-string">'Firefox'</span>, <span class="hljs-string">'Safari'</span>, <span class="hljs-string">'Opera'</span>, <span class="hljs-string">'IE'</span>]
browsers.length <span class="hljs-comment">#=&gt; 5</span>
browsers.count <span class="hljs-comment">#=&gt; 5</span>
browsers.empty? <span class="hljs-comment">#=&gt; false</span>
browsers.<span class="hljs-keyword">include</span>?(<span class="hljs-string">'Konqueror'</span>) <span class="hljs-comment">#=&gt; false</span>
</code></pre>
<p>4.向数组添加元素</p>
<pre class="hljs ruby"><code class="ruby">arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>]
arr.push(<span class="hljs-number">5</span>) <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5]</span>
arr &lt;&lt;<span class="hljs-number">6</span> <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5, 6]</span>
arr.unshift(<span class="hljs-number">0</span>) <span class="hljs-comment">#=&gt; [0, 1, 2, 3, 4, 5, 6]</span>
a = [ <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span> ]
a.unshift(<span class="hljs-string">"a"</span>) <span class="hljs-comment">#=&gt; ["a", "b", "c", "d"] </span>
a.unshift(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; [ 1, 2, "a", "b", "c", "d"]</span>
arr.insert(<span class="hljs-number">3</span>, <span class="hljs-string">'apple'</span>) <span class="hljs-comment">#=&gt; [0, 1, 2, 'apple', 3, 4, 5, 6]</span>
arr.insert(<span class="hljs-number">3</span>, <span class="hljs-string">'orange'</span>, <span class="hljs-string">'pear'</span>, <span class="hljs-string">'grapefruit'</span>) <span class="hljs-comment">#=&gt; [0, 1, 2, "orange", "pear", "grapefruit", "apple", 3, 4, 5, 6]</span>
</code></pre>
<p>5.从数组删除元素</p>
<pre class="hljs ruby"><code class="ruby">arr =  [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>]
arr.pop <span class="hljs-comment">#=&gt; 6</span>
arr <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5] </span>
arr.pop(<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt;[4,5]   </span>
arr <span class="hljs-comment">#=&gt;[1,2,3]  </span>
arr.shift <span class="hljs-comment">#=&gt; 1 </span>
arr <span class="hljs-comment">#=&gt; [2, 3, 4, 5]  </span>
arr.delete_at(<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; 4  </span>
arr <span class="hljs-comment">#=&gt; [2, 3, 5]  </span>
arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>]
arr.delete(<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; [1, 3]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.delete(<span class="hljs-string">"b"</span>) <span class="hljs-comment">#=&gt; "b"  </span>
a <span class="hljs-comment">#=&gt; ["a", "c"]  </span>
a.delete(<span class="hljs-string">"z"</span>) <span class="hljs-comment">#=&gt; nil  </span>
a.delete(<span class="hljs-string">"z"</span>) { <span class="hljs-string">"not found"</span> } <span class="hljs-comment">#=&gt; "not found"</span>
arr = [<span class="hljs-string">'foo'</span>, <span class="hljs-number">0</span>, <span class="hljs-literal">nil</span>, <span class="hljs-string">'bar'</span>, <span class="hljs-number">7</span>, <span class="hljs-string">'baz'</span>, <span class="hljs-literal">nil</span>]
arr.compact <span class="hljs-comment">#=&gt; ['foo', 0, 'bar', 7, 'baz'] </span>
arr <span class="hljs-comment">#=&gt; ['foo', 0, nil, 'bar', 7, 'baz', nil]</span>
arr.compact! <span class="hljs-comment">#=&gt; ['foo', 0, 'bar', 7, 'baz'] </span>
arr <span class="hljs-comment">#=&gt; ['foo', 0, 'bar', 7, 'baz']</span>
arr = [<span class="hljs-number">2</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>, <span class="hljs-number">556</span>, <span class="hljs-number">6</span>, <span class="hljs-number">6</span>, <span class="hljs-number">8</span>, <span class="hljs-number">9</span>, <span class="hljs-number">0</span>, <span class="hljs-number">123</span>, <span class="hljs-number">556</span>]
arr.uniq <span class="hljs-comment">#=&gt; [2, 5, 6, 556, 8, 9, 0, 123]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.delete_if {<span class="hljs-params">|x|</span> x &gt;= <span class="hljs-string">"b"</span> } <span class="hljs-comment">#=&gt; ["a"]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.slice!(<span class="hljs-number">1</span>) <span class="hljs-comment">#=&gt; "b"</span>
a <span class="hljs-comment">#=&gt; ["a", "c"]</span>
a.slice!(-<span class="hljs-number">1</span>) <span class="hljs-comment">#=&gt; "c"</span>
a <span class="hljs-comment">#=&gt; ["a"] </span>
a.slice!(<span class="hljs-number">100</span>) *<span class="hljs-comment">#=&gt; nil</span>
a <span class="hljs-comment">#=&gt; ["a"]</span>
</code></pre>
<p>6.遍历数组</p>
<pre class="hljs ruby"><code class="ruby">arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>]
arr.each { <span class="hljs-params">|a|</span> printa -= <span class="hljs-number">10</span>, <span class="hljs-string">" "</span> } <span class="hljs-comment"># prints: -9 -8 -7 -6 -5#=&gt; [1, 2, 3, 4, 5]</span>
words = <span class="hljs-string">%w[rats live on no evil star]</span>
str = <span class="hljs-string">""</span>words.reverse_each { <span class="hljs-params">|word|</span> str += <span class="hljs-string">"<span class="hljs-subst">#{word.reverse}</span> "</span> } str<span class="hljs-comment">#=&gt; "rats live on no evil star "</span>
arr.map { <span class="hljs-params">|a|</span> <span class="hljs-number">2</span>*a } <span class="hljs-comment">#=&gt; [2, 4, 6, 8, 10]</span>
arr<span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5]</span>
arr.map! { <span class="hljs-params">|a|</span> a**<span class="hljs-number">2</span> } <span class="hljs-comment">#=&gt; [1, 4, 9, 16, 25]</span>
arr<span class="hljs-comment">#=&gt; [1, 4, 9, 16, 25]</span>
</code></pre>
<p>7.从数组选择元素</p>
<pre class="hljs ruby"><code class="ruby">arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>]
arr.select { <span class="hljs-params">|a|</span> a &gt; <span class="hljs-number">3</span> } <span class="hljs-comment">#=&gt; [4, 5, 6]</span>
arr.reject { <span class="hljs-params">|a|</span> a &lt; <span class="hljs-number">3</span> } <span class="hljs-comment">#=&gt; [3, 4, 5, 6]</span>
arr.reject { <span class="hljs-params">|a|</span> a == <span class="hljs-number">3</span> } <span class="hljs-comment">#=&gt; [1, 2, 4, 5, 6]</span>
arr.drop_while { <span class="hljs-params">|a|</span> a &lt; <span class="hljs-number">4</span> } <span class="hljs-comment">#=&gt; [4, 5, 6]</span>
arr<span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5, 6]</span>
arr.delete_if { <span class="hljs-params">|a|</span> a &lt; <span class="hljs-number">4</span> } <span class="hljs-comment">#=&gt; [4, 5, 6]</span>
arr<span class="hljs-comment">#=&gt; [4, 5, 6]</span>
arr = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>]
arr.keep_if { <span class="hljs-params">|a|</span> a &lt; <span class="hljs-number">4</span> } <span class="hljs-comment">#=&gt; [1, 2, 3]</span>
arr<span class="hljs-comment">#=&gt; [1, 2, 3]</span>
</code></pre>
<p>8.获取两个数组相同的元素</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-number">1</span>, <span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-number">5</span> ] &amp; [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] <span class="hljs-comment">#=&gt; [ 1, 3 ]</span>
[ <span class="hljs-string">'a'</span>, <span class="hljs-string">'b'</span>, <span class="hljs-string">'b'</span>, <span class="hljs-string">'z'</span> ] &amp; [ <span class="hljs-string">'a'</span>, <span class="hljs-string">'b'</span>, <span class="hljs-string">'c'</span> ] <span class="hljs-comment">#=&gt; [ 'a', 'b' ]</span>
</code></pre>
<p>9.数组成倍夸张</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] * <span class="hljs-number">3</span> <span class="hljs-comment">#=&gt; [ 1, 2, 3, 1, 2, 3, 1, 2, 3 ]</span>
[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] * <span class="hljs-string">","</span> <span class="hljs-comment">#=&gt; "1,2,3"  同: [ 1, 2, 3 ].join(',') </span>
</code></pre>
<p>10.数组相加</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] + [ <span class="hljs-number">4</span>, <span class="hljs-number">5</span> ] <span class="hljs-comment">#=&gt; [ 1, 2, 3, 4, 5 ]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a + [ <span class="hljs-string">"d"</span>, <span class="hljs-string">"e"</span>, <span class="hljs-string">"f"</span> ]
a <span class="hljs-comment">#=&gt; [ "a", "b", "c", "d", "e", "f" ]</span>
</code></pre>
<p>11.数组相减</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-number">1</span>, <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span> ] - [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">4</span> ]
</code></pre>
<p>12.数组追加</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span> ] &lt;&lt; <span class="hljs-string">"c"</span> &lt;&lt; <span class="hljs-string">"d"</span> &lt;&lt; [ <span class="hljs-number">3</span>, <span class="hljs-number">4</span> ]<span class="hljs-comment">#=&gt; [ 1, 2, "c", "d", [ 3, 4 ] ]</span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span> ].concat( [<span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span>] ) <span class="hljs-comment">#=&gt; [ "a", "b", "c", "d" ] </span>
a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ]
a.concat( [ <span class="hljs-number">4</span>, <span class="hljs-number">5</span> ] )
a <span class="hljs-comment">#=&gt; [ 1, 2, 3, 4, 5 ]</span>
</code></pre>
<p>13.数组比较</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span> ] &lt;=&gt; [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]  <span class="hljs-comment">#=&gt; -1</span>
[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span> ] &lt;=&gt; [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span> ]<span class="hljs-comment">#=&gt; +1</span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span> ]== [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-number">7</span> ] <span class="hljs-comment">#=&gt; false </span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-number">7</span> ] == [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-number">7</span> ] <span class="hljs-comment">#=&gt; true </span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-number">7</span> ] == [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"d"</span>, <span class="hljs-string">"f"</span> ] <span class="hljs-comment">#=&gt; false</span>
</code></pre>
<p>14.获取数组中的元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span>, <span class="hljs-string">"e"</span> ]
a[<span class="hljs-number">2</span>] + a[<span class="hljs-number">0</span>] + a[<span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; "cab" </span>
a[<span class="hljs-number">6</span>] <span class="hljs-comment">#=&gt; nil </span>
a[<span class="hljs-number">1</span>, <span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; [ "b", "c" ] </span>
a[<span class="hljs-number">1</span>..<span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; [ "b", "c", "d" ] </span>
a[<span class="hljs-number">4</span>..<span class="hljs-number">7</span>] <span class="hljs-comment">#=&gt; [ "e" ] </span>
a[<span class="hljs-number">6</span>..<span class="hljs-number">10</span>] <span class="hljs-comment">#=&gt; nil </span>
a[-<span class="hljs-number">3</span>, <span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; [ "c", "d", "e" ] # special cases </span>
a[<span class="hljs-number">5</span>] <span class="hljs-comment">#=&gt; nil </span>
a[<span class="hljs-number">6</span>, <span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; nil </span>
a[<span class="hljs-number">5</span>, <span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; [] </span>
a[<span class="hljs-number">5</span>..<span class="hljs-number">10</span>] <span class="hljs-comment">#=&gt; []</span>
</code></pre>
<p>15.数组赋值</p>
<pre class="hljs ruby"><code class="ruby">a = Array.new
a[<span class="hljs-number">4</span>] = <span class="hljs-string">"4"</span>; <span class="hljs-comment">#=&gt; [nil, nil, nil, nil, "4"] </span>
a[<span class="hljs-number">0</span>, <span class="hljs-number">3</span>] = [ <span class="hljs-string">'a'</span>, <span class="hljs-string">'b'</span>, <span class="hljs-string">'c'</span> ] <span class="hljs-comment">#=&gt; ["a", "b", "c", nil, "4"] </span>
a[<span class="hljs-number">1</span>..<span class="hljs-number">2</span>] = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span> ] <span class="hljs-comment">#=&gt; ["a", 1, 2, nil, "4"] </span>
a[<span class="hljs-number">0</span>, <span class="hljs-number">2</span>] = <span class="hljs-string">"?"</span> <span class="hljs-comment">#=&gt; ["?", 2, nil, "4"] </span>
a[<span class="hljs-number">0</span>..<span class="hljs-number">2</span>] = <span class="hljs-string">"A"</span> <span class="hljs-comment">#=&gt; ["A", "4"] </span>
a[-<span class="hljs-number">1</span>]  = <span class="hljs-string">"Z"</span> <span class="hljs-comment">#=&gt; ["A", "Z"] </span>
a[<span class="hljs-number">1</span>..-<span class="hljs-number">1</span>] = <span class="hljs-literal">nil</span> <span class="hljs-comment">#=&gt; ["A", nil] </span>
a[<span class="hljs-number">1</span>..-<span class="hljs-number">1</span>] = [] <span class="hljs-comment">#=&gt; ["A"] </span>
a[<span class="hljs-number">0</span>, <span class="hljs-number">0</span>] = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span> ] <span class="hljs-comment">#=&gt; [1, 2, "A"] </span>
a[<span class="hljs-number">3</span>, <span class="hljs-number">0</span>] = <span class="hljs-string">"B"</span> <span class="hljs-comment">#=&gt; [1, 2, "A", "B"]</span>
</code></pre>
<p>16.计算字符串中自身缩写集</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-keyword">require</span> <span class="hljs-string">'abbrev'</span>
<span class="hljs-string">%w{ car cone }</span>.abbrev <span class="hljs-comment">#=&gt; {"car"=&gt;"car", "ca"=&gt;"car", "cone"=&gt;"cone", "con"=&gt;"cone", "co"=&gt;"cone"}</span>
<span class="hljs-string">%w{ number cone }</span>.abbrev<span class="hljs-comment">#=&gt; {"number"=&gt;"number", "numbe"=&gt;"number", "numb"=&gt;"number", "num"=&gt;"number", "nu"=&gt;"number", "n"=&gt;"number", "cone"=&gt;"cone", "con"=&gt;"cone", "co"=&gt;"cone", "c"=&gt;"cone"}</span>
<span class="hljs-string">%w{ fast boat day }</span>.abbrev(<span class="hljs-regexp">/^.a/</span>) <span class="hljs-comment">#=&gt; {"fast"=&gt;"fast", "fas"=&gt;"fast", "fa"=&gt;"fast", "day"=&gt;"day", "da"=&gt;"day"}</span>
</code></pre>
<p>17.查找二维数组首元素匹配的数组</p>
<pre class="hljs ruby"><code class="ruby">s1 = [ <span class="hljs-string">"colors"</span>, <span class="hljs-string">"red"</span>, <span class="hljs-string">"blue"</span>, <span class="hljs-string">"green"</span> ]
s2 = [ <span class="hljs-string">"letters"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
s3 = <span class="hljs-string">"foo"</span>
a = [ s1, s2, s3 ]
a.assoc(<span class="hljs-string">"letters"</span>) <span class="hljs-comment">#=&gt; [ "letters", "a", "b", "c" ] </span>
a.assoc(<span class="hljs-string">"foo"</span>) <span class="hljs-comment">#=&gt; nil</span>
a.assoc(<span class="hljs-string">"a"</span>) <span class="hljs-comment">#=&gt; nil</span>
</code></pre>
<p>18.清空数组</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span>, <span class="hljs-string">"e"</span> ] a.clear <span class="hljs-comment">#=&gt; [ ]</span>
</code></pre>
<p>19.数组元素调整</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span> ]
a.map { <span class="hljs-params">|x|</span> x + <span class="hljs-string">"!"</span> } <span class="hljs-comment">#=&gt; ["a!", "b!", "c!", "d!"] </span>
a <span class="hljs-comment">#=&gt; ["a", "b", "c", "d"]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span> ]
a.map! {<span class="hljs-params">|x|</span> x + <span class="hljs-string">"!"</span> }
a <span class="hljs-comment">#=&gt;  [ "a!", "b!", "c!", "d!" ]</span>
</code></pre>
<p>20.一维数组转多维数组</p>
<pre class="hljs ruby"><code class="ruby">a = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>]
a.combination(<span class="hljs-number">1</span>).to_a <span class="hljs-comment">#=&gt; [[1],[2],[3],[4]] </span>
a.combination(<span class="hljs-number">2</span>).to_a <span class="hljs-comment">#=&gt; [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]] </span>
a.combination(<span class="hljs-number">3</span>).to_a <span class="hljs-comment">#=&gt; [[1,2,3],[1,2,4],[1,3,4],[2,3,4]] </span>
a.combination(<span class="hljs-number">4</span>).to_a <span class="hljs-comment">#=&gt; [[1,2,3,4]] </span>
a.combination(<span class="hljs-number">0</span>).to_a <span class="hljs-comment">#=&gt; [[]] # one combination of length 0 </span>
a.combination(<span class="hljs-number">5</span>).to_a <span class="hljs-comment">#=&gt; []  # no combinations of length 5</span>
</code></pre>
<p>21.去除数组中的nil元素</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-string">"a"</span>, <span class="hljs-literal">nil</span>, <span class="hljs-string">"b"</span>, <span class="hljs-literal">nil</span>, <span class="hljs-string">"c"</span>, <span class="hljs-literal">nil</span> ].compact <span class="hljs-comment">#=&gt; [ "a", "b", "c" ]</span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-literal">nil</span>, <span class="hljs-string">"b"</span>, <span class="hljs-literal">nil</span>, <span class="hljs-string">"c"</span> ].compact! <span class="hljs-comment">#=&gt; [ "a", "b", "c" ] </span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ].compact! <span class="hljs-comment">#=&gt; nil</span>
</code></pre>
<p>22.数组统计</p>
<pre class="hljs ruby"><code class="ruby">ary = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">4</span>, <span class="hljs-number">2</span>] ary.count <span class="hljs-comment">#=&gt; 4 </span>
ary.count(<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; 2 </span>
ary.count { <span class="hljs-params">|x|</span> x%<span class="hljs-number">2</span> == <span class="hljs-number">0</span> } <span class="hljs-comment">#=&gt; 3</span>
</code></pre>
<p>23.数组循环</p>
<pre class="hljs ruby"><code class="ruby">n次a = [<span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>]
a.cycle { <span class="hljs-params">|x|</span> puts x } <span class="hljs-comment"># print, a, b, c, a, b, c,.. forever. </span>
a.cycle(<span class="hljs-number">2</span>) { <span class="hljs-params">|x|</span> puts x } <span class="hljs-comment"># print, a, b, c, a, b, c.</span>
</code></pre>
<p>24.数组为空判断</p>
<pre class="hljs ruby"><code class="ruby">[].empty? <span class="hljs-comment">#=&gt; true</span>
</code></pre>
<p>25.改变数组元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span> ]
a.fill(<span class="hljs-string">"x"</span>) <span class="hljs-comment">#=&gt; ["x", "x", "x", "x"] </span>
a.fill(<span class="hljs-string">"z"</span>, <span class="hljs-number">2</span>, <span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; ["x", "x", "z", "z"] </span>
a.fill(<span class="hljs-string">"y"</span>, <span class="hljs-number">0</span>..<span class="hljs-number">1</span>) <span class="hljs-comment">#=&gt; ["y", "y", "z", "z"] </span>
a.fill { <span class="hljs-params">|i|</span> i*i } <span class="hljs-comment">#=&gt; [0, 1, 4, 9] </span>
a.fill(-<span class="hljs-number">2</span>) { <span class="hljs-params">|i|</span> i*i*i } <span class="hljs-comment">#=&gt; [0, 1, 8, 27]</span>
</code></pre>
<p>26.定位指定元素的位置</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.index(<span class="hljs-string">"b"</span>) <span class="hljs-comment">#=&gt; 1 </span>
a.index(<span class="hljs-string">"z"</span>) <span class="hljs-comment">#=&gt; nil </span>
a.index { <span class="hljs-params">|x|</span> x == <span class="hljs-string">"b"</span> } <span class="hljs-comment">#=&gt; 1</span>
</code></pre>
<p>27.获取数组前n个元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"q"</span>, <span class="hljs-string">"r"</span>, <span class="hljs-string">"s"</span>, <span class="hljs-string">"t"</span> ]
a.first <span class="hljs-comment">#=&gt; "q" </span>
a.first(<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; ["q", "r"]</span>
</code></pre>
<p>28.多维数组向低维数组转换</p>
<pre class="hljs ruby"><code class="ruby">s = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] <span class="hljs-comment">#=&gt; [1, 2, 3] </span>
t = [ <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>, [<span class="hljs-number">7</span>, <span class="hljs-number">8</span>] ] <span class="hljs-comment">#=&gt; [4, 5, 6, [7, 8]] </span>
a = [ s, t, <span class="hljs-number">9</span>, <span class="hljs-number">10</span> ] <span class="hljs-comment">#=&gt; [[1, 2, 3], [4, 5, 6, [7, 8]], 9, 10] </span>
a.flatten <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] </span>
a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, [<span class="hljs-number">3</span>, [<span class="hljs-number">4</span>, <span class="hljs-number">5</span>] ] ]
a.flatten(<span class="hljs-number">1</span>) <span class="hljs-comment">#=&gt; [1, 2, 3, [4, 5]]</span>
a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, [<span class="hljs-number">3</span>, [<span class="hljs-number">4</span>, <span class="hljs-number">5</span>] ] ]
a.flatten! <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5] </span>
a.flatten! <span class="hljs-comment">#=&gt; nil </span>
a <span class="hljs-comment">#=&gt; [1, 2, 3, 4, 5] </span>
a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, [<span class="hljs-number">3</span>, [<span class="hljs-number">4</span>, <span class="hljs-number">5</span>] ] ]
a.flatten!(<span class="hljs-number">1</span>) <span class="hljs-comment">#=&gt; [1, 2, 3, [4, 5]]</span>
</code></pre>
<p>29.数组替换</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"d"</span>, <span class="hljs-string">"e"</span> ]
a.replace([ <span class="hljs-string">"x"</span>, <span class="hljs-string">"y"</span>, <span class="hljs-string">"z"</span> ]) <span class="hljs-comment">#=&gt; ["x", "y", "z"] </span>
a <span class="hljs-comment">#=&gt; ["x", "y", "z"]</span>
</code></pre>
<p>30.插入元素到数组指定位置</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">%w{ a b c d }</span>
a.insert(<span class="hljs-number">2</span>, <span class="hljs-number">99</span>) <span class="hljs-comment">#=&gt; ["a", "b", 99, "c", "d"] </span>
a.insert(-<span class="hljs-number">2</span>, <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; ["a", "b", 99, "c", 1, 2, 3, "d"]</span>
</code></pre>
<p>31.数组元素拼接为指定格式字符串</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ].join <span class="hljs-comment">#=&gt; "abc" </span>
[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ].join(<span class="hljs-string">"-"</span>) <span class="hljs-comment">#=&gt; "a-b-c"</span>
</code></pre>
<p>32.获取数组后n个元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"w"</span>, <span class="hljs-string">"x"</span>, <span class="hljs-string">"y"</span>, <span class="hljs-string">"z"</span> ]
a.last <span class="hljs-comment">#=&gt; "z"  </span>
a.last(<span class="hljs-number">2</span>) *<span class="hljs-comment">#=&gt; ["y", "z"]*</span>
</code></pre>
<p>33.数组元素反转</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ].reverse <span class="hljs-comment">#=&gt; ["c", "b", "a"]</span>
 [ <span class="hljs-number">1</span> ].reverse <span class="hljs-comment">#=&gt; [1]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.reverse! <span class="hljs-comment">#=&gt; ["c", "b", "a"] </span>
a <span class="hljs-comment">#=&gt; ["c", "b", "a"]</span>
</code></pre>
<p>34.数组从后向前循环</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ] a.reverse_each {<span class="hljs-params">|x|</span> print x, <span class="hljs-string">" "</span> } <span class="hljs-comment">#=&gt; c b a</span>
</code></pre>
<p>35.定位数组元素最后一个出现的位置</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.rindex(<span class="hljs-string">"b"</span>) <span class="hljs-comment">#=&gt; 3 </span>
a.rindex(<span class="hljs-string">"z"</span>) <span class="hljs-comment">#=&gt; nil </span>
a.rindex { <span class="hljs-params">|x|</span> x == <span class="hljs-string">"b"</span> } <span class="hljs-comment">#=&gt; 3</span>
</code></pre>
<p>36.从一个数组随机去n个元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span>, <span class="hljs-number">7</span>, <span class="hljs-number">8</span>, <span class="hljs-number">9</span>, <span class="hljs-number">10</span> ]
a.sample <span class="hljs-comment">#=&gt; 7 </span>
a.sample(<span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; [6, 4, 2, 5]</span>
</code></pre>
<p>37.随机打乱数组元素</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span> ] <span class="hljs-comment">#=&gt; [1, 2, 3] </span>
a.shuffle <span class="hljs-comment">#=&gt; [2, 3, 1]</span>
a.shuffle(<span class="hljs-symbol">random:</span> Random.new(<span class="hljs-number">1</span>)) <span class="hljs-comment">#=&gt; [1, 3, 2]</span>
</code></pre>
<p>38.数组排序</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"d"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"e"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"b"</span> ]
a.sort <span class="hljs-comment">#=&gt; ["a", "b", "c", "d", "e"]  #升序排序</span>
a.sort { <span class="hljs-params">|x,y|</span> y &lt;=&gt; x } <span class="hljs-comment">#=&gt; ["e", "d", "c", "b", "a"]  #降序排序</span>
a = [ <span class="hljs-string">"d"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"e"</span>, <span class="hljs-string">"c"</span>, <span class="hljs-string">"b"</span> ]
a.sort! <span class="hljs-comment">#=&gt; ["a", "b", "c", "d", "e"] </span>
a.sort! { <span class="hljs-params">|x,y|</span> y &lt;=&gt; x } <span class="hljs-comment">#=&gt; ["e", "d", "c", "b", "a"]</span>
</code></pre>
<p>39.二维数组行列倒转</p>
<pre class="hljs ruby"><code class="ruby">a = [[<span class="hljs-number">1</span>,<span class="hljs-number">2</span>], [<span class="hljs-number">3</span>,<span class="hljs-number">4</span>], [<span class="hljs-number">5</span>,<span class="hljs-number">6</span>]]
a.transpose <span class="hljs-comment">#=&gt; [[1, 3, 5], [2, 4, 6]]</span>
</code></pre>
<p>40.数组去重</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]
a.uniq <span class="hljs-comment"># =&gt; ["a", "b", "c"] </span>
b = [[<span class="hljs-string">"student"</span>,<span class="hljs-string">"sam"</span>], [<span class="hljs-string">"student"</span>,<span class="hljs-string">"george"</span>], [<span class="hljs-string">"teacher"</span>,<span class="hljs-string">"matz"</span>]]
b.uniq { <span class="hljs-params">|s|</span> s.first } <span class="hljs-comment"># =&gt; [["student", "sam"], ["teacher", "matz"]]</span>
a = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ] a.uniq! <span class="hljs-comment"># =&gt; ["a", "b", "c"]</span>
b = [ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ]  b.uniq! <span class="hljs-comment"># =&gt; nil</span>
c = [[<span class="hljs-string">"student"</span>,<span class="hljs-string">"sam"</span>], [<span class="hljs-string">"student"</span>,<span class="hljs-string">"george"</span>], [<span class="hljs-string">"teacher"</span>,<span class="hljs-string">"matz"</span>]]
c.uniq! { <span class="hljs-params">|s|</span> s.first } <span class="hljs-comment"># =&gt; [["student", "sam"], ["teacher", "matz"]]</span>
</code></pre>
<p>41.一次获取数组不同位置的元素</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">%w{ a b c d e f }</span>
a.values_at(<span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-number">5</span>) <span class="hljs-comment"># =&gt; ["b", "d", "f"] </span>
a.values_at(<span class="hljs-number">1</span>, <span class="hljs-number">3</span>, <span class="hljs-number">5</span>, <span class="hljs-number">7</span>) <span class="hljs-comment"># =&gt; ["b", "d", "f", nil] </span>
a.values_at(-<span class="hljs-number">1</span>, -<span class="hljs-number">2</span>, -<span class="hljs-number">2</span>, -<span class="hljs-number">7</span>) <span class="hljs-comment"># =&gt; ["f", "e", "e", nil] </span>
a.values_at(<span class="hljs-number">4</span>..<span class="hljs-number">6</span>, <span class="hljs-number">3</span>...<span class="hljs-number">6</span>) <span class="hljs-comment"># =&gt; ["e", "f", nil, "d", "e", "f"]</span>
</code></pre>
<p>42.多个一维数组组成二维矩阵</p>
<pre class="hljs ruby"><code class="ruby">a = [ <span class="hljs-number">4</span>, <span class="hljs-number">5</span>, <span class="hljs-number">6</span> ]
b = [ <span class="hljs-number">7</span>, <span class="hljs-number">8</span>, <span class="hljs-number">9</span> ]
[<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>].zip(a, b) <span class="hljs-comment">#=&gt; [[1, 4, 7], [2, 5, 8], [3, 6, 9]]</span>
[<span class="hljs-number">1</span>, <span class="hljs-number">2</span>].zip(a, b) <span class="hljs-comment">#=&gt; [[1, 4, 7], [2, 5, 8]] </span>
a.zip([<span class="hljs-number">1</span>, <span class="hljs-number">2</span>], [<span class="hljs-number">8</span>]) <span class="hljs-comment">#=&gt; [[4, 1, 8], [5, 2, nil], [6, nil, nil]]</span>
</code></pre>
<p>43.合并两个数组并且去重</p>
<pre class="hljs ruby"><code class="ruby">[ <span class="hljs-string">"a"</span>, <span class="hljs-string">"b"</span>, <span class="hljs-string">"c"</span> ] <span class="hljs-params">| [ "c", "d", "a" ] #=&gt; [ "a", "b", "c", "d" ]
</span></code></pre>
<p>44.combination</p>
<pre class="hljs ruby"><code class="ruby">a= [<span class="hljs-number">1</span>,<span class="hljs-number">2</span>,<span class="hljs-number">3</span>,<span class="hljs-number">4</span>]a.combination(<span class="hljs-number">1</span>).to_a<span class="hljs-comment">#=&gt; [[1],[2],[3],[4]]</span>
a.combination(<span class="hljs-number">2</span>).to_a<span class="hljs-comment">#=&gt; [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]</span>
a.combination(<span class="hljs-number">3</span>).to_a<span class="hljs-comment">#=&gt; [[1,2,3],[1,2,4],[1,3,4],[2,3,4]]</span>
a.combination(<span class="hljs-number">4</span>).to_a<span class="hljs-comment">#=&gt; [[1,2,3,4]]</span>
a.combination(<span class="hljs-number">0</span>).to_a<span class="hljs-comment">#=&gt; [[]] # one combination of length 0</span>
a.combination(<span class="hljs-number">5</span>).to_a<span class="hljs-comment">#=&gt; []  # no combinations of length 5</span>
</code></pre>
<ol start="45">
<li>coerce</li>
</ol>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-number">1</span>.coerce(<span class="hljs-number">2</span>) <span class="hljs-comment"># =&gt; [2, 1] </span>
<span class="hljs-number">1</span>.coerce(<span class="hljs-number">2.3</span>) <span class="hljs-comment"># =&gt; [2.3, 1.0] </span>
(<span class="hljs-number">4.5</span>).coerce(<span class="hljs-number">2.3</span>) <span class="hljs-comment"># =&gt; [2.3, 4.5] </span>
(<span class="hljs-number">4.5</span>).coerce(<span class="hljs-number">2</span>) <span class="hljs-comment"># =&gt; [2.0, 4.5]</span>
</code></pre>
<p><a href="https://www.jianshu.com/writer" target="_blank"></a></p>
<h3>String的用法</h3>
<p>1.格式化对象到指定格式字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"%05d"</span> % <span class="hljs-number">123</span> <span class="hljs-comment">#=&gt; "00123"</span>
<span class="hljs-string">"%-5s: %08x"</span> % [ <span class="hljs-string">"ID"</span>, <span class="hljs-keyword">self</span>.object_id ] <span class="hljs-comment">#=&gt; "ID  : 200e14d6"</span>
<span class="hljs-string">"foo = %{foo}"</span> % { <span class="hljs-symbol">:foo</span> =&gt; <span class="hljs-string">'bar'</span> } <span class="hljs-comment">#=&gt; "foo = bar"</span>
</code></pre>
<p>2.复制字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"Ho! "</span> * <span class="hljs-number">3</span> <span class="hljs-comment">#=&gt; "Ho! Ho! Ho! "</span>
<span class="hljs-string">"Ho! "</span> * <span class="hljs-number">0</span> <span class="hljs-comment">#=&gt; ""</span>
</code></pre>
<p>3.字符串拼接</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"Hello from "</span> + <span class="hljs-keyword">self</span>.to_s <span class="hljs-comment">#=&gt; "Hello from main"</span>
a = <span class="hljs-string">"hello "</span>
a &lt;&lt; <span class="hljs-string">"world"</span>  <span class="hljs-comment">#=&gt; "hello world"</span>
a.concat(<span class="hljs-number">33</span>)  <span class="hljs-comment">#=&gt; "hello world!"</span>
</code></pre>
<p>4.字符串比较</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"abcdef"</span> &lt;=&gt; <span class="hljs-string">"abcde"</span>    <span class="hljs-comment">#=&gt; 1</span>
<span class="hljs-string">"abcdef"</span> &lt;=&gt; <span class="hljs-string">"abcdef"</span>    <span class="hljs-comment">#=&gt; 0</span>
<span class="hljs-string">"abcdef"</span> &lt;=&gt; <span class="hljs-string">"abcdefg"</span>  <span class="hljs-comment">#=&gt; -1</span>
<span class="hljs-string">"abcdef"</span> &lt;=&gt; <span class="hljs-string">"ABCDEF"</span>    <span class="hljs-comment">#=&gt; 1</span>
<span class="hljs-string">"abcdef"</span>.casecmp(<span class="hljs-string">"abcde"</span>) <span class="hljs-comment">#=&gt; 1 </span>
<span class="hljs-string">"aBcDeF"</span>.casecmp(<span class="hljs-string">"abcdef"</span>) <span class="hljs-comment">#=&gt; 0</span>
<span class="hljs-string">"abcdef"</span>.casecmp(<span class="hljs-string">"abcdefg"</span>) <span class="hljs-comment">#=&gt; -1</span>
<span class="hljs-string">"abcdef"</span>.casecmp(<span class="hljs-string">"ABCDEF"</span>) <span class="hljs-comment">#=&gt; 0</span>
</code></pre>
<p>5.获取子字符串</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">"hello there"</span>
a[<span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; "e"  </span>
a[<span class="hljs-number">2</span>, <span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; "llo"</span>
a[<span class="hljs-number">2</span>..<span class="hljs-number">3</span>] <span class="hljs-comment">#=&gt; "ll"</span>
a[-<span class="hljs-number">3</span>, <span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; "er"</span>
a[<span class="hljs-number">7</span>..-<span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; "her"</span>
a[-<span class="hljs-number">4</span>..-<span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; "her"</span>
a[-<span class="hljs-number">2</span>..-<span class="hljs-number">4</span>] <span class="hljs-comment">#=&gt; ""</span>
a[<span class="hljs-number">11</span>, <span class="hljs-number">0</span>] <span class="hljs-comment">#=&gt; ""</span>
a[<span class="hljs-number">11</span>] <span class="hljs-comment">#=&gt; nil</span>
a[<span class="hljs-number">12</span>, <span class="hljs-number">0</span>] <span class="hljs-comment">#=&gt; nil</span>
a[<span class="hljs-number">12</span>..-<span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; nil</span>
a[<span class="hljs-regexp">/[aeiou](.)\1/</span>] <span class="hljs-comment">#=&gt; "ell"</span>
a[<span class="hljs-regexp">/[aeiou](.)\1/</span>, <span class="hljs-number">0</span>] <span class="hljs-comment">#=&gt; "ell"</span>
a[<span class="hljs-regexp">/[aeiou](.)\1/</span>, <span class="hljs-number">1</span>] <span class="hljs-comment">#=&gt; "l"</span>
a[<span class="hljs-regexp">/[aeiou](.)\1/</span>, <span class="hljs-number">2</span>] <span class="hljs-comment">#=&gt; nil</span>
a[<span class="hljs-regexp">/(?[aeiou])(?[^aeiou])/</span>, <span class="hljs-string">"non_vowel"</span>] <span class="hljs-comment">#=&gt; "l"</span>
a[<span class="hljs-regexp">/(?[aeiou])(?[^aeiou])/</span>, <span class="hljs-string">"vowel"</span>] <span class="hljs-comment">#=&gt; "e"</span>
a[<span class="hljs-string">"lo"</span>] <span class="hljs-comment">#=&gt; "lo"</span>
a[<span class="hljs-string">"bye"</span>] <span class="hljs-comment">#=&gt; nil</span>
</code></pre>
<p>6.判断字符串是否只有ASCII字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"abc"</span>.force_encoding(<span class="hljs-string">"UTF-8"</span>).ascii_only? <span class="hljs-comment">#=&gt; true </span>
<span class="hljs-string">"abc\u{6666}"</span>.force_encoding(<span class="hljs-string">"UTF-8"</span>).ascii_only? <span class="hljs-comment">#=&gt; false</span>
</code></pre>
<p>7.统计字符串字节长度</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"\x80\u3042"</span>.bytesize <span class="hljs-comment">#=&gt; 4 </span>
<span class="hljs-string">"hello"</span>.bytesize <span class="hljs-comment">#=&gt; 5</span>
</code></pre>
<p>8.首字母大写</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.capitalize <span class="hljs-comment">#=&gt; "Hello" </span>
<span class="hljs-string">"HELLO"</span>.capitalize <span class="hljs-comment">#=&gt; "Hello" </span>
<span class="hljs-string">"123ABC"</span>.capitalize <span class="hljs-comment">#=&gt; "123abc"</span>
</code></pre>
<p>9.字符串居中</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.center(<span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; "hello"</span>
<span class="hljs-string">"hello"</span>.center(<span class="hljs-number">20</span>) <span class="hljs-comment">#=&gt; "      hello        " </span>
<span class="hljs-string">"hello"</span>.center(<span class="hljs-number">20</span>, <span class="hljs-string">'123'</span>) <span class="hljs-comment">#=&gt; "1231231hello12312312"</span>
</code></pre>
<p>10.去除字符串尾部换行符或者指定字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.chomp <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"hello\n"</span>.chomp <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"hello\r\n"</span>.chomp <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"hello\n\r"</span>.chomp <span class="hljs-comment">#=&gt; "hello\n" </span>
<span class="hljs-string">"hello\r"</span>.chomp <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"hello \n there"</span>.chomp <span class="hljs-comment">#=&gt; "hello \n there" </span>
<span class="hljs-string">"hello"</span>.chomp(<span class="hljs-string">"llo"</span>) <span class="hljs-comment">#=&gt; "he"</span>
</code></pre>
<p>11.去除字符串尾部最后一个字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"string\r\n"</span>.chop <span class="hljs-comment">#=&gt; "string"</span>
<span class="hljs-string">"string\n\r"</span>.chop <span class="hljs-comment">#=&gt; "string\n"</span>
<span class="hljs-string">"string\n"</span>.chop <span class="hljs-comment">#=&gt; "string"</span>
<span class="hljs-string">"string"</span>.chop <span class="hljs-comment">#=&gt; "strin"</span>
<span class="hljs-string">"x"</span>.chop.chop <span class="hljs-comment">#=&gt; ""</span>
</code></pre>
<p>12.获取字符串第一个字符</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">"abcde"</span>
a.chr <span class="hljs-comment">#=&gt; "a"</span>
</code></pre>
<p>13.清空字符</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">"abcde"</span>
a.clear <span class="hljs-comment">#=&gt; ""</span>
</code></pre>
<p>14.统计字符串指定字符出现的个数</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">"hello world"</span> a.count <span class="hljs-string">"lo"</span> <span class="hljs-comment">#=&gt; 5</span>
a.count <span class="hljs-string">"lo"</span>, <span class="hljs-string">"o"</span> <span class="hljs-comment">#=&gt; 2</span>
a.count <span class="hljs-string">"hello"</span>, <span class="hljs-string">"^l"</span> <span class="hljs-comment">#=&gt; 4</span>
a.count <span class="hljs-string">"ej-m"</span> <span class="hljs-comment">#=&gt; 4</span>
<span class="hljs-string">"hello^world"</span>.count <span class="hljs-string">"\\^aeiou"</span> <span class="hljs-comment">#=&gt; 4</span>
<span class="hljs-string">"hello-world"</span>.count <span class="hljs-string">"a\\-eo"</span> <span class="hljs-comment">#=&gt; 4</span>
c = <span class="hljs-string">"hello world\\r\\n"</span>
c.count <span class="hljs-string">"\\"</span> <span class="hljs-comment">#=&gt; 2</span>
c.count <span class="hljs-string">"\\A"</span> <span class="hljs-comment">#=&gt; 0</span>
c.count <span class="hljs-string">"X-\\w"</span> <span class="hljs-comment">#=&gt; 3</span>
</code></pre>
<p>15.删除字符串指定字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.delete <span class="hljs-string">"l"</span>,<span class="hljs-string">"lo"</span> <span class="hljs-comment">#=&gt; "heo"</span>
<span class="hljs-string">"hello"</span>.delete <span class="hljs-string">"lo"</span> <span class="hljs-comment">#=&gt; "he"</span>
<span class="hljs-string">"hello"</span>.delete <span class="hljs-string">"aeiou"</span>, <span class="hljs-string">"^e"</span> <span class="hljs-comment">#=&gt; "hell"</span>
<span class="hljs-string">"hello"</span>.delete <span class="hljs-string">"ej-m"</span> <span class="hljs-comment">#=&gt; "ho"</span>
</code></pre>
<p>16.字符串小写</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hEllO"</span>.downcase <span class="hljs-comment">#=&gt; "hello"</span>
</code></pre>
<p>17.遍历字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.each_char {<span class="hljs-params">|c|</span> print c, <span class="hljs-string">' '</span> }
</code></pre>
<p>18.判断字符串长度是否为</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.empty? <span class="hljs-comment">#=&gt; false </span>
<span class="hljs-string">" "</span>.empty? <span class="hljs-comment">#=&gt; false </span>
<span class="hljs-string">""</span>.empty? <span class="hljs-comment">#=&gt; true</span>
</code></pre>
<p>19.替换字符串指定字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.gsub(<span class="hljs-regexp">/[aeiou]/</span>, <span class="hljs-string">'*'</span>) <span class="hljs-comment">#=&gt; "h*ll*"</span>
<span class="hljs-string">"hello"</span>.gsub(<span class="hljs-regexp">/([aeiou])/</span>, <span class="hljs-string">'&lt;\1&gt;'</span>) <span class="hljs-comment">#=&gt; "hll"</span>
<span class="hljs-string">"hello"</span>.gsub(<span class="hljs-regexp">/./</span>) {<span class="hljs-params">|s|</span> s.ord.to_s + <span class="hljs-string">' '</span>} <span class="hljs-comment">#=&gt; "104 101 108 108 111 "</span>
<span class="hljs-string">"hello"</span>.gsub(<span class="hljs-regexp">/(?[aeiou])/</span>, <span class="hljs-string">'{\k}'</span>) <span class="hljs-comment">#=&gt; "h{e}ll{o}"</span>
<span class="hljs-string">'hello'</span>.gsub(<span class="hljs-regexp">/[eo]/</span>, <span class="hljs-string">'e'</span> =&gt; <span class="hljs-number">3</span>, <span class="hljs-string">'o'</span> =&gt; <span class="hljs-string">'*'</span>) <span class="hljs-comment">#=&gt; "h3ll*"</span>
</code></pre>
<p>19-01.gsub案例（AaaBaa替换为aaa_baa）</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"HelloWorld"</span>.gsub(<span class="hljs-regexp">/([A-Z]+)([A-Z][a-z])/</span>, <span class="hljs-string">'\1_\2'</span>).gsub(<span class="hljs-regexp">/([a-z\d])([A-Z])/</span>, <span class="hljs-string">'\1_\2'</span>).tr(<span class="hljs-string">'-'</span>, <span class="hljs-string">'_'</span>).downcase<span class="hljs-comment">#=&gt; "hello_world"</span>
<span class="hljs-string">"World"</span>.gsub(<span class="hljs-regexp">/([A-Z]+)([A-Z][a-z])/</span>, <span class="hljs-string">'\1_\2'</span>).gsub(<span class="hljs-regexp">/([a-z\d])([A-Z])/</span>, <span class="hljs-string">'\1_\2'</span>).tr(<span class="hljs-string">'-'</span>, <span class="hljs-string">'_'</span>).downcase<span class="hljs-comment">#=&gt; "world"</span>
</code></pre>
<p>20.判断字符串是否包含指定字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.<span class="hljs-keyword">include</span>? <span class="hljs-string">"lo"</span> <span class="hljs-comment">#=&gt; true</span>
<span class="hljs-string">"hello"</span>.<span class="hljs-keyword">include</span>? <span class="hljs-string">"ol"</span> <span class="hljs-comment">#=&gt; false</span>
<span class="hljs-string">"hello"</span>.<span class="hljs-keyword">include</span>? <span class="hljs-string">?h</span> <span class="hljs-comment">#=&gt; true</span>
</code></pre>
<p>21.定位指定字符在字符串中的位置第一次出现的位置</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.index(<span class="hljs-string">'e'</span>) <span class="hljs-comment">#=&gt; 1</span>
<span class="hljs-string">"hello"</span>.index(<span class="hljs-string">'lo'</span>) <span class="hljs-comment">#=&gt; 3</span>
<span class="hljs-string">"hello"</span>.index(<span class="hljs-string">'a'</span>) <span class="hljs-comment">#=&gt; nil</span>
<span class="hljs-string">"hello"</span>.index(<span class="hljs-string">?e</span>) <span class="hljs-comment">#=&gt; 1</span>
<span class="hljs-string">"hello"</span>.index(<span class="hljs-regexp">/[aeiou]/</span>, -<span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; 4</span>
</code></pre>
<p>最后一次出现的位置"hello".rindex('e') #=&gt; 1</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.rindex(<span class="hljs-string">'l'</span>) <span class="hljs-comment">#=&gt; 3</span>
<span class="hljs-string">"hello"</span>.rindex(<span class="hljs-string">'a'</span>) <span class="hljs-comment">#=&gt; nil</span>
<span class="hljs-string">"hello"</span>.rindex(<span class="hljs-string">?e</span>) <span class="hljs-comment">#=&gt; 1</span>
<span class="hljs-string">"hello"</span>.rindex(<span class="hljs-regexp">/[aeiou]/</span>, -<span class="hljs-number">2</span>) <span class="hljs-comment">#=&gt; 1</span>
</code></pre>
<p>22.字符串替换</p>
<pre class="hljs ruby"><code class="ruby">s = <span class="hljs-string">"hello"</span> <span class="hljs-comment">#=&gt; "hello"</span>
s.replace <span class="hljs-string">"world"</span> <span class="hljs-comment">#=&gt; "world"</span>
</code></pre>
<p>23.插入字符到指定位置</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"abcd"</span>.insert(<span class="hljs-number">0</span>, <span class="hljs-string">'X'</span>) <span class="hljs-comment">#=&gt; "Xabcd"</span>
<span class="hljs-string">"abcd"</span>.insert(<span class="hljs-number">3</span>, <span class="hljs-string">'X'</span>) <span class="hljs-comment">#=&gt; "abcXd"</span>
<span class="hljs-string">"abcd"</span>.insert(<span class="hljs-number">4</span>, <span class="hljs-string">'X'</span>) <span class="hljs-comment">#=&gt; "abcdX"</span>
<span class="hljs-string">"abcd"</span>.insert(-<span class="hljs-number">3</span>, <span class="hljs-string">'X'</span>) <span class="hljs-comment">#=&gt; "abXcd"</span>
<span class="hljs-string">"abcd"</span>.insert(-<span class="hljs-number">1</span>, <span class="hljs-string">'X'</span>) <span class="hljs-comment">#=&gt; "abcdX"</span>
</code></pre>
<p>24.字符串对齐左对齐</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hello"</span>.ljust(<span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"hello"</span>.ljust(<span class="hljs-number">20</span>) <span class="hljs-comment">#=&gt; "hello              " </span>
<span class="hljs-string">"hello"</span>.ljust(<span class="hljs-number">20</span>, <span class="hljs-string">'1234'</span>) <span class="hljs-comment">#=&gt; "hello123412341234123"</span>
右对齐<span class="hljs-string">"hello"</span>.rjust(<span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; "hello" "hello".rjust(20) #=&gt; "              hello" </span>
<span class="hljs-string">"hello"</span>.rjust(<span class="hljs-number">20</span>, <span class="hljs-string">'1234'</span>) <span class="hljs-comment">#=&gt; "123412341234123hello"</span>
</code></pre>
<p>25.去除字符串空格</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"  hello  "</span>.lstrip <span class="hljs-comment">#=&gt; "hello  " </span>
<span class="hljs-string">"hello"</span>.lstrip <span class="hljs-comment">#=&gt; "hello"</span>
<span class="hljs-string">"  hello  "</span>.rstrip <span class="hljs-comment">#=&gt; "  hello" </span>
<span class="hljs-string">"hello"</span>.rstrip <span class="hljs-comment">#=&gt; "hello"</span>
<span class="hljs-string">"    hello    "</span>.strip <span class="hljs-comment">#=&gt; "hello" </span>
<span class="hljs-string">"\tgoodbye\r\n"</span>.strip <span class="hljs-comment">#=&gt; "goodbye"</span>
</code></pre>
<p>26.字符串匹配</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">'hello'</span>.match(<span class="hljs-string">'(.)\1'</span>) <span class="hljs-comment">#=&gt; #</span>
<span class="hljs-string">'hello'</span>.match(<span class="hljs-string">'(.)\1'</span>)[<span class="hljs-number">0</span>] <span class="hljs-comment">#=&gt; "ll" </span>
<span class="hljs-string">'hello'</span>.match(<span class="hljs-regexp">/(.)\1/</span>)[<span class="hljs-number">0</span>] <span class="hljs-comment">#=&gt; "ll" </span>
<span class="hljs-string">'hello'</span>.match(<span class="hljs-string">'xx'</span>) <span class="hljs-comment">#=&gt; nil</span>
</code></pre>
<p>27.字符串加一</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"abcd"</span>.succ <span class="hljs-comment">#=&gt; "abce"</span>
<span class="hljs-string">"THX1138"</span>.succ <span class="hljs-comment">#=&gt; "THX1139"</span>
<span class="hljs-string">"&lt;&gt;"</span>.succ <span class="hljs-comment">#=&gt; "&lt;&gt;"</span>
<span class="hljs-string">"1999zzz"</span>.succ <span class="hljs-comment">#=&gt; "2000aaa"</span>
<span class="hljs-string">"ZZZ9999"</span>.succ <span class="hljs-comment">#=&gt; "AAAA0000"</span>
<span class="hljs-string">"***"</span>.succ <span class="hljs-comment">#=&gt; "**+"</span>
</code></pre>
<p>28.获取字符指定ASCII码值</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"a"</span>.ord <span class="hljs-comment">#=&gt; 97</span>
</code></pre>
<p>29.分割字符串到指定数组</p>
<pre class="hljs ruby"><code class="ruby">str.partition(sep) → [head, sep, tail]
str.partition(regexp) → [head, match, tail]
<span class="hljs-string">"hello"</span>.partition(<span class="hljs-string">"l"</span>) <span class="hljs-comment">#=&gt; ["he", "l", "lo"]</span>
<span class="hljs-string">"hello"</span>.partition(<span class="hljs-string">"x"</span>) <span class="hljs-comment">#=&gt; ["hello", "", ""]</span>
<span class="hljs-string">"hello"</span>.partition(<span class="hljs-regexp">/.l/</span>) <span class="hljs-comment">#=&gt; ["h", "el", "lo"]</span>
<span class="hljs-string">"hello"</span>.rpartition(<span class="hljs-string">"l"</span>) <span class="hljs-comment">#=&gt; ["hel", "l", "o"]</span>
<span class="hljs-string">"hello"</span>.rpartition(<span class="hljs-string">"x"</span>) <span class="hljs-comment">#=&gt; ["", "", "hello"]</span>
<span class="hljs-string">"hello"</span>.rpartition(<span class="hljs-regexp">/.l/</span>) <span class="hljs-comment">#=&gt; ["he", "ll", "o"]</span>
</code></pre>
<p>30.追加字符串到指定字符串最前面</p>
<pre class="hljs ruby"><code class="ruby">a = <span class="hljs-string">"world"</span> a.prepend(<span class="hljs-string">"hello "</span>) <span class="hljs-comment">#=&gt; "hello world" </span>
a <span class="hljs-comment">#=&gt; "hello world"</span>
</code></pre>
<p>31.反转字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"stressed"</span>.reverse <span class="hljs-comment">#=&gt; "desserts"</span>
</code></pre>
<p>32.分割字符串</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">" now's  the time"</span>.split <span class="hljs-comment">#=&gt; ["now's", "the", "time"] </span>
<span class="hljs-string">" now's  the time"</span>.split(<span class="hljs-string">' '</span>) <span class="hljs-comment">#=&gt; ["now's", "the", "time"] </span>
<span class="hljs-string">" now's  the time"</span>.split(<span class="hljs-regexp">/ /</span>) <span class="hljs-comment">#=&gt; ["", "now's", "", "the", "time"]</span>
<span class="hljs-string">"1, 2.34,56, 7"</span>.split(<span class="hljs-regexp">%r{,\s*}</span>) <span class="hljs-comment">#=&gt; ["1", "2.34", "56", "7"] </span>
<span class="hljs-string">"hello"</span>.split(<span class="hljs-regexp">//</span>) <span class="hljs-comment">#=&gt; ["h", "e", "l", "l", "o"] </span>
<span class="hljs-string">"hello"</span>.split(<span class="hljs-regexp">//</span>, <span class="hljs-number">3</span>) <span class="hljs-comment">#=&gt; ["h", "e", "llo"] </span>
<span class="hljs-string">"hi mom"</span>.split(<span class="hljs-regexp">%r{\s*}</span>) <span class="hljs-comment">#=&gt; ["h", "i", "m", "o", "m"] </span>
<span class="hljs-string">"mellow yellow"</span>.split(<span class="hljs-string">"ello"</span>) <span class="hljs-comment">#=&gt; ["m", "w y", "w"] </span>
<span class="hljs-string">"1,2,,3,4,,"</span>.split(<span class="hljs-string">','</span>) <span class="hljs-comment">#=&gt; ["1", "2", "", "3", "4"] </span>
<span class="hljs-string">"1,2,,3,4,,"</span>.split(<span class="hljs-string">','</span>, <span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; ["1", "2", "", "3,4,,"] </span>
<span class="hljs-string">"1,2,,3,4,,"</span>.split(<span class="hljs-string">','</span>, -<span class="hljs-number">4</span>) <span class="hljs-comment">#=&gt; ["1", "2", "", "3", "4", "", ""] </span>
<span class="hljs-string">""</span>.split(<span class="hljs-string">','</span>, -<span class="hljs-number">1</span>)* <span class="hljs-comment">#=&gt; []*</span>
<span class="hljs-string">"World"</span>.split(<span class="hljs-string">'r'</span>)[<span class="hljs-number">0</span>]<span class="hljs-comment">#=&gt; "Wo"</span>
<span class="hljs-string">"World"</span>.split(<span class="hljs-string">'r'</span>)[<span class="hljs-number">1</span>]<span class="hljs-comment">#=&gt; "ld"</span>
</code></pre>
<p>33.去除字符串中重复的字符</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"yellow moon"</span>.squeeze <span class="hljs-comment">#=&gt; "yelow mon" </span>
<span class="hljs-string">"  now  is  the"</span>.squeeze(<span class="hljs-string">" "</span>) <span class="hljs-comment">#=&gt; " now is the" </span>
<span class="hljs-string">"putters shoot balls"</span>.squeeze(<span class="hljs-string">"m-z"</span>) <span class="hljs-comment">#=&gt; "puters shot balls"</span>
</code></pre>
<p>34.大小写互换</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"Hello"</span>.swapcase <span class="hljs-comment">#=&gt; "hELLO"</span>
<span class="hljs-string">"cYbEr_PuNk11"</span>.swapcase <span class="hljs-comment">#=&gt; "CyBeR_pUnK11"</span>
</code></pre>
<p>35.字符串转大写</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"hEllO"</span>.upcase <span class="hljs-comment">#=&gt; "HELLO"</span>
</code></pre>
<p>36.字符串循环</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"a8"</span>.upto(<span class="hljs-string">"b6"</span>) {<span class="hljs-params">|s|</span> print s, <span class="hljs-string">' '</span> } <span class="hljs-comment">#=&gt; a8 a9 b0 b1 b2 b3 b4 b5 b6</span>
<span class="hljs-string">"9"</span>.upto(<span class="hljs-string">"11"</span>).to_a <span class="hljs-comment">#=&gt; ["9", "10", "11"] </span>
<span class="hljs-string">"25"</span>.upto(<span class="hljs-string">"5"</span>).to_a <span class="hljs-comment">#=&gt; [] </span>
<span class="hljs-string">"07"</span>.upto(<span class="hljs-string">"11"</span>).to_a <span class="hljs-comment">#=&gt; ["07", "08", "09", "10", "11"]</span>
</code></pre>
<p>37.判断字符串编码是否正确</p>
<pre class="hljs ruby"><code class="ruby"><span class="hljs-string">"\xc2\xa1"</span>.force_encoding(<span class="hljs-string">"UTF-8"</span>).valid_encoding? <span class="hljs-comment">#=&gt; true </span>
<span class="hljs-string">"\xc2"</span>.force_encoding(<span class="hljs-string">"UTF-8"</span>).valid_encoding? <span class="hljs-comment">#=&gt; false </span>
<span class="hljs-string">"\x80"</span>.force_encoding(<span class="hljs-string">"UTF-8"</span>).valid_encoding? <span class="hljs-comment">#=&gt; false</span>
</code></pre>

          </div>
<html>

怎么抓住机会
----------

> 机会是给有准备的人准备的。

苏明玉在抓住老蒙这次机会之后，日复一日，早上5点半起床学习，当机会来到你面前，能否成功抓住全靠自己日积月累的知识储备了。

对我最惊醒的是，日复一日，早上五点半，我呢？

思考一下，什么途径才能让自己快速挣到1000万，出国工作。这就需要英语储备、编程储备。

此外像是法律、医学、音乐，这些都是自己感兴趣的，这些日积月累的知识储备会让自己变得饱满。

如果我想出国，一个自己的不成熟想法。

1、学习基础和算法  2、加入外企公司  3、出国工作

主要是积累和态度。

所以自己以后也能日复一日，早上六点，并不是没有娱乐，而是娱乐也要让自己健康的娱乐，不要耽误第二天的事情。

鱼与熊掌不能兼得，这是一个成年人的思考方式，如果做不到，选择反面也不会有人嘲笑，因为芸芸众生，嘲笑别人也是嘲笑自己。
