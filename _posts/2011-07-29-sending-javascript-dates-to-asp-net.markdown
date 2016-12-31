---
layout: post
title: Sending javascript dates to Asp.NET
date: '2011-07-29 12:04:04'
---

I've struggled with this at various times and come up with a number of solutions, none of which were entirely satisfying. Now, thanks to <a href="http://stackoverflow.com/questions/6702705/how-to-convert-javascript-datetime-to-c-datetime/6702719#6702719">Stackoverflow</a>, I found a really nice solution:

Javascript:
<pre>
obj.date = new Date().toUTCString();
</pre>

C#:
<pre>
DateTime date= DateTime.ParseExact(obj.date,
                                  "ddd, MMM d yyyy HH:mm:ss GMT",
                                  CultureInfo.InvariantCulture);

</pre>