---
layout: post
title:  "Implementing Template Method Design Pattern in C#"
publishdate:   "Wednesday, 31 January 2018"
tags: [csharp, designpatterns]
permalink: implementing-template-method-design pattern-in-csharp
preview: 'Implementing Template Method Design Pattern in C#'
---
Implementing Template Method Design Pattern in C#

<h4>Step #1: Preparing ValidationResult class</h4>
<div class="code">
<span class="Modifier">public</span>&nbsp;<span class="ReferenceType">class</span>&nbsp;ValidationResult<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;ValidationResult()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsSuccessful&nbsp;=&nbsp;<span class="Keyword">true</span>;<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;ValidationResult(<span class="ReferenceType">string</span>&nbsp;error)<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ErrorMessage&nbsp;=&nbsp;error;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsSuccessful&nbsp;=&nbsp;<span class="Keyword">false</span>;<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;<span class="ValueType">bool</span>&nbsp;IsSuccessful&nbsp;{&nbsp;get;&nbsp;set;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;<span class="ReferenceType">string</span>&nbsp;ErrorMessage&nbsp;{&nbsp;get;&nbsp;set;&nbsp;}<br />
}
</div>

<h4>Step #2: Implementing BaseValidator class</h4>
<div class="code">
<span class="Modifier">public</span>&nbsp;<span class="Modifier">abstract</span>&nbsp;<span class="ReferenceType">class</span>&nbsp;BaseValidator<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">protected</span>&nbsp;<span class="Modifier">readonly</span>&nbsp;FileInfo&nbsp;_fileInfo;<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;BaseValidator(FileInfo&nbsp;fileInfo)<br />
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_fileInfo&nbsp;=&nbsp;fileInfo;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;ValidationResult&nbsp;Validate()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Linq">var</span>&nbsp;validateActions&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;List&lt;Func&lt;ValidationResult&gt;&gt;()<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsFileEmpty,<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsFileSizeValid<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">foreach</span>&nbsp;(<span class="Linq">var</span>&nbsp;validateAction&nbsp;<span class="Statement">in</span>&nbsp;validateActions)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ValidationResult&nbsp;result&nbsp;=&nbsp;validateAction();<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">if</span>&nbsp;(!result.IsSuccessful)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;result;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult();<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">private</span>&nbsp;ValidationResult&nbsp;IsFileEmpty()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;_fileInfo.Length&nbsp;&gt;&nbsp;0&nbsp;?&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult()&nbsp;:&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult(<span class="String">"The&nbsp;proposed&nbsp;file&nbsp;content&nbsp;is&nbsp;empty."</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">protected</span>&nbsp;<span class="Modifier">abstract</span>&nbsp;ValidationResult&nbsp;IsFileSizeValid();<br />
}
</div>
