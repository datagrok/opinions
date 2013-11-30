This document serves to record the rationale behind decisions made during the design of this system.

The opinions specification will be compiled in a separate document.

# A Distributed Network of Metrics

Various pieces of software, in the form of applications and web services, collect opinions. 

- Reddit and Digg collect opinions about URLs in the form of up- or down-votes. They also collect opinions about comments made in their comment system in the same way. Slashdot's comment system has a slightly more granular metric but is essentially the same.

- Google Plus and Facebook collect opinions about URLs in the form of "Like"s or "+1"s.

- Amazon, NewEgg, and Netflix collect opinions about products or movies in the form of rankings between 1 to 5.

- OkCupid collects opinions in the form of responses to an open-ended database of multiple-choice questions.

- Most opinion surveys may ask responders to rate their agreement according to a [Likert scale][] of "Disagree" to "Agree," with "Neither Agree nor Disagree" in between.

All of these sites perform a calculation employing opinion data to provide recommendations to their users, to segment or "cluster" their users into groups by similarity, to describe the nature of their subscribers in aggregate, and various other things.

However, none of them share their raw data with each other, or with their users.

## Problems we try to solve

### Only one metric for "good"

Many users up-vote articles or statements whose opinions they agree with, regardless of their quality. If they represent the majority of users on a site, then effectively the site's metric for "good" becomes: "do you agree?"

Is an article informative but poorly-written? Exciting but poorly researched? Does a particular movie have a great story but horrible acting? With only one metric for "good" these opinions cannot be captured or calculated.

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook and almost every other "social" site ever created.

### Extremely naive algorithm (popular vote) for calculating score from "goodness" metric.


For these sites, as their communities get larger, minority interests and opinions become excluded. The marginalized, whose greatest champion for community-building should be the Internet, are ignored. The winningest content is that with the most votes, even if less-generally-popular content is highly desirable to a subset of the users. The site "descends into mediocrity."

I want a social news site where both a religious neo-conservative Republican and a far-left liberal pacifist atheist socialist can both find recommended to them links that they would consider "good" and comments that they would not necessarily agree with, but consider to be on-topic and/or well-informed.

Guilty: Reddit, Digg, Slashdot, Google Plus, Facebook, etc...

### Limited to a single topic.

OKCupid and Netflix are the rare examples of sites that have effective solutions for the problems mentioned above with Reddit and others.

OKCupid implements a free-form unlimited-number of metrics and has a good (albeit proprietary) algorithm for clustering based on that metric. Unfortunately, all of their metrics and analyses are specific to online dating.

Netflix has a well-defined metric for "good" (albeit only one) and a famously good (and public) algorithm for calculating a score. Unfortunately, the data they collect is specific to rating movies.

Guilty: OKCupid, Netflix

### Proprietary, closed database

In almost all of these systems, the data is locked up. We couldn't fix the problems or run our own calculation algorithms across it if we wanted to. If you spend an hour responding to an opinion survey, you have no ability to save those responses to answer similar questions on other or future surveys.

Guilty: OKCupid, Netflix, Reddit, Digg, Slashdot, Google Plus, Facebook, ...

### Ambiguous metrics.

When using a public key cryptosystem like PGP, does "trust" mean "I am certain this is Alice's key" or "I am certain that Alice knows what she's doing when she authenticates someone else's key"? What if the subject is not keys but messages? Does trust mean "I am certain Alice wrote this" (it sounds just like her!) or, "I am certain that noone could possibly have modified a message that Alice wrote before it reached me"?

