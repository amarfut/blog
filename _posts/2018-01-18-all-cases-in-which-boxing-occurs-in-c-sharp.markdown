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


<article>

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

  
</article>
