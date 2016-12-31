---
layout: post
title: Singletracker.net - the technology
date: '2013-03-18 12:00:27'
---

Just a brief post to quickly call out the technologies I'm using in <a href="http://singletracker.net">Singletracker.net</a>. In the coming weeks, I'll try to put up some more posts that get more into the details.

For starters, It's running in <a href="http://www.windowsazure.com/en-us/">Windows Azure</a> which so far has been really easy to deal with but is going to get too expensive once my 90 day free trial is up.

On the back end it's <a href="http://www.asp.net/mvc">Asp.NET MVC 4</a> with <a href="http://www.asp.net/web-api">WebAPI</a> and <a href="http://msdn.microsoft.com/en-us/data/ef.aspx">Entity Framework 5</a>. For authentication, I'm using OpenID with Facebook and Google. I plan to add Yahoo and Microsoft soon. If people actually use this, I will probably re-build the back end with <a href="http://nodejs.org/">node.js</a>, <a href="http://expressjs.com/">express</a>, and <a href="http://www.mongodb.org/">MongoDB</a>.

On the front end, it's <a href="http://twitter.github.com/bootstrap/">Twitter Bootstrap</a> for the responsive layout and other goodness. I'm using <a href="http://fortawesome.github.com/Font-Awesome/">Font Awesome</a> for the icons throughout - I think there is probably not a single image (except the map tiles of course). For the UI, I'm using <a href="http://backbonejs.org/">Backbone.js</a> and <a href="http://leafletjs.com/">Leaflet.js</a> with <a href="http://www.opencyclemap.org/">OpenCycleMap's</a> tiles. 