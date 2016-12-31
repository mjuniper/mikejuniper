---
layout: post
title: iPad and Motorola Xoom – a tale of two browsers
date: '2011-04-26 09:16:04'
---

We recently built a web application (several actually) that we needed to work well on tablets. Since the first Android 3.0 (Honeycomb) tablet, the Motorola Xoom, is now out (and since I have one) we were specifically targeting the iPad and the Xoom. Though they both use Webkit based browsers, I ran into a couple of interesting differences between the two.

First off, I used -webkit-user-select:none; to prevent selection of various elements in the ui. This worked flawlessly in Mobile Safari on the iPad but Chrome on Android does not seem to respect it at all.

I also ran into a couple of issues using the ArcGIS Javascript API on the Xoom. Since Android doesn't support SVG, the JSAPI uses canvas and there are evidently some small problems with their canvas implementation. TextSymbol doesn't work and neither do any of the fill styles for simple fill symbol except solid and null. Overall the JSAPI works great on the Xoom, though.

I'm told that the text symbol issue will probably be fixed in the 2.3 release.