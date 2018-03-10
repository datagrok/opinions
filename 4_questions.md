# Opinions: A Distributed Network of Metrics

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
[Venti]: http://en.wikipedia.org/wiki/Venti

[MicroFormat]: http://en.wikipedia.org/wiki/Microformat "Wikipedia: Microformat"
[Triplestore]: http://en.wikipedia.org/wiki/Triplestore "Wikipedia: Triplestore"

