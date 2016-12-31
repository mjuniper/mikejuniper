---
layout: post
title: Asp.Net MVC + dojo dialog
date: '2009-02-05 09:15:51'
---

I've bee working a ton with <a href="http://www.asp.net/mvc" target="_blank">dojo</a> and we recently started doing several projects using <a href="http://www.asp.net/mvc" target="_blank">Asp.Net MVC</a>.

I've made fairly extensive use of dojo dialogs (dijit.Dialog) and thus far I've usually just declared the dialog in markup because it was easy and most of the samples do it that way. But I was never totally satisfied with that. But last week I realized something cool: you can create a dojo dialog in javascript and set its href property to an Asp.Net mvc controller method that returns a view user control! This is cool for lots of reasons: you can separate the dialog markup from the rest of it, you only create the dialog if you actually need it, and best of all, the view user control can be used to conditionally render the dialog (enable/ disable controls, show different controls, etc) based on what you've got in viewdata, your model, or session state.

So I realized this should work in theory but I expected to have to fiddle with it to get it to actually work. Nope, it just works.

Some sample code:

Controller method:
<pre>
public ActionResult GetDialog()
{
    var model = new RoleDialogModel();

    model.foo = this.foo;

    return View("RoleDialog", model);
}
</pre>
Pretty straightforward, just create the model, set some properties and return the view, passing it the model. I won't show you the view - just create a view user control and stick some markup in it.

Javascript:
<pre>
showDialog: function(title, action, evt)
{
    //first check if it's there so we don't create a duplicate
    var dialog = dijit.byId('fooDialog');
    if (dialog) { dialog.destroyRecursive(); }

    dialog = new dijit.Dialog({
        refreshOnShow: true,
        id: 'fooDialog',
        title: title,
        onDownloadEnd: dojo.hitch(this, this.initializeDialog, action, evt)
    });

    dialog.setHref(this.dialogUrl);

    dialog.show();
}
</pre>
There are a couple of things to notice here. First, I check if I've already created the dialog, dojo will puke if you try to create two objects with the same id. If it's there I destroy it. I was previously just resetting its properties but the title wouldn't change. I know I saw a post somewhere about how to do that but for now, I'm just destroying it and re-creating it. I set onDownloadEnd to an initialization function which hooks up ui events, etc. You should do any dijit widget stuff here, to be sure the widgets are there when you go looking for them. The setHref function sets the href of the dialog to the controller method above. I realized after I got this working that setHref is deprecated and you should use the href property instead but it was working so I didn't want to mess with it.

I think this is a great way to handle dojo dialogs in an Asp.Net MVC app. I plan to continue using this approach and refine it going forward.