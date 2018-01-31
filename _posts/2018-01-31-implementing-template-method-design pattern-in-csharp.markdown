---
layout: post
title:  "Implementing Template Method Design Pattern in C#"
publishdate:   "Wednesday, 31 January 2018"
tags: [csharp, designpatterns]
permalink: implementing-template-method-design-pattern-in-csharp
preview: 'Implementing Template Method Design Pattern in C#'
---
<p>
The Template Method allows to define a skeleton of an algorithm in a base class and allow subclasses to redefine certain steps. This pattern can be used anytime you have a few similar algorithms that differ by some steps.
</p>

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
<p>
Nothing interesting here. We've defined ValidationResult class that will represent the result of validation. It can be successful or unsuccessful with some error message.
</p>


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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;_fileInfo.Length&nbsp;&gt;&nbsp;0&nbsp;?&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult()&nbsp;:&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult(<span class="String">"The&nbsp;proposed&nbsp;file&nbsp;content&nbsp;is&nbsp;empty."</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">protected</span>&nbsp;<span class="Modifier">abstract</span>&nbsp;ValidationResult&nbsp;IsFileSizeValid();<br />
}
</div>
<p>
Validate method (our template method) is calling IsFileEmpty and IsFileSizeValid one by one. In the real world there would be much more other methods (IsFileNamevalid, IsExtensionValid etc) but in this example we will proceed only with these two ones.

<br/><br/>IsFileEmpty should work in the same way for file types, because it simply checks whether file has at least one byte inside. Thats why this method is places in base class.

Next method IsFileSizeValid 
</p>

<h4>Step #3: Implementing TextFileValidator class</h4>
<div class="code">
<span class="Modifier">public</span>&nbsp;<span class="ReferenceType">class</span>&nbsp;TextFileValidator&nbsp;:&nbsp;BaseValidator<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;TextFileValidator(FileInfo&nbsp;fileInfo)&nbsp;:&nbsp;<span class="Keyword">base</span>(fileInfo)&nbsp;{&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">private</span>&nbsp;<span class="Modifier">const</span>&nbsp;<span class="ValueType">int</span>&nbsp;MAX_FILE_SIZE_BYTES&nbsp;=&nbsp;1048576;<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">protected</span>&nbsp;<span class="Modifier">override</span>&nbsp;ValidationResult&nbsp;IsFileSizeValid()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;_fileInfo.Length&nbsp;&lt;=&nbsp;MAX_FILE_SIZE_BYTES&nbsp;?<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult()&nbsp;:<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult(<span class="String">"The&nbsp;file&nbsp;size&nbsp;is&nbsp;greater&nbsp;than&nbsp;1&nbsp;MB"</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
}
</div>

<h4>Step #4: Implementing ImageFileValidator class</h4>
<div class="code">
<span class="Modifier">public</span>&nbsp;<span class="ReferenceType">class</span>&nbsp;ImageFileValidator&nbsp;:&nbsp;BaseValidator<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">public</span>&nbsp;ImageFileValidator(FileInfo&nbsp;fileInfo)&nbsp;:&nbsp;<span class="Keyword">base</span>(fileInfo)&nbsp;{&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">private</span>&nbsp;<span class="Modifier">const</span>&nbsp;<span class="ValueType">int</span>&nbsp;MIN_FILE_SIZE_BYTES&nbsp;=&nbsp;1048576;<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">private</span>&nbsp;<span class="Modifier">const</span>&nbsp;<span class="ValueType">int</span>&nbsp;MAX_FILE_SIZE_BYTES&nbsp;=&nbsp;10485760;<br />
&nbsp;&nbsp;&nbsp;&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">protected</span>&nbsp;<span class="Modifier">override</span>&nbsp;ValidationResult&nbsp;IsFileSizeValid()<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ValidationResult&nbsp;result&nbsp;=&nbsp;<span class="Keyword">null</span>;<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">if</span>&nbsp;(_fileInfo.Length&nbsp;&lt;&nbsp;MIN_FILE_SIZE_BYTES)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult(<span class="String">"The&nbsp;file&nbsp;size&nbsp;is&nbsp;less&nbsp;than&nbsp;1&nbsp;MB"</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">else</span>&nbsp;<span class="Statement">if</span>&nbsp;(_fileInfo.Length&nbsp;&gt;&nbsp;MAX_FILE_SIZE_BYTES)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult(<span class="String">"The&nbsp;file&nbsp;size&nbsp;is&nbsp;greater&nbsp;than&nbsp;1&nbsp;MB"</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">else</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;ValidationResult();<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">return</span>&nbsp;result;<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
}
</div>

<h4>Step #5: Usage</h4>
<div class="code">
<span class="ReferenceType">class</span>&nbsp;Program<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="Modifier">static</span>&nbsp;<span class="ValueType">void</span>&nbsp;Main(<span class="ReferenceType">string</span>[]&nbsp;args)<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileInfo&nbsp;fileInfo&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;FileInfo(<span class="String">"test.txt"</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BaseValidator&nbsp;validator&nbsp;=&nbsp;<span class="Keyword">new</span>&nbsp;TextFileValidator(fileInfo);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ValidationResult&nbsp;result&nbsp;=&nbsp;validator.Validate();<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">if</span>&nbsp;(result.IsSuccessful)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Console.WriteLine(<span class="String">"The&nbsp;file&nbsp;is&nbsp;valid."</span>);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="Statement">else</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Console.WriteLine(result.ErrorMessage);<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
}
</div>
