Thanks to a talk from <a href="https://twitter.com/tomdale">Tom Dale</a>, a core author of <a href="http://emberjs.com">Ember.js</a>, my mind has been spinning around update models, from native apps, to my own live extension update system (not finished), html5 offline, and normal web caching. <a href="http://www.ustream.tv/recorded/26977615">Watch the talk here, I highly recommend it.</a>

tl;dr HTML5 offline has a fantastic update model that hits the perfect balance between performance considerations and user experience. You should use it <a href="https://twitter.com">TODAY</a>.

On iOS, app updates are shepherded through Apple. The user has to go and click update to update the app. It requires user intervention, and is not the best developer or user experience in my mind. One positive could be that the user always knows when the app is going to change. A huge positive is app performance. Apps launch instantly, they are local for pete's sake. Can't beat that!

On the (current) web, everything is always the latest copy. This is great for developers, and we love it. However, anyone truly keen on performance knows every second added to a page load time can drastically affect your business. This is best proved in a talk from Torbit, you should <a href="http://www.ustream.tv/recorded/25213585">watch it on ustream here.</a> tl;dw: Torbit has collected performance data at a massive scale, with a relation to revenue. This talk is the first time the numbers were released.

Now, I'm going to talk about the design decisions I've made in my own update system I'm working on for chrome extensions. Yes chrome extensions auto-update with chrome, but sometimes chrome updates break which means your extension won't update either. If you want to update the day after a chrome update was pushed out, you're waiting 6 weeks for the next chrome update. But that's not the focus here.

<h2>My design decisions</h2>

My first design was to hook up a websocket that will load code in localStorage, and then eval it. The 'extension' crx is a shell to do this, nothing more. You download the extension, the websocket connects, loads code in localStorage, and gracefully executes's it. I have my build process concatenate files into one single file, which I upload to my server, and have an update even sent out. The websocket receiver updates localStorage and everything's up to date for the next time a user loads my extension.

Then I learned websockets are actually <a href="https://speakerdeck.com/3rdeden/realtimeconf-dot-oct-dot-2012">a really terrible technology</a>. Even the hallmark node + websocket framework <a href="http://meteor.com/">Meteor</a> <a href="https://github.com/meteor/meteor/issues/332">doesn't actually use window.WebSocket</a>, it uses xhr-long polling.

So, I faced a decision. Do I want to bog down a users browser with a long polling connection, or have less than live updates? I thought about it for a long time. Now imagine if many many extensions held some websocket or long polling connection in the background? That would suck! This is why Apple has one listener for updates through it's AppStore. It would be crazy to give each app it's own listener for updates. Talk about a resource hog.

So listeners are out the door. We can't have quite live updates. That's just being mean to a person's computer/device.

But we do want Native performance. I'm not going to make  is great because there's no page load. And if you don't believe me go watch the talk from Torbit and Tom Dale. Performance matters. Native is winning because networks are slow and can be flaky. Like Tom, I want the web to win, and we can start by utilizing HTML5 Offline.

I thought really hard on the most efficient but also very prompt update system.

You download the extension, I see there's nothing loaded in localStorage so I fetch the first update. eval and the extension is running. Now, <em>do I sit and have a connection wait for an update?</em> No. When do I check for updates? Well, once my extension runs and I'm done, I should check for an update then. As someone is using it, I could also periodically ping the server to check for updates. If there's an update, I'll download it an it'll be ready next time the user uses the extension. I could display a little notification that an update will take effect next time the user reloads the page, so things don't expectedly change and piss users off, like facebook.

An important note: to get native level quality, I need to have my app ready the second someone wants to interact with it, the only network requests should be getting data, like for a news feed. Page loads suck.

Then I realized, this update model IS IDENTICAL TO HTML5 OFFLINE.

Admittedly, there are a few douchbag parts of the appcache.

At this point, I'm going to assume you know how html5 offline works. You can read up on it from Gmail mobile's blog posts here: or from html5rocks.com here: http://www.html5rocks.com/en/tutorials/appcache/beginner/

The appcache updates everything when any byte in the appcache changes. I think this should and will change to when any resource name changes. Modern web apps give their assets a hash in the filename, like main.s683jksd78ds.css. This is important for IE cache busting, but also serves well for the appcache. When this file hash changes in the appcache, the browser should just update that file, and no others.

Guess what? Everything else about html5 offline is much better than you think, it's just different.