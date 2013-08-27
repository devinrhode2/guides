# Everything you ever wanted to know about iframes.

I figured it would be worthwhile to write out everything I know about iframes, as I've learned an awful lot.

Let's start by sharing how I frame any site in Scout. There are 2 key issues with framing any site on the web.

1. The X-Frame-Options header. If sites issue this http header, browsers will not load the requested page, simple as that. IE introduced it and other browsers quickly adopted it. However, after working for a long time on a hybrid hack around this, Chrome introduced a new API that allows me to strip this header. This was the magic I needed.

2. In page javascript breaking out. Many major sites include javascript that either skews the page when loaded in an iframe, fails to load, or attempts to break out of the frame. Being an extension, I simply reverse engineer this javascript. For Facebook, since they use document.write to say "Please go to Facebook.com to view this page" or something like that. I overwrite the document.write method with some bs, and facebook's done. Github uses some jQuery method I think, which I also overwrite, in theory this stuff can break the sites but I do it the safest way possible and haven't had issues for months as I and other have been using Scout.

Random fine details about iframes:

1. The onload event is useful. It fires for every subsequent page load of the iframe. BUT the src attribute of the iframe does not update. Currently, there is no way to read an updated url of an iframe.

2. For all iframes you do, you probably want to use some of the html5 permissions. Add the attribute `sandbox=""`

2. No matter what, always use html5 sandbox="", I use allow-scripts allow-forms allow-same-origin, but this is up to you. What you probably really don't want, is allow-top-navigation. This allows a framed page to break out of the frame. Worst case scenario, it tries to break out and the page goes white. You may be able to detect this with iframeElement.contentWindow.location.href != iframeElement.getAttribute('src'). The location.href would likely be 

Future of iframes:
There are a few key things iframes do. They separate styles and javascript. By default they also separate domain security, but ideally this should be optional. Anyone scrape a page and load an iframe pointing to their domain serving the scraped page. But you can't really do this AND have the page authenticated. When I say authenticated, I mean if I load twitter.com/devinrhode2, you should be able to click "Follow" and have it work. Right now, there's no way for a site to do this, perhaps for good reasons. However, they can load a public page, where you aren't authenticated on the page.


Follow the author: <a href="https://twitter.com/devinrhode2">@DevinRhode2</a>
And editors: <a href="https://twitter.com/">@YourUserNameHere</a>
