---
layout: post
title: Accessing ASP.NET Web Services with Dojo
date: '2008-07-29 08:19:27'
---

A while back, <a href="http://blog.davebouwman.net" target="_blank">Dave</a> wrote a <a href="Accessing ASP.NET Web Services with dojo.xhrGet" target="_blank">post</a> on this subject. Last week, I decided to pick up where he left off.

I've got some web services that take parameters so I decided to try to figure out how to pass the parameters up using dojo.xhrGet. You'll have to check out Dave's post for the fundamentals but here's what I did. I created a criteria object with the parameters that I needed to pass to the webservice (the vals object was returned by a call to my dojo dialog's getValues method):

var criteria = { "projectName":'"' + vals.projectName + '"', "bufferDistance":vals.bufferDistance };

then I added the criteria to my dojo.xhrGet (note the 'content: criteria' part):
<pre>dojo.xhrGet({
    url: 'Services/FuelsSearchService.asmx/GetByProject',
    handleAs: 'json',
    contentType: "application/json; charset=utf-8",
    content: criteria,
    load: handleFuelsResults,
    error: callbackError,
    timeout: 10000
});
</pre>
And it works!

Well, not right away. See those funky extra quotes around the projectName value? Well it wouldn't work until I added those. Even though you and I know that vals.projectName is a string, dojo knows it's a string, and javascript knows it's a string, Microsoft's javascript deserialization stuff apparently doesn't know that. I kept getting "Error: bad http response code:500". But when I actually looked at the response in Firebug, it said, "Invalid JSON primitive: Cities." Cities is the value of vals.projectName. So I added the extra quotes and voila, it works. But it's ugly. Anybody know another way?