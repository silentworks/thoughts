---
published: true
layout: post
title: "Why no promise AngularJS 1.2.0+"
category: posts
comments: true
---

In the latest version of [AngularJS][] (1.2.0+) there seem to have been a change which will probably break most app which have in the past relied on the view unwrapping the promise object and showing the results directly in the view.

After searching online and not being able to find any fix for my issue, I resorted to [Twitter][] as it seems like I normally get quicker results on Twitter than I do searching [Google][] when it's [AngularJS][] related.

After a few minutes of my tweet I got a reply from [@ithinkdancan](https://twitter.com/ithinkdancan) pointing me to a [Stack Overflow][stackoverflow] question he had asked and got the answer to.

This is not as prominent in the [changelog][] as I would expect it to be, seeing that this is one of the features that was talked about a lot in previous versions of [AngularJS][].

There are two ways to fix this issue according to the [Stack Overflow][stackoverflow] link. You can either turn the feature back on by using

{% highlight js %}
$parseProvider.unwrapPromises(true);
{% endhighlight %}

This is only recommended for short term fix as this feature is actually deprecated. If it is not clear where you would set this, I have written in example 1 below showing how this would be called.

### Example 1

	var app = angular.module('Application', []);
	app.config(function ($parseProvider) {
		$parseProvider.unwrapPromises(true);
	});
    
You can see the full code below.

<a class="jsbin-embed" href="http://jsbin.com/asOQeHU/2/embed?js">Angular 1.2.1 Test</a><script src="http://static.jsbin.com/js/embed.js"></script>
    
The recommended way of handling this is to unwrap your promise inside of your controller and pass the unwrapped data to your view, this can be done using the .then() method of the promise object.

### Example 2

	app.controller('RepeatController', function ($scope, DataFactory) {
  		DataFactory.getData().then(function (data) {
    			$scope.weeks = data;
  		});
	});
    
You can see full code below.

<a class="jsbin-embed" href="http://jsbin.com/UwuJacI/1/embed?js">Angular 1.2.1 Test</a><script src="http://static.jsbin.com/js/embed.js"></script>

Hope this will be of help to others and also serves as a reminder for myself.

[Twitter]: http://twitter.com
[Google]: http://www.google.com
[changelog]: https://github.com/angular/angular.js/blob/master/CHANGELOG.md#bug-fixes-2
[AngularJS]: http://angularjs.org
[stackoverflow]: http://stackoverflow.com/questions/19472017/angularjs-promise-not-binding-to-template-in-1-2
