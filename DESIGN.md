This document serves to record the rationale behind decisions made during the design of this system.

# A Distributed Network of Metrics

Various software, in the form of applications and web services, collect opinions.

## Problems

### Only one metric for "good."

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook and almost every other "social" site ever created.

Many users may rank as "good" articles or statements whose opinions they agree with, regardless of their quality. Is an article informative but poorly-written? Exciting but poorly researched? Does a particular movie have a great story but horrible acting?

### Extremely naive algorithm (popular vote) for calculating score from "goodness" metric.

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook, etc...

For these sites, as their communities get larger, minority interests and opinions become excluded. The winningest content is that with the most votes, even if the content is highly desirable to a subset of the users. The site "descends into mediocrity."

I want a social news site where both a religious neo-conservative Republican and a far-left liberal pacifist atheist socialist can find recommended to them links that they would consider "good" and comments that they would not necessarily agree with, but would consider on-topic and/or well-informed.

### Limited to a single topic.

Guilty: OKCupid, Netflix

OKCupid and Netflix are the rare examples of sites that have effective solutions for the problems mentioned above with Reddit and others.

OKCupid implements a free-form unlimited-number of metrics and has a good (albeit proprietary) algorithm for clustering based on that metric. Unfortunately, all of their metrics and analyses are specific to online dating.

Netflix has a well-defined metric for "good" (albeit only one) and a famously good (and public) algorithm for calculating a score. Unfortunately, the data they collect is specific to rating movies.

### Proprietary, closed database

Guilty: OKCupid, Netflix, Reddit, Digg, Slashdot, Google Plus, Facebook, ...

In almost all of these systems, the data is locked up. We couldn't fix the problems or run our own calculation algorithms across it if we wanted to.

### Only one metric for "trust."

Guilty: PGP

When using a public key cryptosystem like PGP, does "trust" mean "I am certain this is Alice's key" or "I am certain that Alice knows what she's doing when she authenticates someone else's key"? What if the subject is not keys but messages? Does trust mean "I am certain Alice wrote this" (it sounds just like her!) or, "I am certain that noone could possibly have modified a message that Alice wrote before it reached me"?

### The "real name" initiative, aka [Nymwars][]

Guilty: Facebook and Google Plus

Many social media sites and pundits insist that one must use their real name in their online persona. At the very least, this is an assault on the freedoms of speech and expression.

