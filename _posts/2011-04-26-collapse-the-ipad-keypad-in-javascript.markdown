---
layout: post
title: Collapse the iPad Keypad in Javascript
date: '2011-04-26 12:29:52'
---

I recently built a web application with a search field which geolocates whatever you typed into that field and zooms you to the results. We were severely constrained on UI space so we went without a search button and we just do the search when the user hits the enter key.

This worked fine on the iPad except that the keypad didn't collapse. Of course, you can't directly control the keypad in Javascript but I had a hunch that if I just removed focus from the search field, it would collapse. And it worked! Code (jQuery) below:
<pre>$(this).blur();</pre>