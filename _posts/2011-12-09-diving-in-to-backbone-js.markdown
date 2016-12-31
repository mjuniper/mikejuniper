---
layout: post
title: Diving in to Backbone.js
date: '2011-12-09 13:12:27'
---

We've known for a while that there are a number of javascript mvc frameworks out there and that many of the apps we build would likely benefit greatly from using one. Bet we've held back mostly because of the learning curve/ time constraints but also partly because it wasn't clear which one was gonna be 'best' for our needs. Well, we've got a particular project on our plate right now that just begs for a framework like this so we decided to take the plunge. We settled on <a href="http://backbonejs.org/">Backbone.js</a> mostly because it seems like there are lots of super smart people who are using it (and they still like it!) and also because there seem to be lots of resources available for it (not least of which is the very good documentation). 

I don't really have the time to do a proper Intro to Backbone post but fortunately many others have done so. Some are better than others though, so what I thought I'd do is share some of the resources I found most helpful and mention a few of the additional libraries we're using.

It's true that the learning curve is somewhat steep but once you dive in, it's not really that bad. A good place to start is <a href="http://backbonetutorials.com/">http://backbonetutorials.com/</a> which provides a brief introduction to the various backbone components. A very easy to understand and quite good hello world example is at <a href="http://arturadib.com/hello-backbonejs/docs/1.html">http://arturadib.com/hello-backbonejs/docs/1.html</a>. This one is cool because it starts very simply and progressively adds complexity.

Rob Conery wrote a <a href="http://wekeroad.com/2011/08/11/the-backbonejs-todo-list-sample-refactored-part-1/">two</a> <a href="http://wekeroad.com/2011/08/12/the-backbone-js-todo-list-refactored-part-2-being/">part</a> post discussing some of the problems he saw with many Backbone tutorials. I was really happy to see this because I shared some of his objections.

One of the things I missed when I first started this project was model binding. Then I found <a href="https://github.com/derickbailey/backbone.modelbinding">this</a> awesome plugin for Backbone which provides two-way, convention-based model binding. This was ridiculously easy to use and it allowed me to slim down my views considerably.

One issue I had was that the particular needs of our app called for messaging between various components other than just a model and its view. For instance, I needed my Results view to know when my Criteria model changed so it could update itself with new data based on the Criteria model that changed; but other than that, the Results view needed no knowledge of the Criteria model. I initially used a shared reference to the Criteria model but I didn't love it because I wanted my components to be more loosely coupled. I now think that would have been fine but my dissatisfaction lead me to <a href="http://lostechies.com/derickbailey/2011/07/19/references-routing-and-the-event-aggregator-coordinating-views-in-backbone-js/">this</a> post that suggests using the Event Aggregator pattern. It's really very simple. Since I already had all my modules namespaced, I just put my event aggregator as a property on my namespace object and all my other modules have access to it:
<pre>
dts.eventAggregator = _.extend({}, Backbone.Events);
</pre>

Then you can just do <pre>dts.eventAggregator.bind('fooEvent', fooEventHandler, this);</pre> and <pre>dts.eventAggregator.trigger('fooEvent', {/*event arguments*/});</pre>

I found <a href="http://lostechies.com/derickbailey/2011/09/15/zombies-run-managing-page-transitions-in-backbone-apps/">this</a> very good post discussing a good approach to avoiding memory leaks. One of the comments lead me to <a href="http://stackoverflow.com/questions/7567404/backbone-js-repopulate-or-recreate-the-view/7607853#7607853">this</a> StackOverflow post discussing the same issue. I think a combination of these two approaches would work well.

Though I'm new to javascript mvc frameworks, I've been using javascript templating for a while now. I've mostly been using jQuery Templates which I like just fine, but I took this opportunity to try some other templating engines. I ended up settling on <a href="http://icanhazjs.com/">icanhaz.js</a> because it uses the <a href="http://mustache.github.com/">mustache.js</a> template syntax (it actually uses mustache under the covers, I think) which I like very much and because it precompiles the templates into functions (thus improving performance, I assume) and hangs them right on the ich object with the function name corresponding to the id of the template (thus making it very easy to use). I'll definitely be sticking with icanhaz going forward.

Of course, one of the best things about the MVC pattern is that it lends itself quite well to testing. I found a very nice multi-part post on testing backbone applications - the first part is <a href="http://tinnedfruit.com/2011/03/03/testing-backbone-apps-with-jasmine-sinon.html">here</a>. I had used <a href="http://docs.jquery.com/QUnit">qUnit</a> before so I decided to stay with it to minimize the risk of my head exploding from all the new stuff (though I was tempted to give <a href="https://github.com/pivotal/jasmine/">Jasmine</a> a whirl). The post above lead me to <a href="http://sinonjs.org/">sinon.js</a> (and its qUnit adapter, <a href="http://sinonjs.org/qunit/">sinon-qunit</a>) which is a really great little library that provides test spies, stubs, and mocks. Unit testing this stuff was fun and it actually helped me uncover some bugs that I didn't notice in my ui testing.

I've learned a ton this week, which is always fun, and I'm looking forward to building more applications with Backbone.js.
