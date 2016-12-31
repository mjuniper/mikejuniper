---
layout: post
title: Single finger scrolling in Mobile Safari
date: '2011-06-21 13:01:11'
---

We built the <a href="http://uc.dtsagile.com">BackChannel</a> using the Mobile View Engine I discussed in <a href="http://www.mikejuniper.com/2011/03/mobile-device-detection-and-android/">this</a> post. If you hit the site with a phone we serve a mobile version, but if you hit it with a tablet, we serve the full version, which, of course, we expect to work. For the <a href="http://uc.dtsagile.com/agenda">schedule</a> page, I built a nifty full screen multi-pane interface that works perfectly in all desktop browsers I tried and in the Android 3.1 browser on my Xoom. But yesterday <a href="http://blog.davebouwman.com/">Dave</a> pointed out that the bottom left and right panes wouldn't scroll on iPad.

After a little digging, I found that Mobile Safari doesn't support single finger scrolling on divs. You have to use two fingers. This is actually documented in the <a href="http://support.apple.com/manuals/ipad/">iPad User Guide</a>. Presumably this is because on touch based devices it's hard to know if you intend to scroll the whole page or just the div. But, like I said, it works perfectly on my Xoom. Now, if I didn't know this, I certainly can't expect my users to.

I found that <a href="http://www.flexmls.com/developers/2011/04/13/ipad-and-single-finger-scrolling-in-flexmls/">Nick Larson</a> had created a very simple fix to implement single finger scrolling. I took it and rolled it into a jQuery plugin (my first, actually) using <a href="http://www.malsup.com/jquery/">Mike Alsup's</a> <a href="http://www.learningjquery.com/2007/10/a-plugin-development-pattern">plugin pattern</a>. There are other solutions out there but mine has the virtue of being very easy to understand and very light (644 bytes minified).

<pre>
/*mjuniper 06/21/2011
    based on code from:
    http://www.flexmls.com/developers/2011/04/13/
ipad-and-single-finger-scrolling-in-flexmls/
    and http://www.learningjquery.com/2007/10/a-plugin-development-pattern
*/
//
// create closure
//
(function ($) {
    //
    // plugin definition
    //
    $.fn.mobileScroll = function (options) {
        //only do it for iOS devices
        var userAgent = window.navigator.userAgent.toLowerCase();
        if (userAgent.indexOf('ipad') > -1 || 
                userAgent.indexOf('iphone') > -1) {
            this.bind('touchstart', function (evt) {
                var e = evt.originalEvent;
                startY = e.touches[0].pageY;
                startX = e.touches[0].pageX;
            });

            this.bind('touchmove', function (evt) {
                //need the touches property of the native dom event
                // - jquery exposes originalEvent for that
                var e = evt.originalEvent;
                var touches = e.touches[0];
                //cache $(this)
                var $this = $(this);

                // override the touch event’s normal functionality
                e.preventDefault();

                // y-axis
                var touchMovedY = startY - touches.pageY;
                startY = touches.pageY; // reset startY for the next call
                $this.scrollTop($this.scrollTop() + touchMovedY);

                // x-axis
                var touchMovedX = startX - touches.pageX;
                startX = touches.pageX; // reset startX for the next call
                $this.scrollLeft($this.scrollLeft() + touchMovedX);
            });
        }

        return this;
    };
    //
    // end of closure
    //
})(jQuery);
</pre>