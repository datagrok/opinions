# Opinions: A Distributed Network of Metrics

## Problems we try to solve

### Only one metric for "good"

Many users up-vote articles or statements whose opinions they agree with, regardless of their quality. If they represent the majority of users on a site, then effectively the site's metric for "good" becomes: "do you agree?"

Is an article informative but poorly-written? Exciting but poorly researched? Does a particular movie have a great story but horrible acting? With only one metric for "good" these opinions cannot be captured or calculated.

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook and almost every other "social" site ever created.

Solution: The Opinions data structure provides an abstract, extensible way to represent a metric. Metrics which are poorly-defined may be abandoned for better ones at the whim of the users. Users may form opinions about the Metrics themselves, represented by Opinion data structures.

### Extremely naive algorithm (popular vote) for calculating score from "goodness" metric.

For many opinion-collecting sites, as their communities get larger, minority interests become excluded. The marginalized, whose greatest champion for community-building should be the Internet, are ignored. The winningest content is that with the most votes, even if less-generally-popular content is highly desirable to a subset of the users. The site "descends into mediocrity."

I want a social news site where both a religious neo-conservative Republican and a far-left liberal pacifist atheist socialist can both find recommended to them links that they would consider "good" and comments that they would not necessarily agree with, but consider to be on-topic and/or well-informed.

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook, etc...

Solution: we do not specify an algorithm for calculating recommendations. Instead, we specify a format which allows users (and services) to collect and share opinion data, so they can do a variety of analytics against that data themselves.

### Limited to a single topic.

OKCupid and Netflix are the rare examples of sites that have effective solutions for the problems mentioned above with Reddit and others.

OKCupid implements a free-form unlimited-number of metrics and has a good (albeit proprietary) algorithm for clustering based on that metric. Unfortunately, all of their metrics and analyses are specific to online dating.

Netflix has a well-defined metric for "good" (albeit only one) and a famously good (and public) algorithm for calculating a score. Unfortunately, the data they collect is specific to rating movies.

Guilty: OKCupid, Netflix

Solution: All opinion data is representable by a common data structure, enabling algorithms to employ data from disparate sources.

### Proprietary, closed database

In almost all of these systems, the data is locked up. We couldn't fix the problems or run our own calculation algorithms across it if we wanted to. If you spend an hour responding to an opinion survey, you have no ability to save those responses to answer similar questions on other or future surveys.

Guilty: OKCupid, Netflix, Reddit, Digg, Slashdot, Google Plus, Facebook, ...

Solution: We cannot stop services from closing their databases, but we can enable the creation of open ones. With popularity, services would have to adapt to what users prefer.

### Ambiguous metrics.

When using a public key cryptosystem like PGP, does "trust" mean "I am certain this is Alice's key" or "I am certain that Alice knows what she's doing when she authenticates someone else's key"? What if the subject is not keys but messages? Does trust mean "I am certain Alice wrote this" (it sounds just like her!) or, "I am certain that noone could possibly have modified a message that Alice wrote before it reached me"?

Reddit [takes great pains to insist](http://www.reddit.com/help/reddiquette) that "The down [vote] is for comments that add nothing to the discussion." However, the downvote has a different meaning for articles. Many users simply ignore this advice, making it a perpetual source of contention and repetitous discussion.

Guilty: PGP and other cryptosystems' metric of "trust." Reddit's "upvote."

Solution: Metrics themselves may be as specific or general as users wish. Poorly-defined metrics may themselves gain a reputation for being so within the framework.

### The "real name" initiative, aka [Nymwars][]

Many social media sites and pundits insist that one must use their "real" Government-associated name in their online persona. At the very least, this is an assault on the freedoms of speech and expression, and [disenfranchises the marginalized][real names policy].

Strong PGP-style cryptography coupled with a decentralized system for reputation is a sufficent solution to this problem. Implemented with a PKI and signatures, it enables avatars to earn reputation, discouraging the creation of and enabling others to preemptively filter out "throwaway" avatars, while providing plausible deniability (in one sense) and protecting users.

[JWZ on Nym Wars](http://www.jwz.org/blog/2011/08/nym-wars/)

[real names policy]: http://geekfeminism.wikia.com/wiki/Who_is_harmed_by_a_%22Real_Names%22_policy%3F "GFW: Who is harmed by a 'real names' policy?"
[Nymwars]: http://en.wikipedia.org/wiki/Nymwars "Wikipedia: Nymwars"

Guilty: Facebook and Google Plus

Solution: we provide a framwork wherein _identity_ is synonymous with _public key_; users are encouraged to create many of them.
