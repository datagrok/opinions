This document serves to record the rationale behind decisions made during the design of this system.

The opinions specification will be compiled in a separate document.

# A Distributed Network of Metrics

Various pieces of software, in the form of applications and web services, collect opinions.

- Facebook and Google Plus "Like"s or "+1"s about URLs, statements, individuals, or companies.
- Reddit and Digg collect opinions about URLs in the form of up- or down-votes. They also collect opinions about comments made in their comment system in the same way.
- Slashdot's comment system is essentially the same, employing an 11-category rating system, but the categories are mutually exclusive.
- Wikipedia's [Article Feedback Tool][] employs a four-category scalar rating system.
- Amazon, NewEgg, Netflix, and Yelp collect opinions about products, movies, or restaurants in the form of rankings from 1 to 5.
- OkCupid collects opinions in the form of responses to an open-ended database of multiple-choice questions.
- Most opinion surveys may ask responders to rate their agreement according to a [Likert scale][] of "Disagree" to "Agree," with "Neither Agree nor Disagree" in between.
- PGP includes a metric that allows one to assign a "trust level" and "trust amount" to some key being signed.[^pgp-trust]

All of these services collect opinion data to perform some calculation. Examples of these calcuations include:

- providing recommendations to their users
- segmenting or "clustering" users into groups by similarity, for the purpose of introductions, networking, or targeted advertising
- describing the nature of their subscribers in aggregate

However, none of the services share their raw data with each other, or with their users. Furthermore, none of the services share the algorithms they use to make their calculations. The opinion data we give to companies on the internet becomes locked into various data silos.

We propose a general-purpose data format suitable for easy exchange of a wide variety of opinions. This enables:

- decentralized networks where users may have more control over what opinions they share, and with whom
- smarter, user-driven recommendation engines that might, for example, recommend Netflix movies based on one's Amazon purchases
- sharing of opinion data between multiple sites, for example you might expose your movie ratings to both IMDB, Netflix, and Amazon without having to tediously click hundreds of ratings widgets on all three sites.

Examples of opinions that we might capture include:

- "This news article is misleading."
- "I'd give this movie a four-star rating."
- "I am mostly certain of the authenticity of the public key for Alice."
- "This restaurant has great food."
- "This restaurant has terrible service."
- "This restaurant has decent prices."
- "I think a government should tax minimally and stay out of people's affairs."
- "I think a government should provide free health care for its citizens."
- "This Opinion _Metric_ is well-worded."

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

## Toward a solution

Let's create a system that can collect opinions about things while solving those problems.

More specifically, let's create a protocol for the exchange of abstract opinion data, and a proof-of-concept application that can speak that protocol.

For opinion data to be general-purpose and easily compatible with recommendation engines and clustering algorithms, we should optimally represent them as a single scalar value. But not all opinions conform to a single type of scalar, so we also define a metric to describe exactly how to interpret the scalar. For consistency we will define the data type of the scalar to be an unsigned 32-bit integer; metrics may define a mapping from that range to and from its preferred range.

Opinions are described by a scalar rating according to some _metric_. Metrics could be anything; let's not limit them. Let's simply say that a metric is a description of the scale used by the scalar. Targets could be anything; real things, web sites, people, avatars or public keys that represent people, etc.

	(Target, Metric, Rating)

It should be possible to convey someone else's opinion, so we need to include the Avatar (the "I" in "_I_ think _Target_ has a rating of _Rating_ according to _Metric_".)

	(Avatar, Target, Metric, Rating)

Particular Targets and Metrics may become very popular. (Consider hit movies, virally popular internet content. Also consider very well-worded and general Metrics which apply to many classes of Targets.)

To limit the amount of data we have to work with and defer decisions about ontology, we can store the full values for Avatars, Targets, and Metrics elsewhere and merely reference them from the opinion tuple.

At first glance, we might think URIs would work well for this purpose. But we wouldn't want an opinion made about the content of a URI to remain valid if the content changes. So, let's store instead the hash values of the content, not the URIs, of Targets and Metrics.

With a hash function _H_ that takes as input data of any length and outputs a fixed-length hash value, the resulting opinion tuple is now fixed-length:

	(
        H(Avatar),
        H(Target),
        H(Metric),
        Rating
        )

This decision offers some advantages and requries us to make some concessions:

One advantage is that referring to data by its hash value makes it a perfect candidate for storage in one of many high-performance Content Addressable Storage (CAS) systems. like [Venti][], a homegrown CAS like the one that `git` employs, new systems like [ipfs][] or simple implementation within other storage systems like postgres or a NoSQL engine.

Another advantage is that the values for any given hash never change, enabling simple and agrgessive caching techniques.

A concession that we might have to make when we discuss storage in high-performance distributed CAS systems is that the notion of copyright will require some interpretation. We have strong signatures to provide authenticity, but users of this system should assume that content will be aggressively cached and distributed widely, automatically. If one's ability to publish text depends on revenue from advertising that accompanies it, different schemes may have to be employed than "everyone may read content only after retrieving it from my servers, wrapped in advertisements."

