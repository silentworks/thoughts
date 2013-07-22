The PHP Framework market landscape has been changed recently with the anticipation of Laravel 4, the most expressive
PHP Framework to have come out of the techworks. The anticipation of this framework is not only due to its easy to use
expressive syntax but is also due to the changes that L4 was bringing to the community.

This version of the Framework has been moved to a component based approach which allows other developers in the PHP
community to make use of certain aspects of the Framework outside of the Frameworks.

The most popular of this is Eloquent ORM, its a simple but powerful ORM that makes interacting with databases easy and seamless.
In my current project we are using Eloquent with Microsoft SQL Server and we had only 1 issue which we sent in a pull request for
and this was merged in. The creator of the framework made it really easy to make use of Eloquent outside of Laravel through its
Illuminate\Database package.

However there is a part of the Database package that isn't as fun or easy to work with as Eloquent and that is the Schema Builder.
I had somewhat envied Laravel 4 users having artisan to deal with all their CLI migrations necessary.
