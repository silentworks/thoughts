---
published: true
layout: post
category: posts
comments: true
---



Recently folks have been asking about what middlewares are available for [Slim 3][]. Although [Slim 3][] is still in RC stage, there have been a number of developers who have decided to create middlewares already. Here I am going to list the ones I think are beneficial to the community and to keep an eye on.

Before I continue, do note that most PSR7 middlewares should work with [Slim 3][], we have intentionally not included an Interface for PSR7 middlewares in order to keep the PHP eco-system healthy. We also promote the use of PSR7 methods in middlewares and not use Slim specific methods where possible.

### List of Middlewares

#### Authentication

Mika Tuupola's Basic Auth has been around since [Slim 2][] and now have a new 2.x branch for [Slim 3][], this library includes different authenticator stores (array, db or create your own) out of the box. 

https://github.com/tuupola/slim-basic-auth

Mika Tuupola's JSON Web Token Authentication has also been around since [Slim 2][] and now have a new 2.x branch for [Slim 3][].

https://github.com/tuupola/slim-jwt-auth

#### Content Negotiation

Ryan Szrama's Negotiation middleware for [Slim 3][], which makes use of Will Durand [Negotiation][] library. 

https://github.com/rszrama/negotiation-middleware

#### Http Cache

There is a HTTP Cache middleware that was created by the Slim Team.

https://github.com/slimphp/Slim-HttpCache

#### CSRF

Another one from the Slim Team is the CSRF middleware.

https://github.com/slimphp/Slim-Csrf

If you know of any other middleware not listed here that you would like to be listed, please leave a comment below with a link to the repository. Please note that I am only listing middlewares here, not service providers that you can add into [Slim 3][], hence why I didn't list the [Twig-View][] and others.

[Negotiation]: https://github.com/willdurand/Negotiation
[Slim 3]: http://www.slimframework.com/docs
[Slim 2]: http://docs.slimframeworks.com/
[Twig-View]: https://github.com/slimphp/Twig-View
