---
layout: post
title: Blueprint CSS and the ArcGIS Javascript API
date: '2011-04-25 11:00:17'
---

This is one of those things that I'm posting so that I can remember it in the future, but maybe it will help somebody else too.

<a href="http://www.blueprintcss.org/">Blueprint</a> is a css framework I sometimes use. It's easy to understand and use. But I've noticed that it stomps on some ArcGIS Javascript API styles and messes up the zoom slider. Also, don't forget to assign the appropriate class to the body or some appropriate element so the JSAPI styles get applied!

Specifically, some of the table related styles in Blueprint's screen.css file are to blame. Generally, I just get rid of all of them but it you want to get surgical about it, the following are the offending declarations:
<pre>table {margin-bottom:1.4em;width:100%;}</pre>
and
<pre>tbody tr:nth-child(even) td, tbody tr.even td {background:#e5ecf9;}</pre>