[JWZ on Nym Wars](http://www.jwz.org/blog/2011/08/nym-wars/)

[Nymwars]: http://en.wikipedia.org/wiki/Nymwars "Wikipedia: Nymwars"

## Toward a solution

Let's create a system that can collect opinions about things while solving those problems.

Opinions like:

- This news article is misleading.
- I'd give this movie a four-star rating.
- I am mostly certain of the authenticity of the public key for Alice.
- This restaurant has great food.
- This restaurant has terrible service.
- This restaurant has decent prices.
- I think a government should tax minimally and stay out of people's affairs.
- I think a government should provide free health care for its citizens.
- This metric is well-worded.

More specifically, let's create a protocol for the exchange of abstract opinion data, and a couple proof-of-concept applications that can speak that protocol.

Opinions are described by a scalar rating according to some _metric_. Metrics could be anything; let's not limit them. Let's simply say that a metric is a description of the scale used by the scalar. Things could be anything; real things, web sites, people, avatars or public keys that represent people, etc.

	(Thing, Metric, Rating)

It should be possible to convey someone else's opinion, so we need to inlcude the Principal (the "I" in "I think _Thing_ has a rating of _Rating_ according to _Metric_".)

	(Principal, Thing, Metric, Rating)


Particular Things and Metrics may become very popular. (Consider hit movies, virally popular internet content. Also consider very well-worded and general Metrics which apply to many classes of Things.)

To limit the amount of data we have to work with and defer decisions about ontology, we can store the full values for Principals, Things, and Metrics elsewhere and merely reference them from the opinion tuple. URIs might work well for this purpose.

But we wouldn't want an opinion made about the content of a URI to remain valid if the content changes, so let's store the hash values of the content, not the URI, of Things and Metrics.

	(H(Principal), H(Thing), H(Metric), Rating)

Clients will probably as a convenience provide a mapping of hash values to contents, or a description of how to obtain the contents (URIs should work well here.) This may however be outside the scope of the basic protocol.

	URIs = {
		H(Thing): "http://.../",
		H(Metric): "http://.../",
		H(Thing): "file://.../",
		H(Metric): "...(actual metric data)...",
	}

Opinions may change over time, so let's include a timestamp.

	(H(Principal), H(Thing), H(Metric), Rating, Time)

We want to allow for peers and aggregation services to distribute our opinions to others, but we wouldn't want an impostor to express fake opinions in our name. So, let's sign the data.

A signature will not validate without the correct public key, so including the Principal in the signed data is redundant.

	Understanding that SIGNED(x) means (x, SIGNATURE(x)),

	(H(Principal), SIGNED(H(Thing), H(Metric), Rating, Time))

TODO: check my crypto textbooks: is it safe to omit the Principal from the signature leave the signer out of the signed document?

## Use cases / stories

## Proof-of-concept implementation goals (?)

- Protocol description
- Libraries for manipulating and exchanging opinion data in Python and C
- A social-news site founded on opinion data
- A simple movie rankings database founded on opinion data
- An opinions aggregation and distribution service
- Clients might be seeded with a set of good or popular Metrics

## Questions

### Similarities and differences with RDF

TODO

	Opinions: (Principal):±² (Thing, Metric, Value)

	RDF: (Thing, Thing, Relationship)

- RDF data may be free-form and extensive. An opinion is a tiny nugget of data.

TODO

### How should we store this data locally? What are the space and bandwidth requirements?

- Hashes will likely be fixed-bitlength; sha1 is 160 bits.
- Values could be a simple 8-bit byte; for metrics requiring only a boolean would use 0 for false/disagree and 255 for true/agree.
- Signatures vary but would likely require the same space as a hash plus perhaps another hash corresponding to a public key.
- Timestamps could be TAI64; 64 bits.
- Maps from hash values to URIs could live in any key-value store.
- For the protocol design we need not worry too much about storage details. But consider them anyway to avoid design mistakes.

### How do we selectively distribute opinions?

I might not want to expose certain sets of opinions to certain people. How do we identify those we are sharing with, and how do we filter our opinion set for them?

While accepting that there's nothing short of DRM schemes that one could use to *force* trusted peers not to redistribute their opinion, what methods could one employ to *request* that they do not?

### How do we calculate "similar people"? How do we discover new people?

Corollary: how do we perform the "similar people" calculation across unknown parts of the network and not just one's known peers without revealing "friends-only" opinions?

Opinion forwarding?

Aggregation services? Though the goal is to be distributed, having an open platform for which an aggregation service is optional, and for which no single aggregation service can "take over," can be useful. C.f. git vs. GitHub; RSS vs. Google Reader.

Query forwarding like [gnutella][]?

[gnutella]: http://en.wikipedia.org/wiki/Gnutella "Wikipedia: Gnutella"

### How do we determine the "network's opinion"?

Various approaches are possible. One that I would like to see is: calculate a weighted average of other people's opinions for a given Thing, where higher weight is given to people calculated (by their other opinions) to be "similar to me." Allow the user to dig in and examine the frequency distribution of opinions, etc.

[Cluster analysis][] is a mature, well-researched and oft-published topic in mathematics. Once we have this corpus of opinion data, we need to investigate the performance of various clustering algorithms against it.

My hypothesis is that even a naive application of simpler cluster analysis techniques like those using [Singular Value Decomposition][SVD] will immediately perform better than Reddit's popularity metric.

Investigate the various techniques employed by [Netflix Prize][] winners.

[Netflix Prize]: http://en.wikipedia.org/wiki/Netflix_Prize "Wikipedia: Netflix Prize"

### Can we distribute a pre-calculated "network opinion" rating to peers?

For any given clustering algorithm, it would be useful to distribute the computational load of the clustering algorithm across the network, where possible.

[Cluster Analysis]: http://en.wikipedia.org/wiki/Cluster_analysis "Wikipedia: Cluster analysis"

[SVD]: http://en.wikipedia.org/wiki/Singular_value_decomposition "Wikipedia: Singular Value Decomposition"

### How do we receive updates to changed opinions?

Polling? Publish/subscribe among peers? Revocation lists ala PGP?

## Related Reading

- [Attack Resistant Trust Metrics, Raph Levien](http://www.levien.com/thesis/compact.pdf)
- [Advogato's Trust Metric](http://advogato.org/trust-metric.html)
