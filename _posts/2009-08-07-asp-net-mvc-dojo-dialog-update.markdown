---
layout: post
title: Asp.Net MVC + dojo dialog update
date: '2009-08-07 08:09:41'
---

I've made some changes to the showDialog function I discussed in <a href="http://www.mikejuniper.com/2009/02/aspnet-mvc-dojo-dialog/">this</a> post. I've made it more generic - it now takes dialogId, href, title, queryString, and onDownloadend (a function). That way I can use it to open any dialog in my application. I'm also now not destroying the dialog if it already exists, I'm just resetting the onDownloadEnd and title properties and using my custom setTitle method:

<pre>
//extend dojo dialog to have a setTitle method
dojo.extend(dijit.Dialog, {
  setTitle: function(/*string*/title)
  {
     this.titleNode.innerHTML = title || "";
  }
});
</pre>

 Finally, I'm using the href property instead of setHref which is deprecated. Here's the new showDialog function:

<pre>
showDialog: function(dialogId, href, title, onDownloadEnd)
{
  //first check if it's there so we don't create a duplicate
  var dialog = dijit.byId(dialogId);
  if (dialog)
  { //it already exists, reset a few properties
    dialog.setTitle(title);
    dialog.onDownloadEnd = onDownloadEnd;
    dialog.href = href;
  }
  else
  { //it doesn't exist yet, so create it
    dialog = new dijit.Dialog({
      refreshOnShow: true,
      id: dialogId,
      href: href,
      title: title,
      onDownloadEnd: onDownloadEnd,
      onDownloadError: dojo.hitch(this, '_dialogDownloadError', dialogId)
    });
  }

  //show it    
  dialog.show();
}
</pre>