Clients will probably as a convenience provide a mapping of hash values to contents, and/or to descriptions of one or more means to obtain the contents (URIs should work well here.) This may however be outside the scope of the basic protocol. Each Target, Metric, or Avatar might be sourced from multiple different URLs, including multiple different protocols. Mappings should be able to provide either a list of URLs per hash key, or repeat the hash key.

    {
        references: {
            H(Target): ["http://.../", "ftp://.../"],
            H(Metric): ["https://.../", ...],
            H(Target): ["magnet:...", ...],
            H(Avatar): ["http://pgp.mit.edu/pks/lookup?op=get&search=0x0F865001024EA507", ...],
        },
        sources: {
            H(Target): "...(raw target data)...",
            H(Metric): "...(raw target data)..."
        }
    }

Clients SHOULD, upon encountering a previously unknown hash,

1. endeavor to obtain the value for that hash by various means,
2. keep a local cache of the discovered value, and
3. redistribute the discovered value to the network.

All of those operations should be performed, or omitted, according to policies configurable by the user. These policies may interface with a trust system so that, for example, the client will attempt to retrieve values from trusted peers with high bandwidth sooner than it will from untrusted ones with low bandwidth.

Opinions may change over time, and one may wish to describe an opinion as it was in the past, so let's include a timestamp representing "a time at which this opinion was held."

	(H(Avatar), H(Target), H(Metric), Rating, Time)

We want to allow for peers and aggregation services to distribute our opinions to others, but we wouldn't want an impostor to express fake opinions in our name. So, let's sign the data.

Depending on the signature scheme, it may be redundant to include `H(Avatar)`[^pgp-redundant-avatar]

	(
        H(Avatar),
        H(Target),
        H(Metric),
        Rating,
        Time,
        SIGNATURE(
                H(Avatar),
                H(Target),
                H(Metric),
                Rating,
                Time
                )
        )

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

### What is the data format over the wire?

- Encodings don't really matter, except that we have a standard one that is easy to implement in many different languages. I feel the W3C wastes inordinant amount of time bikeshedding over schemas, attributes, and representation of data as XML without focusing enough on the abstract structure of the data itself. For opinions, any data encoding should be acceptable which:
    - Captures all the fields
    - Is 
- Opinions may(?) be encoded in RDF. (And many other data formats, as opinions are merely tuples. XML, RSS, HTML as a [MicroFormat][], etc.)

### Similarities and differences with RDF

RDF data entities (called RDF "triples") are composed of subject-predicate-object, like "Bob is 35 (years old)" or "Bob knows Fred."

RDF data seems to be assumed to be _factual_; dealing with conflicting statements represented in RDF seems out-of-scope for the system. Opinion data is assumed to be _subjective_, and conflicting statements are expected. Authenticity in RDF seems to be out-of-scope as well, while all Opinion tuples are signed. (FIXME TODO _I haven't actually read the specs, must verify that I'm not completely making things up here._)

Opinion data entities are composed of subject-object-metric-value-signature, where subject is always "I." It may be possible, depending on the metric used, to represent (some) Opinions as (a set of) RDF, but the goals of each format differ too much for a procedural transformation. For example, opinions with this metric are straightforward:

    Opinions:
        avatar: Bob
        target: Fred
        metric: is a friend of mine
        rating: 100%
        signed: Bob

    RDF:
        subject: Bob
        predicate: is friends with
        object: Fred

However, other opinions may be difficult to represent as RDF. Likert-scale opinions would require some contortions to represent both object and rating in RDF format.

    Opinions:
        avatar: Bob
        target: "free markets are the best economic policy"
        metric: "agrees with this political statement (Likert scale with 0% = disagree, 100% = agree)"
        rating: 50%
        signed: Bob
        
    RDF:
        subject: Bob
        object: "free markets are the best economic policy"
        predicate: "has an opinion about"

        subject: The URL to the above RDF tuple
        predicate: "the tuple has a value of"
        object: 50%


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
[ipfs]: http://ipfs.io/ "a global, versioned, peer-to-peer filesystem"

# TODO
- shorten titles
- johnny can't encrypt: what to do about the UI
- consideration of internationalization. merge metrics?

[Article Feedback Tool]: http://en.wikipedia.org/wiki/Wikipedia:Article_Feedback_Tool "Wikipedia: Article Feedback Tool"
[Venti]: http://en.wikipedia.org/wiki/Venti

[^pgp-trust]: OpenPGP defines a metric for trust in RFC4880 §5.2.3.13, but it is rarely exposed to users as it is difficult to understand, and can easily be conflated with various meanings of the phrase "trust a key." If A signs B's key and I trust that A is who they say they are, does that mean B is who they say they are?

[^pgp-redundant-avatar]: Depending on the signature scheme we choose, the signature key ID may be equivalent to `H(Avatar)`, and may be already included in the notion of "signing" some data. For example, OpenPGP RPC4880 §5.2.3.5 defines an "Issuer" signature subpacket which is the eight-octet OpenPGP key ID. If present, we need not encode it twice. However: it is entirely valid to sign an opinion structure at a different time than it applies to, so it is entirely valid to include our timestamp even though OpenPGP defines in §5.2.3.4 a "Signature Creation Time" mandatory signature subpacket.

