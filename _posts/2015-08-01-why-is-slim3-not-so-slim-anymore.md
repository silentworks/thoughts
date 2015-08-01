---
published: true
title: Why is Slim 3 not so slim anymore
layout: post
category: posts
comments: true
---

There is a common misconception that [Slim 3][] has plenty of files and is no longer slim. [Slim 3][] does indeed contain more files than [Slim 2][] and this has been the result of being more flexible and moving away from the Not Invented Here (NIH) philosophy.

Installing [Slim 3][] through composer will install all its dependencies, when doing a PHP file count you will notice we have doubled in file count. This is a given with the amount of flexibility we now have. Most developers might not see any benefit in this as they will likely just work with what is provided, but if at any point you should hit a limitation in any working part of the framework, you can easily swap it out without a fuss.

> composer require slim/slim:"^3.0.0-beta1"

If you are on Linux or OSX, you can get a file count using the command below.

> find ./vendor -name "*.php" | wc -l

You will also notice Slim only depend on 4 external sources, 2 of which are just contracts that Slim conforms to. 

We have removed a fair bit from the core as we don't think we should provide these out of the box when there were some developers who didn't have any use for them in the first place.

> Vegtables are good for you, but we won't force feed them to you.

Doing a file count resulted in 104 PHP files with all dependencies and composer autoload files. I have seen some state they are getting 1000+ files, this is not true, you will only get that amount of files if you are working on the framework itself using the command below, as this is the result of development dependencies.

> git clone https://github.com/slimphp/Slim.git && cd Slim && composer install

We have tried to keep to Slim's simplicity and left the developer in control as it was in [Slim 2][], we continue to improve on this leading up to the release of [Slim 3][]. We have been overwhelmed with the help of the community and think Slim is now a project driven by the community and not by an individual.

I would like to thank [Josh Lockhart][josh] the creator of Slim for allowing me to work alongside himself over the years building the framework and community. I would like to thank all the contributors who have been doing such great work on the framework and improving it by the day, to name a few [Rob Allen][rob], [Jason Coward][jason], [Glenn Eggleton][glenn], [Alex Stansfield][alex] and [Samuel Laulhau][samuel].

[Slim 3]: http://www.slimframework.com/docs
[Slim 2]: http://www.slimframeworks.com/
[josh]: https://twitter.com/codeguy
[rob]: https://twitter.com/akrabat
[jason]: https://twitter.com/drumshaman
[glenn]: https://twitter.com/geggleto
[alex]: https://twitter.com/SirMuttley
[samuel]: https://twitter.com/_samuel_
