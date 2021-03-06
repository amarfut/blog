---
layout: post
title:  "Boxing occurrences in C#"
publishdate:   "Monday, 18 January 2018"
tags: [csharp]
permalink: boxing-occurrences-in-c-sharp
preview: '
This article is just quick recap of majority of cases in which boxing occurs in C# 6.0 or higher.
Boxing occurs everywhere when we need a reference type to a value type. Here is the most popular example:

<div class="code">
<span class="ValueType">int</span>&nbsp;number&nbsp;=&nbsp;123;&nbsp;&nbsp;<br />
<span class="ReferenceType">object</span>&nbsp;boxed&nbsp;=&nbsp;number;
</div>
<p>
But what about another cases that are not so obvious? Let’s consider them.</p>
'
---

<p>
  This article is just quick recap of majority of cases in which boxing occurs in C#6.0 or higher.
</p>

<p>
Boxing occurs everywhere when we need a reference type to a value type. Here is the most popular example:  
</p>

<div class="code">
<span class="ValueType">int</span>&nbsp;number&nbsp;=&nbsp;123;&nbsp;&nbsp;<br />
<span class="ReferenceType">object</span>&nbsp;boxed&nbsp;=&nbsp;number;
</div>

<p>
  But what about another cases that are not so obvious? Let’s consider them.
 </p>

  <div class="code">
<span class="Modifier">public</span>&nbsp;<span class="ValueType">enum</span>&nbsp;Color<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;RED&nbsp;=&nbsp;1,<br />
&nbsp;&nbsp;&nbsp;&nbsp;GREEN&nbsp;=&nbsp;2,<br />
&nbsp;&nbsp;&nbsp;&nbsp;BLACK&nbsp;=&nbsp;3<br />
}<br />
<br />
<span class="Modifier">public</span>&nbsp;<span class="ReferenceType">interface</span>&nbsp;IPrintable<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="ValueType">void</span>&nbsp;Print();<br />
}<br />
<br />
<span class="Modifier">public</span>&nbsp;<span class="ValueType">struct</span>&nbsp;Number&nbsp;:&nbsp;IPrintable<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;<span class="ValueType">int</span>&nbsp;Value&nbsp;{&nbsp;get;&nbsp;set;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;<span class="ValueType">void</span>&nbsp;Print()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Console.WriteLine(<span class="String">"Value&nbsp;=&nbsp;{0}"</span>,&nbsp;<span class="Keyword">this</span>.Value);<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;<span class="Modifier">override</span>&nbsp;<span class="ReferenceType">string</span>&nbsp;ToString()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;<span class="Keyword">base</span>.ToString();<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
}
</div>


<h4>#1 Converting value type to System.Object type:</h4>

<div class="code">
<span class="ReferenceType">object</span>&nbsp;number&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;Number();
</div>

<h4>#2 Converting value type to System.ValueType type:</h4>

<div class="code">
System.ValueType&nbsp;number&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;Number();
</div>

<h4>#3 Converting value of enumeration type to System.Enum type:</h4>

<div class="code">
System.Enum&nbsp;color&nbsp;=&nbsp;Color.GREEN;
</div>

<h4>#4 Converting value type into interface reference:</h4>

<div class="code">
IPrintable&nbsp;number&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;Number();
</div>

<h4>#5 Using value types in C# string concatenation:</h4>

<div class="code">
Action&nbsp;print&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;Number().Print;
</div>

<h4>#6 Calling Object.GetType method on value types:</h4>

<div class="code">
Color.BLACK.GetType();<br />
<span class="InlineComment">//OR</span><br />
<span class="Keyword">new</span>&nbsp;Number().GetType();
</div>

<h4>#7 Calling a base class method from a struct:</h4>

<div class="code">
<span class="Linq">var</span>&nbsp;number&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;Number();<br />
<span class="Linq">var</span>&nbsp;str&nbsp;=&nbsp;number.ToString();
</div>

<h4>#8 Constant patterns under is expression:</h4>

<div class="code">
<span class="ValueType">int</span>&nbsp;value&nbsp;=&nbsp;1;<br />
<span class="Statement">if</span>&nbsp;(value&nbsp;<span class="Keyword">is</span>&nbsp;1)&nbsp;{&nbsp;<span class="InlineComment">//both&nbsp;‘value’&nbsp;and&nbsp;‘1’&nbsp;will&nbsp;be&nbsp;boxed</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;Console.WriteLine(<span class="String">"true"</span>);<br />
}
</div>

<p>
Memorizing the examples above doesn’t guarantee you will never get undesirable boxing. It’s important to be quite attentive to every piece of code you write. Also it would be great to check the code in Ildasm or dotPeek tools.
</p>


