In many popular frameworks these days you can lazy load your actions in order to not invoke every action unless it is the one matched that should be dispatched.

We have multiple ways of doing so and in this article I will talk about a few different ways of doing this.

### Anonymous Function

This is one of the popular approaches you might take when first starting with a micro-framework like Slim. Everything is stored in the anonymous function itself for quick and dirty prototyping.

{% highlight php %}
$app->get('/', function ($request, $response) use ($template) {
    $data = [
        'hello' => 'World',
        'name' => 'JohnnyFive'
    ];

    return $response->write(
        $template->render('mytemplate.php', $data)
    );
});

$app->get('/world', function ($request, $response) use ($template) {
    return $response->write(
        $template->render('myothertemplate.php', ['say' => 'Hello World'])
    );
});
{% endhighlight %}

Based on the example above, we know that if the user should hit `/world` in the browser the only bit of code that will execute is the code within that route action. In this other example I will be using a class instead which we will assume has two methods with the code from above.

{% highlight php %}
$app->get('/', function ($request, $response) use ($template) {
    return (new JohnnyFiveController($template))->helloJohnnyFive($response);
});

$app->get('/world', function ($request, $response) use ($template) {
    return (new JohnnyFiveController($template))->say($response);
});
{% endhighlight %}

We now have a more encapsulated code, but this now feels like I am repeating myself as I would need to inject the same dependency multiple times and If this class had more methods I would have to repeat this process even more. In my opinion this is no maintainable in the long run.

### Pattern based action loading

In Laravel you can use the `@` symbol to state your action for the defined route, while in Slim you would use the `:` symbol. The issue I am finding with this approach is that my IDE cannot go to the defining class by ctrl clicking on the string, nor can I refactor a class easily because my route actions are all defined as strings.
