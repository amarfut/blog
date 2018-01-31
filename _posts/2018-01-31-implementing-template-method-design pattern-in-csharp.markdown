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
