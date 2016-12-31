---
layout: post
title: Extending dojo query part 2
date: '2009-02-25 09:45:41'
---

One of the cool things about the dojo NodeList is that most (all?) of the methods of NodeList return the list so you can chain NodeList operations ala jQuery. I realized that I had overlooked this with my removeAttr method when I wrote this:
<pre>
dojo.query('#okConfirmBtn', dialogId).removeAttr('disabled').
  connect('onclick', clickFunc);
</pre>
<br/>
This would fail because removeAttr doesn't return anything. It's a simple change though, here's the updated code:

<pre>
//extend nodelist to have a removeAttr function
dojo.extend(dojo.NodeList, {
  removeAttr: function(attribute){
    return this.forEach(function(item) { 
      dojo.removeAttr(item, attribute); 
    });
  }
});
</pre>
<br/>

I also created another extension to the NodeList, a widgets method. This filters the NodeList to only nodes that are widgets and returns a NodeList of the widgets (not the dom nodes, the actual widgets).

<pre>
/*extend nodelist to have a widgets function that 
filters the list and returns a nodelist of dijit widgets*/
dojo.extend(dojo.NodeList, {
  widgets: function(){
    return this.filter(function(item)
    {
      var widget = dijit.byNode(item);
      if (widget) { return true; }
    }).map(function(widget) { return dijit.byNode(widget); });
  }
});
</pre>