Reddit [takes great pains to insist](http://www.reddit.com/help/reddiquette) that "The down [vote] is for comments that add nothing to the discussion." However, the downvote has a different meaning for articles. Many users simply ignore this advice, making it a perpetual source of contention and repetitous discussion.

Guilty: PGP and other cryptosystems' metric of "trust." Reddit's "upvote."

### The "real name" initiative, aka [Nymwars][]

Many social media sites and pundits insist that one must use their "real" Government-associated name in their online persona. At the very least, this is an assault on the freedoms of speech and expression, and [disenfranchises the marginalized][real names policy].

Strong PGP-style cryptography coupled with a decentralized system for reputation is a sufficent solution to this problem. Implemented with a PKI and signatures, it enables avatars to earn reputation, discouraging the creation of and enabling others to preemptively filter out "throwaway" avatars, while providing plausible deniability (in one sense) and protecting users.

[JWZ on Nym Wars](http://www.jwz.org/blog/2011/08/nym-wars/)

[real names policy]: http://geekfeminism.wikia.com/wiki/Who_is_harmed_by_a_%22Real_Names%22_policy%3F "GFW: Who is harmed by a 'real names' policy?"
[Nymwars]: http://en.wikipedia.org/wiki/Nymwars "Wikipedia: Nymwars"


Guilty: Facebook and Google Plus

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

### An attack-resistant distributed system for secure pseudonymous reputation 

## Proof-of-concept implementation goals (?)

- Protocol description
    - Driven by proof-of-concept implementation -- decisions in protocol should
      be resolved in favor of what makes for the easiest implementation without
      sacrificing security.
- Libraries for manipulating and exchanging opinion data in Python (and C?)
- An opinions aggregation and distribution service
- A social-news site founded on opinion data
- A simple movie rankings database founded on opinion data
- Clients might be seeded with a set of good or popular Metrics
	- Movies
	- MetaMetrics -- "This metric is unbiased" "This metric is generally useful" etc.
	- Political stance; encode all questions from [The Political Compass][]
	- Personality traits such as those for calculating the [Myers-Briggs Type Indicator][] or the [Keirsey Temperament Sorter][]
- A client for importing and managing a corpus of opinions

## Questions

### Similarities and differences with RDF

RDF data entities (called RDF "triples") are composed of subject-predicate-object, like "Bob is 35 (years old)" or "Bob knows Fred."

Opinion data entities are composed of subject-object-metric-value-signature, where subject is implied and always "I". It may be possible to procedurally encode Opinions into RDF, perhaps for storage into a high-performance [Triplestore][].

RDF data seems to be assumed to be factual; dealing with conflicting statements represented in RDF seems out-of-scope for the system. Authenticity seems to be out-of-scope as well. (FIXME I haven't actually read the specs, verify that I'm not completely making things up here.)

Opinions are assumed to be just that: opinions, which may not be objective statements of truth. Thus it is important to bind opinions to the principals who issue them, and give consideration to how to present conflicting opinions.

- Opinions may(?) be encoded in RDF. (And many other data formats, as opinions are merely tuples. XML, RSS, HTML as a [MicroFormat][], etc.)

FIXME further study and elaboration. Possible of opinions to build on the existing mountain of RDF work without embracing its complexity and giant specifications?

### How should we store this data locally? What are the space and bandwidth requirements?

- Hashes will likely be fixed-bitlength; sha1 is 160 bits.
- Values could be a simple 8-bit byte; for metrics requiring only a boolean would use 0 for false/disagree and 255 for true/agree.
- Signatures vary but would likely require the same space as a hash plus perhaps another hash corresponding to a public key.
- Timestamps could be TAI64; 64 bits.
- Maps from hash values to URIs could live in any key-value store.
- For the protocol design we need not worry too much about storage details. But consider them anyway to avoid design mistakes.
- Serialization of some data structure for encryption is critically important. Serialization for transmission over the wire is unimportant, but benefits from the existence of a standard.

### How do we selectively distribute opinions?

I might not want to expose certain sets of opinions to certain people. How do we identify those we are sharing with, and how do we filter our opinion set for them?

While accepting that there's nothing short of DRM schemes that one could use to *force* trusted peers not to redistribute their opinion, what methods could one employ to *request* that they do not?

### How do we discover new people?

Opinion forwarding? This might imply that each client node on the network might need to store a copy of all opinion data for every other node it ever interacts with. This might be too heavy of a data load for clients. Maybe clients only store opinion data for metrics they have employed? Maybe clients only tak

Aggregation services? Though the goal is to be distributed, having an open platform for which an aggregation service is optional, and for which no single aggregation service can "take over," can be useful. C.f. git vs. GitHub; RSS vs. Google Reader.

### How do we determine the "network's opinion"? How do we determine "similar people's opinion?" How do we calculate who is "similar"?

Various approaches are possible. One that I would like to see is: calculate a weighted average of other people's opinions for a given Thing, where higher weight is given to people calculated (by their other opinions) to be "similar to me." Allow the user to dig in and examine the frequency distribution of opinions, etc.

[Cluster analysis][] is a mature, well-researched and oft-published topic in mathematics. Once we have this corpus of opinion data, we need to investigate the performance of various clustering algorithms against it.

My hypothesis is that even a naive application of simpler cluster analysis techniques like those using [Singular Value Decomposition][SVD] will immediately perform better than Reddit's popularity metric.

Investigate the various techniques employed by [Netflix Prize][] winners.

[Netflix Prize]: http://en.wikipedia.org/wiki/Netflix_Prize "Wikipedia: Netflix Prize"

Algorithms may need to be _attack-resistant_, to make it difficult for one party to disproportionately influence the "network's opinion."

### How do we perform the "similar people" calculation across unknown parts of the network and not just one's known peers, without revealing "friends-only" opinions?

Query forwarding like [gnutella][]?

[gnutella]: http://en.wikipedia.org/wiki/Gnutella "Wikipedia: Gnutella"

### Can we distribute a pre-calculated "network opinion" rating to peers?

For any given clustering algorithm, it would be useful to distribute the computational load of the clustering algorithm across the network, where possible. The most immediate concern is: how might we trust other nodes to return correct results?

[Cluster Analysis]: http://en.wikipedia.org/wiki/Cluster_analysis "Wikipedia: Cluster analysis"

[SVD]: http://en.wikipedia.org/wiki/Singular_value_decomposition "Wikipedia: Singular Value Decomposition"

### How do we receive updates to changed opinions?

Polling? Publish/subscribe among peers? Revocation lists ala PGP?

### How do we deal with problems inherent in [Likert scale][] items?

Many metrics will take on the form of a Likert item. From the Wikipedia article on the Likert scale,

> "Respondents may avoid using extreme response categories (central tendency bias); agree with statements as presented (acquiescence bias); or try to portray themselves or their organization in a more favorable light (social desirability bias)."

- To deal with acquiescence bias, perhaps information could be encoded into some metrics that let clients know how to present the negation of a given metric.
- Hopefully the ability to create pseudononymous avatars will reduce the tendency for social desirability bias.
- How do we deal with central tendency bias? Extra descriptive text that clearly defines the feeling one might have upon selecting a particular value?

### What about echo chambers or "filter bubbles"?

Opinion data forms the base upon which a recommendation engine may be built, to recommend content, stories, statements, or relationships.

Some are concerned that recommending things that appeal to someone's interests forms an echo chamber, in which all media that a person consumes is that which they already agree with. It is bad to hide from opposing views.

Since Opinions are fine-grained it becomes possible to evaluate content by more dimensions than simply "I agree." Having data available makes it possible to create recommendation algorithms that take into account not only agreement but also technical quality.

We feel that is is rather premature to be worrying about "filter bubbles" when most of today's popular media sites contain poor implementations of recommendation engines. (If they feature them at all!)

"Filter bubbles" may indeed become an issue in a future where opinoin data is available and sites make use of recommendations, but that does not undermine efforts to implement filtering and recommendation engines whatsoever. The solution to "filter bubbles" will be a minor adjustment to existing, good recommendation algorithms, and it should be user-configurable: "show highly-rated opposing views" should be a check-box on a configuration screen.

[burst]: http://www.technologyreview.com/view/522111/how-to-burst-the-filter-bubble-that-protects-us-from-opposing-views/ "How to Burst the 'Filter Bubble' that Protects Us from Opposing Views"

## Related Reading

- [Attack Resistant Trust Metrics, Raph Levien](http://www.levien.com/thesis/compact.pdf)
- [Advogato's Trust Metric](http://advogato.org/trust-metric.html)

[MicroFormat]: http://en.wikipedia.org/wiki/Microformat "Wikipedia: Microformat"
[Likert scale]: http://en.wikipedia.org/wiki/Likert_scale "Wikipedia: Likert scale"
[Triplestore]: http://en.wikipedia.org/wiki/Triplestore "Wikipedia: Triplestore"
[The Political Compass]: http://www.politicalcompass.org
[Myers-Briggs Type Indicator]: http://en.wikipedia.org/wiki/Myers-Briggs_Type_Indicator_(MBTI) "Wikipedia: Myers-Briggs Type Indicator"
[Keirsey Temperament Sorter]: http://en.wikipedia.org/wiki/Keirsey_Temperament_Sorter "Wikipedia: Keirsey Temperament Sorter"

# TODO
- shorten titles
- include [Article Feedback Tool]
- johnny ccan't encrypt: what to do about the UI
- consideration of internationalization. merge metrics?

[Article Feedback Tool]: http://en.wikipedia.org/wiki/Wikipedia:Article_Feedback_Tool "Wikipedia: Article Feedback Tool"
