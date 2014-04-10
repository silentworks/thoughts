---
published: true
layout: post
category: posts
comments: true
title: "Slim PHP 101 - How to use multiple view parsers"
---

In this post we will go through setting up [Slim][] to work with multiple view parsers, for this example we will look at using [Slim's][] PHP view library and the [Twig][] view library. In order to use Slim with Twig we need a view parser, luckily enough there is already one created in the [Slim Views][] repository.

> We will be using composer to install all our dependencies.

### Composer setup

Our composer.json file will look like below.

{% highlight json %}

{
    "require": {
        "slim/slim": "2.4.*",
        "slim/views": "0.1.*",
        "twig/twig": "1.15.*",
    }
}

{% endhighlight %}

Once you have installed all your dependencies we can start setting up our Slim project. We will do all of our setup in our `index.php` file for this project. Here is what my `index.php` look like.

{% highlight php %}

<?php
require 'vendor/autoload.php';

$app = new \Slim\Slim();

// GET routes
$app->get('/', function () use ($app) {
    $app->render('home.php');
})->name('home');

$app->get('/about', function () use ($app) {
    $app->render('about.php');
})->name('about');

$app->run();

{% endhighlight %}

Now we have a basic web project setup and rendering two templates with the [Slim's][] PHP view library.

> Note: your templates should be in your designated templates directory, in this case that would be the default `./templates`

Lets start adding in our extra code now to configure our twig renderer while keeping our current setup.

{% highlight php %}

<?php
require 'vendor/autoload.php';

$app = new \Slim\Slim();

$app->container->singleton('twig', function ($c) {
    $twig = new \Slim\Views\Twig();

    /* Option */
    $twig->parserOptions = array(
        'debug' => true,
        'cache' => dirname(__FILE__) . '/cache'
    );

    /* Extensions */
    $twig->parserExtensions = array(
        new \Slim\Views\TwigExtension(),
    );

    $templatesPath = $c['settings']['templates.path'];
    $twig->setTemplatesDirectory($templatesPath);

    return $twig;
});

{% endhighlight %}

### Step through `index.php` code above

Now we are injecting a dependency into the [Slim DI][] container, which we will use as a resource locator later on. If you have a look into the `Slim.php` source code you will see this being used for quite a few dependencies inside of Slim itself. We are setting this as a singleton because we want the resource definition to remain each time it is requested.

In the singleton method we define our resource as `twig` and pass it a closure with our settings and extensions we want to load. The first piece of code inside of our closure is just instantiating the Twig view.

{% highlight php %}

<?php
$twig = new \Slim\Views\Twig();

{% endhighlight %}

Now we have our [Twig][] view initialised, we might want to setup our options and extensions we want [Twig][] to use. This is what the lines below are doing:

{% highlight php %}
<?php
/* Option */
$twig->parserOptions = array(
    'debug' => true,
    'cache' => dirname(__FILE__) . '/cache'
);

/* Extensions */
$twig->parserExtensions = array(
    new \Slim\Views\TwigExtension(),
);
{% endhighlight %}

Once we have this done we need to tell out [Twig][] view parser where out template directory is located, we can get this through the resource locator from our
Slim settings.

{% highlight php %}
<?php
$templatesPath = $c['settings']['templates.path'];
$twig->setTemplatesDirectory($templatesPath);

{% endhighlight %}


Once we are done with our settings and setting the template path we return our instance.

{% highlight php %}
<?php
return $twig;

{% endhighlight %}

> Note: our instance will be lazy loaded, meaning the Twig view won't be instantiated unless we request the resource from the resource locator.

Now we can start making changes to our code to make use of our newly setup Twig view. If we wanted to change our `about` route we would rewrite as:

{% highlight php %}
<?php
$app->get('/about', function () use ($app) {
    $app->twig->display('about.php');
})->name('about');

{% endhighlight %}

We are now using our Twig view to render our template instead of the PHP view, while in our `home` route we are still using our PHP view. Here is the full code below:

{% highlight php %}

<?php
require 'vendor/autoload.php';

$app = new \Slim\Slim();

$app->container->singleton('twig', function ($c) {
    $twig = new \Slim\Views\Twig();

    /* Option */
    $twig->parserOptions = array(
        'debug' => true,
        'cache' => dirname(__FILE__) . '/cache'
    );

    /* Extensions */
    $twig->parserExtensions = array(
        new \Slim\Views\TwigExtension(),
    );

    $templatesPath = $c['settings']['templates.path'];
    $twig->setTemplatesDirectory($templatesPath);

    return $twig;
});

// GET routes
$app->get('/', function () use ($app) {
    $app->render('home.php');
})->name('home');

$app->get('/about', function () use ($app) {
    $app->twig->display('about.php');
})->name('about');

{% endhighlight %}

Here are some use cases for doing this.

- Migrating from one view parser to another
- Having a designer friendly template parser for HTML emails
- Creating a site builder system and want to make sure the end user won't be able to easily break your code

[Slim]: http://www.slimframework.com/
[Slim's]: http://www.slimframework.com/
[Twig]: http://twig.sensiolabs.org/
[Slim Views]: http://github.com/codeguy/Slim-Views
[Slim DI]: http://docs.slimframework.com/#DI-Overview
[Slim's DI]: http://docs.slimframework.com/#DI-Overview
