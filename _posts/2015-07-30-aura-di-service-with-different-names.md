---
published: true
title: Aura Di setting service with different names
layout: post
category: posts
comments: true
---

I have been working on a legacy application and wanted to remove the old process of how it lookup its services from file name and database entry to using a service locator container instead. I looked at a few DI containers and went along with Aura Di.

I was happy that it would allow me to work with it as a service locator as I wanted for backward compatibility in the application. In the legacy code, service names vary and look like `error.handler`, while we want to refer to them by using PHP 5.5 class name resolution `ErrorHandler::class`.

{% highlight php %}
<?php
use Aura\Di\ContainerBuilder;
$builder = new ContainerBuilder();
$di = $builder->newInstance();

$di->set(ErrorHandler::class, $di->newInstance('ErrorHandler'));
{% endhighlight %}

This works really well for the the new codebase, but as we are working on keeping things backward compatible. In order to have also keep the legacy codebase working we can reference the already set service.

{% highlight php %}
<?php
$di->set('error.handler', $di->lazyGet(ErrorHandler::class));
{% endhighlight %}

Now we can get retrieve a service using the legacy name for old code and also using class name resolution for new code.

{% highlight php %}
<?php
$di->get('error.handler');

// or

$di->get(ErrorHandler::class);
{% endhighlight %}

Thanks to [Paul Jones][pmjones] for taking the time to help solving this issue.

[pmjones]: https://twitter.com/pmjones
