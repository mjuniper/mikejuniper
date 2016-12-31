---
layout: post
title: Using dijit.form.DateTextBox with Asp.Net
date: '2009-08-05 13:42:13'
---

Since there's no date literal in javascript, there's no standard way of serializing/ deserializing dates. MS Ajax uses \/Date(&lt;ticks&gt;)\/ where ticks is the number of milliseconds since midnight 01/01/1970. It's a reasonable approach - unambiguous and easy to transform in both directions. For a while I was transforming this date object into a date before sticking it into a dojo DateTextBox, and transforming it back before sending it up to the server. But then I got smart and extended dijit.form.DateTextBox to handle this itself. Here's the code:
<pre>
dojo.provide('mjuniper.widgets.DateTextBox');
dojo.require('dijit.form.DateTextBox');</code>

/*A dojo datetextbox that handles sending dates
to and recieving dates from asp.net*/
dojo.declare('mjuniper.widgets.DateTextBox',
  dijit.form.DateTextBox,
  {
      // prevent parser from trying to convert to Date object
      value: "",

      postMixInProperties: function()
      {
        this.inherited(arguments);

      //if it's null, return
      if (!this.value)
      {
        return;
      }

      if (this.value.indexOf('/Date(') &gt; -1)
      {
        //extract the ticks from the json object
        var ticks =
          this.value.substring(this.value.indexOf('(') + 1);
        var endChar = (ticks.indexOf('-') === -1) ? ')' : '-';
        ticks = ticks.substring(0, ticks.indexOf(endChar));
        //instantiate a date
        this.value = new Date(parseInt(ticks));
        //if it's invalid, set value to null
        if (this.value == 'Invalid Date') { this.value = null; }
      }
    },

    /*override the serialize method to write
    back to the server in proper format*/
    serialize: function(dateObject, options)
   {
      return '\/Date(' + dateObject.getTime() + ')\/';
    },

    /*override setValue so we can set it with the
    json date object we get from .net*/
    setValue: function(dateString)
    {
      if (dateString.indexOf('/Date(') &gt; -1)
    {
    //extract the ticks from the json object
    var ticks =
      dateString.substring(dateString.indexOf('(') + 1);
    var endChar = (ticks.indexOf('-') === -1) ? ')' : '-';
    ticks = ticks.substring(0, ticks.indexOf(endChar));

    arguments[0] = new Date(parseInt(ticks))
    this.inherited(arguments);
    }
  }
});
</pre>

There are obviously a number of ways I could have handled extracting the ticks from the json string (including a regex, but then I would have had two problems). This one is not very elegant but it's relatively easy to understand and I was able to get it working quickly.