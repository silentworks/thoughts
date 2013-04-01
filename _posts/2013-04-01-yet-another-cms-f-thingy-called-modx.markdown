---
layout: post
title: Yet Another CMS/F Thingy called MODX
category: posts
comments: true
---

Some of you might have seen my many rants on Twitter about the state of MODX, I have grown weary
and have now decided to __fork__ it. I have decided to also name my __fork__ "MODX", yes this is
for real, you don't believe me. [Proof right here][proof].

> I have been called [fearless][fearless], but with the unknown of __MODX__ why wouldn't I be?

In this post I will be talking about what my __MODX 3__ will bring, I will try and be as un-detailed 
as possible (yes un-detailed is a word, well so I say).

Lets address the main issue everyone keeps talking about, will I remove ExtJS from the core? No I
won't, so you might ask why I made this decision? Well dictators answer to no-one, so there is your anwers.

I also see a number of you complaining about why is __MODX__ so far behind with all the new features
PHP has to offer and all the new cool stuff like [Composer][composer]. I can't say why its so far
behind but that is changing in my __MODX 3__, you will be able to install it with [Composer][composer],
I will outline the steps you will need to take below:

* Clone repo off Github (yes you need to posses a degree in Git first)
* Do _composer install_ in your terminal at the root of the repo
* Do _modx.phar install_ to get the system setup
* Next you will need to make sure you have PEAR setup (you might ask, what do I need this for? well nothing at all)

You should also know I have opted for [__Twig__][twig] as the template parser, yes I know you don't like logic
in your templates. Luckily enough I created a _Morpheus Complexius Zubberium Snippet_ manager
on top of [__Twig__][twig], how does this work? I shall explain below.

> A Snippet calls a Snippet that calls a Chunk that returns a Snippet.

Now I know the current repo looks just like __MODX Revolution__ repo, but don't be fooled. I have been
developing this for the past 14 years with the help of [this guy][thisguy]. Yes he works for
the other __MODX__, but he has been helping me unknowningly. I will explain about him later.

### Versioning

This is a major concern for every developer who uses __MODX__, when will the next minor version be out?
when will the next major version be out? I have investigated this and have a viable solution.

TIME TRAVEL

Yes I am being real, this has been made possible by [this guy][thisguy], you don't believe me? go 
checkout [this extra][thisextra] he created before MODX even began.

I think the current release cycle (if we can call it that) is busted. You have to wait around 3 months
per minor release and 3 years per major release. I have adopted the Chrome release cycle for my __MODX__,
yes that means you should have MODX 5 before [Drupal 8][drupal] is ever released.

### Bug Fixing

The current process is flawed, I mean so flawed. So person finds a bug, person reports it to
the __MODX team__ via the tracker bug tracking thingy. __MODX team__ evaluates it and then fix it some
weeks later. Now that is definitely the wrong way to run a open source project.

Here is how I will be dealing with this, person finds a bug, person reports the bug to me, I tell
person who find the bug to fix it themself. If I didn't find it, it doesn't exist otherwise it just
plain old user error or browser issue.

### Roadmap

This is a big issue with the current __MODX team__, WHERE IS THE ROADMAP? well it [here][here], actually
its on the tracker. See its all uncertainty.

I have revised this, I asked my Son to create a roadmap for me but he was too busy to complete it,
so I did one myself. You can find the [roadmap here][roadmap].

> My initiative is copy someone elses work and say its yours because you are awesome.

Go on people's, get forking and send in your PRs like nobody's business, but do remember I don't fix
other people's bugs.

If you have any questions, follow and ping me on Twitter- I'm [@silentworks][twitter].

[composer]: http://composer.org/
[proof]: http://github.com/silentworks/modx
[thisguy]: http://twitter.com/sepiariver
[thisextra]: http://modx.com/extras/package/visualsitemap
[here]: http://rtfm.modx.com/display/revolution20/Roadmap
[roadmap]: https://maps.google.co.uk/maps?f=q&source=s_q&hl=en&geocode=&q=25+Highland+Park+Village,+Dallas,+TX,+United+States&aq=0&oq=25+Highland+Park+Village,+Dallas,+TX&sll=53.800651,-4.064941&sspn=7.049238,21.643066&vpsrc=0&ie=UTF8&hq=&hnear=25+Highland+Park+Village,+Dallas,+Texas+75205,+United+States&ll=32.836111,-96.806653&spn=0.00979,0.021136&t=m&z=16&iwloc=A
[drupal]: http://buytaert.net/starting-to-work-on-drupal-8
[twig]: http://twig.sensiolabs.org/
[twitter]: https://twitter.com/silentworks
[fearless]: https://twitter.com/jpdevries/status/318449404216475649