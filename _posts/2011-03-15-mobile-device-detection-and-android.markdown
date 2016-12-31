---
layout: post
title: Mobile Device Detection and Android
date: '2011-03-15 11:39:16'
---

We've been doing more and more apps for mobile devices. Specifically we've built some apps recently that we want to work from desktop browsers, tablets, and smartphones.

I've heard the argument that you should just serve good semantic html and style it appropriately for the device and be done with it. But we're not building content driven websites, we're building data driven web apps often containing maps. We need to give the user an entirely different experience on a phone vs a desktop browser.

We're using a custom mobile view engine in ASP.NET MVC (based on the one in <a href="http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx">this</a> Scott Hanselman post) to serve the appropriate view for the requesting device. That's been working great but one problem I've run into is how to distinguish an Android tablet from an Android phone. I finally found the solution to this problem on the <a href="http://android-developers.blogspot.com/2010/12/android-browser-user-agent-issues.html">Android Developers Blog</a>. Basically, if a device runs android, it should contain the string "android" in the User-Agent string. Google recommends that "manufactures of large-form-factor devices (where the user  may prefer the standard web site over a mobile optimized version)  remove 'Mobile' from the User Agent". That would be great except that Motorola has already screwed this up by including "Mobile" in the User-Agent of the Xoom.

The Request.Browser object in ASP.NET has a bunch of properties that tell you about the requesting device. But it's not really sufficient (and some of it is just plain wrong or absent) for accurately identifying the capabilities of mobile devices. So we've started using <a href="http://51degrees.codeplex.com/">51degrees.mobi</a>. It uses the <a href="http://wurfl.sourceforge.net/">WURFL</a> database, which seems to be the most complete and up to date database of mobile devices, to populate tons of useful info on the Request.Browser object. One thing I didn't realize at first is that besides accurately populating the existing properties of the Request.Browser object, it also makes available many of the other properties exposed by WURFL. They can be accessed like so: Request.Browser["is_tablet"]. We're now using that in conjunction with the custom mobile view engine.

There are two drawbacks to the 51degrees.mobi component: It adds some overhead to every request. I haven't really seen a lag but it's gotta cost something. The more significant drawback is that you have to manually update the WURFL XML file.

<strong>Update: </strong>I don't know if I was mistaken about the Xoom's User-Agent or if Motorola has fixed it in a later revision but I now own a Xoom and the User-Agent does not contain "mobile" (which is correct, it should not). Also, the 51Degrees.mobi component is now available via <a href="http://nuget.codeplex.com/">NuGet</a>, which, if you use Visual Studio, you should be using.