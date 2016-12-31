---
layout: post
title: Extending dojo.query
date: '2009-02-19 16:12:01'
---

I've been making more and more use of dojo.query. In case you're not familiar with it, it basically allows you to query the dom using css syntax. It returns a NodeList which is an array of dom nodes with some extra dojo goodness built in. Like many (all?) of the dojo array processing functions, some of which I discussed <a href="http://www.mikejuniper.com/2008/11/my-dojo-top-ten-part-1-array-processing/">here</a>. NodeList also has some handy methods like addClass, removeClass, toggleClass, style, addContent, and a few others. NodeList.attr will set an attribute on every node in the nodelist. Very handy, but there's no NodeList.removeAttr, I'm not sure why. I needed this functionality in a bunch of places. At first I was just using NodeList.forEach, but then I realized it would be better to just extend NodeList like so:
<pre>
dojo.extend(dojo.NodeList, {
  removeAttr: function(attribute){
    this.forEach(function(x) { dojo.removeAttr(x, attribute); });
  }
});
</pre>
<br/>
That little bit of code adds the removeAttr function to NodeList! Now I can just do this:
<pre>
dojo.query('foo').removeAttr('bar');
</pre>
<br/>
I'm doing something else with this that I think is interesting. I'm using alternative syntax (that probably has a name but I don't know what it is) to reduce the amount of code I'm writing. For instance, instead of writing this:
<pre>
if (valid)
{
  dojo.query('.detailSaveButton').removeAttr('disabled');
}
else
{
  dojo.query('.detailSaveButton').attr('disabled', 'disabled');
}
</pre>
<br/>
I'm doing this:
<pre>
var action = (valid) ? 'removeAttr' : 'attr';
dojo.query('.detailSaveButton')[action]('disabled', 'disabled');
</pre>
<br/>
Note that the second argument being passed in will be ignored if action == 'removeAttr'.