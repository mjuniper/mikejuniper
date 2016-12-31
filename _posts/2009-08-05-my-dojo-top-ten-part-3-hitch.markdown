---
layout: post
title: 'My dojo top ten (part 3: hitch)'
date: '2009-08-05 11:46:12'
---

I think I will call my <a href="http://www.mikejuniper.com/2009/02/extending-dojoquery/" target="_blank">series</a> <a href="http://www.mikejuniper.com/2009/02/extending-dojo-query-part-2/" target="_blank">of</a> <a href="http://www.mikejuniper.com/2009/08/dojo-query-putting-it-all-together/" target="_blank">posts</a> dealing with dojo.query and NodeList part two of my dojo top ten and move on to part three.

I've come to rely heavily on dojo.hitch but at first I didn't understand it at all. And I haven't found many good explanations of it online (except <a href="http://dojocampus.org/content/2008/03/15/jammastergoat-dojohitch/" target="_blank">this one</a>) so I'm going to try my hand at a simple concise explanation of hitch.

Here we go: hitch returns a function in which 'this' will be whatever is specified as the first argument to hitch.

I'm now using it elsewhere but hitch is particularly useful with callback functions. So here's an example:
<pre>dojo.xhrGet({
  url: 'http://mywebservice.com/helloworld',
  load: helloworldCallback</pre>
So we're calling a webservice and specifying the callback function as helloworldCallback. This is all well and good but if you try to use 'this' in helloworldCallback, say to call other functions in the same module, it may not be what you expect it to be. Enter hitch:
<pre>dojo.xhrGet({
  url: 'http://mywebservice.com/helloworld',
  load: dojo.hitch(this, 'helloworldCallback')</pre>
By using hitch in the above example, we guarantee that when we hit the callback function, 'this' will refer to what we expect it to (in this case the module that contains both helloworldCallback and the function that does our webservice call).