---
layout: post
title: Loading a blog feed on your website
date: '2011-07-29 12:14:46'
---

Recently I needed to load recent blog posts onto a website. I discovered the <a href="http://code.google.com/apis/feed/v1/">Google Feed API</a> which, as most things Google, is awesome. I also discovered <a href="http://jquery.malsup.com/gfeed/">this</a> jQuery plugin which uses it along with what appears to be an old version of their Dynamic Feed Control. This plugin, like all of Mike Alsup's other plugins, is great. However, I couldn't get it to work exactly the way I wanted. So I built my own.

I also borrowed and modified some <a href="http://ejohn.org/blog/javascript-pretty-date/">code</a> from John Resig to format the dates. Oh yeah, it also depends on the <a href="http://api.jquery.com/category/plugins/templates/">jQuery Templating Plugin</a>.

It takes an options argument with a required url property and a couple of other optional properties, gets the feed, and loads the items into semantic markup inside the selected element. You can then style it any way you want.

<pre>
/**
*  Plugin which uses the Google AJAX Feed API for creating feed content
*  depends on jquery template plugin
*  very loosely based on http://jquery.malsup.com/gfeed/
*/

(function ($) {
    if (!$.template) {
        alert('You must include the jQuery Templating Plugin script');
        return;
    }

    //feed item template
    var templateHtml = '&lt;div&gt;' +
                            '&lt;div class="feed-item"&gt;' +
                                '&lt;a href="${link}" target="_blank"&gt;' +
                                    '&lt;h2 class="feed-title"&gt;${title}&lt;/h2&gt;' +
                                '&lt;/a&gt;' +
                                '&lt;div class="feed-snippet"&gt;${contentSnippet}&lt;/div&gt;' +
                                '&lt;div class="feed-footer"&gt;Posted ${publishedDate} by ${author}&lt;/div&gt;' +
                            '&lt;/div&gt;' +
                        '&lt;/div&gt;';

    //from based on code from John Resig 
    // - http://ejohn.org/blog/javascript-pretty-date/
    function prettyDate(dateString) {
        var date = new Date(dateString);
        diff = (((new Date()).getTime() - date.getTime()) / 1000),
		            day_diff = Math.floor(diff / 86400);

        if (isNaN(day_diff) || day_diff < 0) {
            return;
        }

        return day_diff == 0 && (
			        diff < 60 && "just now" ||
			        diff < 120 && "1 minute ago" ||
			        diff < 3600 && Math.floor(diff / 60) + " minutes ago" ||
			        diff < 7200 && "1 hour ago" ||
			        diff < 86400 && Math.floor(diff / 3600) + " hours ago") ||
		            day_diff == 1 && "Yesterday" ||
		            day_diff < 7 && day_diff + " days ago" ||
		            day_diff < 42 && Math.ceil(day_diff / 7) + " weeks ago" ||
                    day_diff >= 42 && Math.floor(day_diff / 31) + " months ago";
    }
    new Date().tou
    $.fn.feed = function (options) {
        var opts = $.extend({
            url: '',
            target: this,
            title: 'The latest from our blog',
            max: 5   // max number of items
        }, options || {});

        //show a loading indicator
        opts.target.append('&lt;div id="feed-loading"&gt;loading blog feed...&lt;/div&gt;');

        //get the feed items
        var feedItemTemplate = $(templateHtml).template();
        var gFeedsUrl = 'http://ajax.googleapis.com/ajax/services/feed/load?v=1.0&callback=?&num=' + opts.max + '&q='
        $.ajax({
            url: gFeedsUrl + encodeURIComponent(opts.url),
            dataType: 'json',
            success: function (response) {
                //remove the loading indicator
                $('#feed-loading').remove();

                if (response && response.responseData 
                                  && response.responseData.feed 
                                  && response.responseData.feed.entries) {
                    var feedItems = response.responseData.feed.entries;
                    //map them to format the date
                    $.map(feedItems, function (item, idx) { 
                        item.publishedDate = prettyDate(item.publishedDate); 
                        return item; 
                    });
                    //load the feeds into the dom
                    opts.target.append('&lt;h1&gt;' + opts.title + '&lt;/h1&gt;&lt;div id="rss-feed"&gt;&lt;/div&gt;')
                        .find('#rss-feed')
                        .append($.tmpl(feedItemTemplate, feedItems));
                } else {
                    opts.target.append('&lt;div id="feed-error"&gt;Unable to retrieve blog feed.&lt;/div&gt;');
                }
            }
        });

        return this;
    };
})(jQuery);

</pre>