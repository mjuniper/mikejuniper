---
layout: post
title: Esri UC BackChannel app - localStorage and jQuery deferred
date: '2011-06-10 15:12:15'
---

So <a href="http://blog.davebouwman.com/">Dave</a> and I recently put together a <a href="http://uc.dtsagile.com/">BackChannel</a> app for the Esri User Conference next month. For the agenda part of it, we wanted to use localStorage to store the data so that after the initial page load, it would be snappy (the session data is several hundred kilobytes). So I built a little repository class that wraps all that up using jQuery deferred. Below is the getUcSessions method. Basically, it checks to see if the sessions are in local storage and, if so, returns them, if not, it requests them with an XHR, returns a deferred, and resolves the deferred when it's got them. That way my calling code doesn't need to know anything about localstorage or the XHR or anything.

<pre>
getUcSessions: function (sessionsJsonPath) {
       //if we've got it in localStorage, return that
       var data = localStorage.getItem(_sessionKey);
       if (data) {
           console.debug('got sessions from localStorage.');
           _ucSessions =JSON.parse(data);
           return _ucSessions;
       }

       //otherwise, load the json and stuff it into localstorage
       //here we return a promise that will be resolved when we've gotten and processed all the data
       var dfd = new $.Deferred();
       $.getJson(sessionsJsonPath, function (sessions) {
            _ucSessions = sessions;
            console.debug('got sessions via XHR.');

            dfd.resolve(_ucSessions);
       });
       return dfd.promise();
}
</pre>

Then to call it, you just do:

<pre>
$.when(_repo.getUcSessions(args.sessionDataUrl)).then(function (ucSessions) {
    //Do stuff with the sessions...

});
</pre>