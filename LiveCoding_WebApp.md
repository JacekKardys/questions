Task:
Create a web application which will expose one GET endpoint which will return result of string
operation:
/validator?operation=&lt;equals/permutations&gt;&amp;input1=&lt;string&gt;&amp;input2=&lt;string&gt;
Acceptance criteria:
● Use https://start.spring.io/ to generate project quickly
● Expected json (200 OK):
{
&quot;operation&quot;: &quot;EQUALS/PERMUTATIONS&quot;,
&quot;result&quot;: true
}
● Return input1 and input2 request params in response headers
● Check equality validation: Check if provided string are the same
● Check permutation validation: Given two strings, write a method to decide if one is a
permutation of the other. For example:
○ input1=aa&amp;input2=aa =&gt; true
○ input1=ekolimk&amp;input2=lkkieom =&gt; true
○ input1=alap&amp;input2=eple =&gt; false