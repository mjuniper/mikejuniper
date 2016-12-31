---
layout: post
title: Introducing Singletracker - crowd-sourced trail conditions for mountain bikers
date: '2013-03-17 12:04:42'
---

So a few weeks ago I wanted to go mountain biking. I wasn't sure the trails would be fit to ride on so I went to the websites of the various trails looking for current trail condition info. It was either non-existent or hopelessly out of date.

So I got to thinking, it would be great to have a crowd sourced trail conditions web app. So I built it!

It's a fairly responsive design - intended to be used on mobile phones (recent versions of iOS and Android) but it should work very well on desktop browsers and tablets. Note that it is intended to be used on modern devices/ browsers - I give ielt9 the Heisman.

V1 is now live. I'm representing the trails as points (think of them as the trail-head) because it's far easier for a whole bunch of reasons. I'm thinking about ways to handle them as polylines but that's down the road a ways. I want to get it out there, try to get people using it, get their feedback and see where that takes us.

The zen of it is:
<ol>
<li>You land at the page and get a map centered on your location.</li>
<li>If there are any trails that have been entered nearby, they will appear as points with different symbols for good, fair, poor, or unknown condition.</li>
<li>You can click/ tap on a point to get a popup containing a bit more information and a button that takes you to the trail details page.</li>
<li>You can also click 'List' in the menu to get a list of all the trails in the system sorted by distance from your location.</li>
<li>If you click/tap an item in the list, you go to the trail details page.</li>
<li>On the trail details page there is info about the trail and its condition as well as buttons to: update the trail condition, edit the trail info, or view the trail on the map.</li>
</ol>

Check it out <a href="http://singletracker.net" title="singletracker.net">singletracker.net</a>.