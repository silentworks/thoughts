---
layout: post
title: Using Schema Builder with CLI Migrations
category: posts
comments: true
---

The PHP Framework market landscape has changed recently with the anticipation of [Laravel 4][laravel], the most expressive PHP Framework to have come out of the techworks. The anticipation of this framework is not only due to its easy to use expressive syntax but is also due to the changes that L4 was bringing to the community.

This version of the Framework has been moved to a component based approach which allows other developers in the PHP community to make use of certain aspects of the Framework outside of the Frameworks.

The most popular of this is [Eloquent ORM][eloquent], its a simple but powerful ORM that makes interacting with databases easy and seamless.

In my current project we are using Eloquent with Microsoft SQL Server and we had only one issue which we sent in a pull request for and this was merged in. The creator of the framework made it really easy to make use of Eloquent outside of Laravel through its Illuminate\Database package.

However there is a part of the Database package that isn't as fun or easy to work with as Eloquent and that is the Schema Builder.

I had somewhat envied Laravel 4 users having [Artisan][artisan] to deal with all their CLI migrations needs. I appealed on [Twitter][twitter_status] about me needing [Artisan][artisan] CLI to be available standalone, I got a reply from the one and only [Jamie York][jamie] who has always been keen to help on twitter.

I decided to take a look at the library he provided a [link][phpmig] to, but soon after realised it doensn't have support for MS SQL Server, so I quickly coded up an Adapter for MS SQL Server and was ready to go. This library was written with Dependency Injection in mind, so you could plug whatever other DBAL layer you want into it or just use raw SQL queries.

I found the instructions on the [Github][phpmig] page to be really easy to follow, so I followed the steps, for my connection I used normal PDO which I stored in the `db` container but along with this I also stored [Eloquent][eloquent] into another container called `schema`.

{% highlight php %}

use \Phpmig\Pimple\Pimple,
\Illuminate\Database\Capsule\Manager as Capsule;

$container = new Pimple();

$container['db'] = $container->share(function() {
    return new PDO('dblib:dbname=testdb;host=127.0.0.1','username','passwd');
});

$container['schema'] = $container->share(function($c) {
    /* Bootstrap Eloquent */
    $capsule = new Capsule;
    $capsule->addConnection($c['config']);
    $capsule->setAsGlobal();
    /* Bootstrap end */

    return Capsule::schema();
});

$container['phpmig.adapter'] = $container->share(function() use ($container) {
    return new Adapter\PDO\SqlServer($container['db'], 'migrations');
});

{% endhighlight %}

The reason we are using the `PDO\SqlServer` adapter rather than creating one with the `Illuminate\Database` package is that I wanted to keep the way we access, create, update the `migrations` table independent of Eloquent. This means that I am not binding a user into using Eloquent for all their needs as they can access the `db` container and use raw SQL syntax.

Once we run our migration the first time, it will create a `migrations` table. We then need to run:

{% highlight bash %}

$ vendor/bin/phpmig generate AddMyFirstTable

{% endhighlight %}
    
Once you have done this, you should have a class created for you with the necessary methods. In order to get the exposed version of [Eloquent][eloquent] in here we can access it through a `get` method provided by the `Migration` class which your current class extends.

{% highlight php %}

/**
 * Do the migration
 */
public function up()
{
    /* @var \Illuminate\Database\Schema\Blueprint $table */
    $this->get('schema')->create('posts', function ($table)
    {
        $table->increments('id');
        $table->string('title');
        $table->text('content');
        $table->timestamps();
        $table->softDeletes();
    });
}

{% endhighlight %}

You now have access to [Eloquent's][eloquent] schema builder, the bit of code above the `$this->get` is only for IDE which support autocomplete by looking at var properties in comments.

If you require even more autocomplete and also don't want to repeat the table name in the up and down method, there is an additional `init` method where you can initialize all your variables like, below is an example of the entire class.

{% highlight php %}

class AddMyFirstTable extends Migration
{
    protected $tableName;

    /* @var \Illuminate\Database\Schema\Builder $schema */
    protected $schema;
    
    public function init()
    {
        $this->tableName = 'posts';
        $this->schema = $this->get('schema');
    }

    /**
     * Do the migration
     */
    public function up()
    {
        /* @var \Illuminate\Database\Schema\Blueprint $table */
        $this->schema->create($this->tableName, function ($table)
        {
            $table->increments('id');
            $table->string('title');
            $table->text('content');
            $table->timestamps();
            $table->softDeletes();
        });
    }

    /**
     * Undo the migration
     */
    public function down()
    {
        $this->schema->drop($this->tableName);
    }
}

{% endhighlight %}

You are now able to run your migrations from the CLI while making use of [Eloquent's][eloquent] schema builder, so you get all the niceties of Laravel 4 `Illuminate\Database` package without the need of [Artisan][artisan].

Do note you can use the Schema Builder without a CLI if your wish, you could add them to files that get accessed via the url and your database would update, but what this mean is that you would have to manage all your migrations manually. With the setup above, your migrations are handled by the [PHPMig][phpmig] library.

Thanks to Taylor Otwell for creating such a great framework, thanks to Dave Marshall for creating [PHPMig][phpmig] and thanks to Jamie for pointing me to [PHPMig][phpmig].

[twitter_status]: https://twitter.com/silentworks/status/356817117619818496
[artisan]: http://laravel.com/docs/artisan
[jamie]: https://twitter.com/jamieyork/status/356907540799434752
[laravel]: http://laravel.com/
[eloquent]: http://laravel.com/docs/eloquent
[phpmig]: https://github.com/davedevelopment/phpmig
