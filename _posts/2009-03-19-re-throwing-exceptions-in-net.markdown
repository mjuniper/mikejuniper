---
layout: post
title: Re-throwing exceptions in .Net
date: '2009-03-19 13:35:04'
---

I knew that I read somewhere that there was a very important difference between throw; and throw ex; when you catch an exception and want to re throw it. But I couldn't remember what the difference was. Thanks to <a href="http://www.tkachenko.com/blog/archives/000352.html" target="_blank">this post</a> I now know. If you use throw ex, the stack trace info will be overriden. So it is almost always preferable to use just throw.