---
published: false
layout: post
category: posts
comments: true
---
I really like RactiveJS and how the inner workings work most of the time. I recently ran into an situation where I needed a component to behave a particular way and this was a challenge at first but with the help of Rich Harris (creator of RactiveJS) I was able to find a solution.

Before jumping to my current solution I will evaluate another potential solution and explain its shortcomings.

In my scenario I want to create a Ractive component and pass attributes (props or whatever you want to call them) into the actual template of the component. In this example I will do it all by manually stating the attribues I am expecting on the component template itself.

<a class="jsbin-embed" href="http://jsbin.com/cehofa/1/embed?js,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.35.13"></script>

You will notice I have created a `class="{{ class }}"` on the button element in the template for the PrettyButton component, along with this I have also provided a `title="{{ title }}"`. Both of these still had to be manually typed in the template, the problem that this leaves is that if there was no `class="pretty-button"` provided on the `<PrettyButton>` component, it would still show on our DOM node.
