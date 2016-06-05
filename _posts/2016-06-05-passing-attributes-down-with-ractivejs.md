---
published: true
layout: post
category: posts
comments: true
title: Passing Attributes Down With RactiveJS
---
I really like [RactiveJS][] and how the inner workings work most of the time. I recently ran into an situation where I needed a component to behave a particular way and this was a challenge at first but with the help of [Rich Harris][] (creator of [RactiveJS][]) I was able to find a solution.

Before jumping to my current solution I will evaluate another potential solution and explain its shortcomings.

In my scenario I want to create a Ractive component and pass attributes (props or whatever you want to call them) into the actual template of the component. In this example I will do it all by manually stating the attribues I am expecting on the component template itself.

<a class="jsbin-embed" href="http://jsbin.com/cehofa/1/embed?js,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.35.13"></script>

You will notice I have created a `class="{\{ class \}\}"` on the button element in the template for the PrettyButton component, along with this I have also provided a `title="\{\{ title \}\}"`. In Both of these still had to be manually typed in the template, the problem that this leaves is that if there was no `class="pretty-button"` provided on the `<PrettyButton>` component, it would still show on our DOM node. There is also the issue where if you need to add a new attribute on `<PrettyButton>` you would also need access to the template of that component.

It so happens that the component I am working on is something I plan to open source and it would be very hard to modify the template as it wouldn't be a part of user land code. The solution to this problem is to dynamically add the attributes passed dowm.

Here is the solution I went with after getting [help from Rich Harris][solution].

<a class="jsbin-embed" href="http://jsbin.com/lakawiv/3/embed?js,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.35.13"></script>

The difference between this code and previous code is that I am now adding attributes dynamically. These attributes are being added from a _safe list_ object with keys and values mapped. If we needed to make this work in user land code, all we would have to do is expose the _safe list_ to user land and we could add new attributes easily.

[RactiveJS]: http://www.ractivejs.org/
[Rich Harris]: https://twitter.com/Rich_Harris
[solution]: https://twitter.com/Rich_Harris/status/739123210596298753