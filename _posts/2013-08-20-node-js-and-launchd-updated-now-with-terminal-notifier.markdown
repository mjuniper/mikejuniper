---
layout: post
title: Node.js and launchd - updated! Now with terminal-notifier
date: '2013-08-20 16:52:06'
---

I thought it would be nice to pump the backup notifications I talked about in this post to the OSX notification system. It turns out that this isn't quite as simple as you'd hope but it's not too bad.

You need 2 things the <a href="https://github.com/alloy/terminal-notifier">terminal-notifier</a> command line tool and <a href="https://github.com/evanw/node-terminal-notifier">node-terminal-notifier</a>.

First you install the terminal-notifier tool by typing this command in the terminal:
<code>$ [sudo] gem install terminal-notifier</code>

On my Mac, Ruby was already installed which was handy.

Then you:
<code>npm install terminal-notifier</code>

Then in your node script:
<code>var notifier = require('terminal-notifier');</code>

and:
<code>notifier('This is the message.', { title: 'This is the title' });</code>

Sweet!