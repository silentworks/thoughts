The PHP Framework market landscape has changed recently with the anticipation of [Laravel 4][laravel], the most expressive PHP Framework to have come out of the techworks. The anticipation of this framework is not only due to its easy to use expressive syntax but is also due to the changes that L4 was bringing to the community.

This version of the Framework has been moved to a component based approach which allows other developers in the PHP community to make use of certain aspects of the Framework outside of the Frameworks.

The most popular of this is [Eloquent ORM][eloquent], its a simple but powerful ORM that makes interacting with databases easy and seamless.

In my current project we are using Eloquent with Microsoft SQL Server and we had only one issue which we sent in a pull request for and this was merged in. The creator of the framework made it really easy to make use of Eloquent outside of Laravel through its Illuminate\Database package.

However there is a part of the Database package that isn't as fun or easy to work with as Eloquent and that is the Schema Builder.

I had somewhat envied Laravel 4 users having [Artisan][artisan] to deal with all their CLI migrations needs. I appealed on [Twitter][twitter_status] about me needing [Artisan][artisan] CLI to be available standalone, I got a reply from the one and only [Jamie York][jamie] who has always been keen to help on twitter.

I decided to take a look at the library he provided a [link][phpmig] to, but soon after realised it doensn't have support for MS SQL Server, so I quickly coded up an Adapter for MS SQL Server and was ready to go. This library was written with Dependency Injection in mind, so you could plug whatever other DBAL layer you want into it or just use raw SQL queries.

I found the instructions on the [Github][phpmig] page to be really easy to follow, so I followed the steps, for my connection I used normal PDO which I stored in the `db` container but along with this I also stored [Eloquent][eloquent] into another container called `schema`.

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
    	return new Adapter\PDO\SqlServer($container['db'], '__migrations__');
	});

The reason we are using the `PDO\SqlServer` adapter rather than creating one with the `Illuminate\Database` package is that I wanted to keep the way we access, create, update the `__migrations__` table independent of Eloquent. This means that I am not binding a user into using Eloquent for all their needs as they can access the `db` container and use raw SQL syntax.

Once we run our migration the first time, it will create a `__migrations__` table

[twitter_status]: https://twitter.com/silentworks/status/356817117619818496
[artisan]: http://laravel.com/docs/artisan
[jamie]: https://twitter.com/jamieyork/status/356907540799434752
[laravel]: http://laravel.com/
[eloquent]: http://laravel.com/docs/eloquent
[phpmig]: https://github.com/davedevelopment/phpmig