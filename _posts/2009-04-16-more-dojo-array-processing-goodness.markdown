---
layout: post
title: More dojo array processing goodness
date: '2009-04-16 07:12:16'
---

I use dojo.forEach everywhere. One thing that I occasionally need that dojo.forEach can't do (as far as I know) is break out of the loop early. Thanks to <a href="http://lazutkin.com/blog/2008/jan/12/functional-fun-javascript-dojo/">Eugene Lazutkin</a> I realized that I can just use dojo.some or dojo.every to break early. By the way, you should read that whole article - there's lots of good stuff there.

Let's say that you've got an array of objects and you need to know the index of one of the objects in the array based on a property value. Consider these two functions that will both accomplish this:
<pre>GetArrayItemIndex: function(ary, item, fieldName)
{
    var i = -1, result;
    dojo.forEach(ary, function(aryItem)
    {
        i++;
        if (aryItem[fieldName] === item[fieldName]) { result = i; }
    });

    return result;
}

GetArrayItemIndex2: function(ary, item, fieldName)
{
    //using some instead of forEach to break early
    var i = -1;
    dojo.some(ary, function(aryItem)
    {
        i++;
        return (aryItem[fieldName] === item[fieldName]);
  });

      return i;
}</pre>
GetArrayItemIndex uses dojo.forEach so it will go through every item in the array even if the item we're looking for is at index 0. GetArrayItemIndex2 uses dojo.some to short-circuit the loop when we locate the item we're looking for. I've been wondering about this for a while - I'm glad I finally googled it today and discovered Eugene's post.