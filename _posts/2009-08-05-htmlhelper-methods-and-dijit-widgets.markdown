---
layout: post
title: HtmlHelper methods and dijit widgets
date: '2009-08-05 14:46:03'
---

This is certainly not earth-shattering but it's pretty cool. Asp.Net MVC HtmlHelper methods return a string of html and are used to render html elements. They are cool because you can use them to change the way the control is rendered at runtime based on what you've got in ViewData or your model. What's even cooler is that you can pass html attributes (including custom ones like dojoType) to the HtmlAttributes parameter. This allows you to use HtmlHelper methods to render dijit widgets like so:
<pre>
&lt;%=Html.TextBox("search_StreetAddress",
  Model.Criteria.StreetAddress,
  new { dojoType = "dijit.form.TextBox", trim = "true" })%&gt;</code>

&lt;%= Html.DropDownList("CityId", Model.CitySelectList,
  new { dojoType = "dijit.form.FilteringSelect" })%&gt;

&lt;%= Html.TextBox("Year", Model.Year,
  new { dojoType = "dijit.form.NumberTextBox",
  constraints = "{min:1900, max:2500, pattern:'0000'}",
  required = "true",
  rangeMessage = "A 4 digit year is required",
  invalidMessage = "A 4 digit year is required" })%&gt;
</pre>