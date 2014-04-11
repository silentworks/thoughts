---
published: true
layout: post
category: posts
comments: true
title: "Slim PHP 101 - Using Laravel config package"
---

Over the past few years Laravel have become a popular full stack PHP framework and with its latest version (L4) the framework have been modularize. Now we can make use of most of Laravel components outside of the framework, the author of the framework have created packages under the name [illuminate][].

Today I will show you how to integrate the [Config][] package into Slim and make use of it. Before we move on to that, some may ask what value will this add to [Slim][] since we can already store settings inside of [Slim][]. With the [Config][] package
we can store settings into separate files and we can also make use of the cascading folder structure for different environments.

Here is an example of the structure we can have below.

{% highlight bash %}
  - app
  - config
    - testing
      - database.php
    - production
      - database.php
      - urbanairship.php
    - database.php
    - urbanairship.php
    - config.php
  - templates
  - vendor
  - index.php
{% endhighlight %}

This means that when we request for a config, the [Config][] package will check our environment and load that config in the correct environment directory, if the config is not available in that environment directory, the [Config][] package will go to the root level of the config directory to check.

> Note: I will go through setting up an environment variable further on in this article.

Now that we can see one of the benefits (will cover more as the article goes on), we can now move on to the actual code.

Here is a example of what our `config.php` file would look like.

{% highlight php %}
<?php
return array(
  'site_name' => 'My Website',
  'site_strapline' => 'This is my first website'
);
{% endhighlight %}

> We will be using composer to install all our dependencies.

### Composer setup

Our composer.json file will look like below.

{% highlight json %}
{
    "require": {
        "slim/slim": "2.4.*",
        "illuminate/config": "4.1.*"
    }
}
{% endhighlight %}

> [Config][] package has its own dependencies which will get installed

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

Lets start adding in the [Config][] package into our current code.

{% highlight php %}
<?php
require 'vendor/autoload.php';

// Setup our environment variable base on SLIM_ENV or default to production
define('ENVIRONMENT', isset($_SERVER['SLIM_ENV']) ? $_SERVER['SLIM_ENV'] : 'production');

$app = new \Slim\Slim();

// Config code
$app->container->singleton('files', function () {
  return new Illuminate\Filesystem\Filesystem;
});

$app->container->singleton('loader', function ($c) {
  $configPath = __DIR__ . '/config';
  return new Illuminate\Config\FileLoader($c['files'], $configPath);
});

$app->container->singleton('config', function ($c) {
  return new Illuminate\Config\Repository($c['loader'], ENVIRONMENT);
});
{% endhighlight %}

Lets walk through the added code above and explain what each bit is doing (I will intentionally skip the `ENVIRONMENT` variable bit as the comment states what it's doing).

{% highlight php %}
<?php
$app->container->singleton('files', function () {
  return new Illuminate\Filesystem\Filesystem;
});
{% endhighlight %}

In order to make use of the [Config][] package we need the [Filesystem][] package, we are passing this into the [DI][] container of [Slim][].

{% highlight php %}
<?php
$app->container->singleton('loader', function ($c) {
  $configPath = __DIR__ . '/config';
  return new Illuminate\Config\FileLoader($c['files'], $configPath);
});
{% endhighlight %}

In the code above we now setup the config loader which we pass the [Filesystem][] package into along with our config root path.

{% highlight php %}
<?php
$app->container->singleton('config', function ($c) {
  return new Illuminate\Config\Repository($c['loader'], ENVIRONMENT);
});
{% endhighlight %}

Now we are creating our actual `config` resource locator and passing in our previously defined `loader` resource along with our `ENVIRONMENT` constant we defined at the top of our code.

We can now make use of this by calling `$app->config`, you can find the [API documentation][].

{% highlight php %}
<?php
$app->get('/', function () use ($app) {
  $data['site_name'] = $app->config->get('config.site_name');

  $app->render('home.php', $data);
})->name('home');
{% endhighlight %}

What the code above will do is look into our config directory for a file called `config.php` then inside of that file look for a key `site_name` in the array.

If our environment was production then this would look for a file called `config.php` inside of `production` directory with the key `site_name` in the array as this doesn't exist in our directory structure we defined earlier in this article it would revert to the root `config.php` file.

You will notice that this call is using the dot notation to get information from an array, this is thanks to the [Support][] package helpers.

Here is our complete code below.

{% highlight php %}
<?php
require 'vendor/autoload.php';

// Setup our environment variable base on SLIM_ENV or default to production
define('ENVIRONMENT', isset($_SERVER['SLIM_ENV']) ? $_SERVER['SLIM_ENV'] : 'production');

$app = new \Slim\Slim();

// Config code
$app->container->singleton('files', function () {
  return new Illuminate\Filesystem\Filesystem;
});

$app->container->singleton('loader', function ($c) {
  $configPath = __DIR__ . '/config';
  return new Illuminate\Config\FileLoader($c['files'], $configPath);
});

$app->container->singleton('config', function ($c) {
  return new Illuminate\Config\Repository($c['loader'], ENVIRONMENT);
});

// GET routes
$app->get('/', function () use ($app) {
  $data['site_name'] = $app->config->get('config.site_name');

  $app->render('home.php', $data);
})->name('home');

$app->get('/about', function () use ($app) {
    $app->render('about.php');
})->name('about');

$app->run();
{% endhighlight %}

[illuminate]: https://packagist.org/search/?q=illuminate
[Config]: https://packagist.org/packages/illuminate/config
[Filesystem]: https://packagist.org/packages/illuminate/filesystem
[Support]: https://packagist.org/packages/illuminate/support
[Slim]: http://slimframework.com/
[Slim DI]: http://docs.slimframework.com/#DI-Overview
[DI]: http://docs.slimframework.com/#DI-Overview
[API documentation]: http://laravel.com/api/4.1/Illuminate/Config/Repository.html
