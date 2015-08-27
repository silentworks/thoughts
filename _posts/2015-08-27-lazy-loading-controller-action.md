---
published: true
layout: post
category: posts
comments: true
title: Lazy loading your controller action
---



In many popular frameworks these days you can lazy load your actions in order to not invoke every action unless it is the one matched that should be dispatched.

We have multiple ways of doing so and in this article I will talk about a few different ways of doing this. I will be using [Slim 3](http://www.slimframework.com/docs) code for examples.

### Anonymous Function

This is one of the popular approaches you might take when first starting with a micro-framework like Slim. Everything is stored in the anonymous function itself for quick and dirty prototyping.

{% highlight php %}
<?php
$app->get('/', function ($request, $response) use ($container) {
    $data = [
        'hello' => 'World',
        'name' => 'JohnnyFive'
    ];

    return $response->write(
        $container->get('template')->render('mytemplate.php', $data)
    );
});

$app->get('/world', function ($request, $response) use ($container) {
    return $response->write(
        $container->get('template')->render('myothertemplate.php', ['say' => 'Hello World'])
    );
});
{% endhighlight %}

Based on the example above, we know that if the user should hit `/world` in the browser the only bit of code that will execute is the code within that route action. In this other example I will be using a class instead which we will assume has two methods with the code from above.

{% highlight php %}
<?php
$app->get('/', function ($request, $response) use ($container) {
    return (new JohnnyFiveController($container))->hello($response);
});

$app->get('/world', function ($request, $response) use ($container) {
    return (new JohnnyFiveController($container))->say($response);
});
{% endhighlight %}

We now have a more encapsulated code, but this now feels like I am repeating myself as I would need to inject the same dependency multiple times and If this class had more methods I would have to repeat this process even more. In my opinion this is no maintainable in the long run.

### Pattern based action loading

In [Laravel](http://www.laravel.com) you have the `@` symbol to state your action for the defined route, while in [Slim](http://www.slimframework.com/) you have the `:` symbol. The issue I am finding with this approach is that my IDE cannot open the defining action by **ctrl** clicking on the string, nor can I refactor a class easily because my route actions are all defined as strings. Here is what our example above would look like in this case.

{% highlight php %}
<?php
$app->get('/', 'JohnnyFiveController:hello');

$app->get('/world', 'JohnnyFiveController:say');
{% endhighlight %}

This looks so much better, but you might ask where do I tell my `JohnnyFiveController` that `$container` should be passed into the `__construct` method, well we would have to state this elsewhere or our app would fail. We would now have to add our `JohnnyFiveController` into Slim's default Di container which is [Pimple](http://pimple.sensiolabs.org/). Meaning we would be defining what class the `JohnnyFiveController` string should be looking for in our container. To me this is tedious and time consuming and if someone was to make a mistake and reference a different class in the container pointing to the `JohnnyFiveController` string we would be giving wrong information. I am also growing less fond of this method because of the refactoring issues mentioned above.

### A Factory for all

Recently I have started toying with the idea of adding all my controllers into a factory and then use that factory inside of the anonymous function. This means my controller are only created when needed, they are encapsulated, my IDE can refactor the class easily and my IDE can go to a method definition when I **ctrl** click.

{% highlight php %}
<?php
$controllerFactory = new ControllerFactory($container);

$app->get('/', function ($request, $response) use ($controllerFactory) {
    return $controllerFactory->newJohnnyFiveController()->hello($response);
});

$app->get('/world', function ($request, $response) use ($controllerFactory) {
    return $controllerFactory->newJohnnyFiveController()->say($response);
});
{% endhighlight %}

I now only need to inject my dependencies once inside of my `ControllerFactory` and within the factory I can handle the passing around of my dependencies.

If you have a different way of handling this and would like to share, please do so in the comments below.
