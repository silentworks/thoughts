In many popular frameworks these days you can lazy load your actions in order to not invoke every action unless it is the one matched that should be dispatched.

We have multiple ways of doing so and in this article I will talk about a few different ways of doing this.

### The Route Factory

This is one of the popular approaches you might take when first starting with a micro-framework like Slim. Everything is stored in the factory itself for quick and dirty prototyping.

{% highlight php %}
$app->get('/', function () use ($app) {
    $data = [
        'hello' => 'World',
        'name' => 'JohnnyFive'
    ];

    $app->render('mytemplate.php', $data);
});

$app->get('/world', function () {
    echo "Hello World";
});
{% endhighlight %}

Based on the example above, we know that if the user should hit `/world` in the browser the only bit of code that will execute is the `echo "Hello World";`

In Laravel you can use the `@` symbol to state your action for the defined route, while in Slim you would use the `:` symbol. The issue I am finding with this approach is that my IDE cannot go to the defining class by ctrl clicking on the string, nor can I refactor a class easily because my route actions are all defined as strings.
