---
layout: post
title: dojo.query - putting it all together
date: '2009-08-05 10:24:40'
---

So I guess this will be the finale to my <a href="http://www.mikejuniper.com/2009/02/extending-dojoquery/" target="_blank">series</a> of <a href="http://www.mikejuniper.com/2009/02/extending-dojo-query-part-2/" target="_blank">posts</a> on dojo.query. I'm using dojo.query to validate data entered into dojo dialogs. In <a href="http://www.mikejuniper.com/2009/02/aspnet-mvc-dojo-dialog/" target="_blank">this post</a>, I discussed getting dialog content from an Asp.Net MVC controller method. In the onDownloadEnd function, I do any setup of the dialog that is necessary including something like this:
<pre>var onValueChanged =
        dojo.hitch(this, this.dialogValueChanged, dialogId);
//gotta be keyup -
        //validation is out of sync if you use keydown or keypress
dojo.query('input[type="text"], textarea', dialogId)
        .connect('onkeyup', onValueChanged);
dojo.query('select', dialogId)
        .connect('onchange', onValueChanged);
dojo.query('.dijit', dialogId).widgets()
        .connect('onChange', onValueChanged);</pre>
This query's for input elements in the dialog and connects each element's appropriate method to the function where I do the validation (dialogValueChanged). Note the use of the widgets method discussed in <a href="http://www.mikejuniper.com/2009/02/extending-dojo-query-part-2/" target="_blank">this post</a>.

Here's the validation function:
<pre>dialogValueChanged: function(dialogId)
{
  //check if everything is valid
  var valid = dojo.query('[widgetid], [widgetId]', dialogId)
    .widgets().every(function(widget)
    {
      if (widget.isValid)
      {
        return widget.isValid();
      }
      //if it is not a widget
      //or does not have an isValid method, return true
      return true;
    });                                

    //enable/ disable the save button based on whether it's valid
    var action = (valid) ? 'removeAttr' : 'attr';
    dojo.query('.dialogSaveBtn', dialogId)
	  [action]('disabled', 'disabled');
}</pre>
Here we're querying for widgets and using our previously discussed widgets method to get the actual widgets (not just the dom nodes). Then we use the every method of NodeList to call the isValid method on each widget. The every method will return false if any of the widgets' isValid methods returned false. Then we use the value returned from every to set our 'action' variable and use query again to enable or disable our save button(s) by adding or removing the disabled attribute. Note the use of the removeAttr method we added to NodeList in <a href="http://www.mikejuniper.com/2009/02/extending-dojo-query-part-2/" target="_blank">this post</a>. Note also that this all depends on all the input elements on the dialog being dijit widgets. This mechanism can be easily extended to handle non-dijit elements too.