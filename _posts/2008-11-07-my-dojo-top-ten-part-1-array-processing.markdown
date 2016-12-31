---
layout: post
title: 'My dojo top ten (part 1: array processing)'
date: '2008-11-07 14:45:39'
---

I've been using the <a href="http://dojotoolkit.org/" target="_blank">dojo toolkit</a> a lot lately and I've come to really like it. Except for the documentation. <a href="http://dojotoolkit.org/book/dojo-book-1-0" target="_blank">The Book of Dojo</a> is pretty good (though not comprehensive enough) but the <a href="http://api.dojotoolkit.org/" target="_blank">api documentation</a> sucks. There are a few resources on the web that are helpful but we've been relying heavily on two books: <a href="http://www.amazon.com/Dojo-Definitive-Guide-Matthew-Russell/dp/0596516487" target="_blank">Dojo the Definitive Guide</a> and <a href="http://www.amazon.com/Mastering-Dojo-JavaScript-Experiences-Programmers/dp/1934356115" target="_blank">Mastering Dojo</a>.

So this is the first in a series of posts on my top ten dojo functions.

Dojo's array processing functions basically make Javascript 1.6 array functions available in all browsers. These are extremely powerful functions that let you filter, transform, and query arrays.

I'm going to skip indexOf, lastIndexOf, some, every, and forEach (which are great, go look them up) and focus on map, and filter.

I've used dojo.map alot lately to transform the elements in an array. A somewhat contrived example: You get some json data back from a web service that is an array of objects. You've got to pass that array to another function but it expects each element of the array to have a name property instead of the title property they've got. Just call
<pre>var newAry =
  dojo.map(ary, function(x){ x.name = x.title; return x; });</pre>
and voila, you've got a new array, and each element now has a name property that is the same as the title property.

I've also been using dojo.filter to filter arrays. Say you've got an array of objects and you want to work with a subset of them. You can use dojo.filter to filter the array. Just pass it the array and a function that evaluates each element and returns true if it should be included in the filtered array and false if not. Using the above example, lets say we want to return all elements in the array whose title starts with 'A'. Just do this:
<pre>var newAry =
  dojo.filter(ary, function(x){ return x.title.indexOf('A') == 0; });</pre>
As I said, these are extremely powerful functions and I've only scratched the surface in this post.