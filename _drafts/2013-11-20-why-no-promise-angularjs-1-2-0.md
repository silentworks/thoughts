---
published: false
---

In the latest version of AngularJS (1.2.0+) there seem to have been a change which will probably break most app which have in the past relied on the view unwrapping the promise object and showing the results directly in the view.

After searching online and not being able to find any fix for my issue, I resorted to Twitter as it seems like I normally get quicker results on Twitter than I do searching Google when it's AngularJS related.

After a few minutes of my tweet I got a reply from [@ithinkdancan](https://twitter.com/ithinkdancan) pointing me to a [Stack Overflow][stackoverflow] question he had asked and got the answer to.

This is not as prominent in the changelog as I would expect it to be, seeing that this is one of the features that was talked about a lot in previous versions of AngularJS.

There are two ways to fix this issue according to the [Stack Overflow][stackoverflow] link. You can either turn the feature back on by doing



[stackoverflow]: http://stackoverflow.com/questions/19472017/angularjs-promise-not-binding-to-template-in-1-2