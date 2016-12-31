---
layout: post
title: Fun with dojo.io.iframe.send
date: '2009-03-27 15:06:08'
---

One of my projects has had functionality to export a map image for some time. To get the map image, I just call (javascript) window.open and point the url to my http handler that will return the map image (with additional querystring parameters that specify the map parameters). This was working fine but now the customer wants to include lots of other markup on the map, including stuff they've drawn on the map with the nifty drawing tools I've provided them. This presented a bit of a problem because if I continued to just use window.open, the querystring was gonna get really long. So I realized I'd have to post the map parameters to my handler. But as far as I know, there's no way to post data when you call window.open. So what to do?

The first thing I thought of was that I could post the parameters using dojo.xhrPost and then prepare the image and return the url to it. Then I'd call window.open with the url returned from the first request. I didn't really like that. For one thing, it would be two requests, but for another, I'd have to keep that map image around in some manner.

So I decided to use dojo.io.iframe.send. This ended up working very well but I did run into a few issues that I thought I'd share.

One thing that I didn't realize at first is that dojo expects the response to be an html document. And if you want to return anything other than an html document (json or text for example), you <em>must</em> wrap it in an html textarea. Dojo will look inside the first textarea in the response document and return to your handle, load, or error functions whatever it finds there in the format you specified in handleAs (defaulting to text if you did not specify one).

Another thing I ran into was that my dojo.io.iframe.send call would only work once per page load. I'm not exactly sure why that is. I found a suggestion that you set a timeout on the __IoArgs object passed to iframe.send. That worked in the sense that I could call it again after the timeout expired but I have no idea how long this operation is going to take so it's hard to know what to set the timeout to. My solution was to keep a reference to the dojo.Deferred object returned by iframe.send and call cancel on it before trying to send another request like so:

<pre>
if (this.getMapImageDeferred)
{
  this.getMapImageDeferred.cancel();
}
</pre><br/>

This seems to work fine but I do have to check in my errBack function whether the request was canceled:

<pre>
_mapImageCallbackError: function(response, ioArgs)
{
  if (response.message === 'Deferred Cancelled')
  {
    return response; 
  }
    // handle the error...
},
</pre><br/>

The last thing I ran into really had me stumped. I was setting method = "post" in the __IoArgs object passed to iframe.send but it was always using get. I read a few things here and there that kind of seemed to suggest that if I passed a form with its method set to 'post' into __IoArgs that might do the trick. And it did! So here's what I'm doing now:

<pre>
//create a form with method=post
var form = document.createElement('form');
dojo.attr(form, 'method', 'post');
document.body.appendChild(form);

//create my postdata
var content = {
  /*set a bunch of parameters here that 
  will be posted to the http handler*/
};

//make the request
this.getMapImageDeferred = dojo.io.iframe.send({
  url: path + 'map.mapimage.aspx',
  form: form,
  content: content,
  error: dojo.hitch(this, this._mapImageCallbackError)
});

//get rid of the form created above
document.body.removeChild(form);
</pre><br/>

I'd be interested to know if anyone else has run into these issues and how you resolved them.