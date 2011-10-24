# A Distributed Network of Metrics

## Problems

Two problems with Reddit, Digg, Slashdot, Google Plus, Facebook, and almost every other "social" site ever created: there's only one metric for what is considered "good," and there is only one (extremely naive) algorithm for calculating a score from that metric. As their communities get larger, minority interests and opinions become excluded. I want a social news site where both a religious neo-conservative Republican and a far-left liberal pacifist atheist socialist can find recommended to them links that they would consider "good" and comments that they would not necessarily agree with, but consider on-topic or well-informed.

A problem with OKCupid and Netflix: They are the rare examples of sites that have effective solutions for the problems mentioned above for Reddit etc., but they are specific to dating or movie ratings.

A problem with PGP: There's only one metric for "trust." Does "trust" mean "I am certain this is Alice's key" or "I am certain that Alice knows what she's doing when she authorizes someone else's key"?

A problem with most of these systems: the data is locked up. We couldn't fix the problems if we wanted to.

A problem with Facebook and Google Plus: the "real name" initiative. Many social media sites and pundits insist that one must use their real name in their online persona. At the very least, this is a threat to freedoms of speech and expression.

## Toward a solution

Let's create a system that can collect opinions about things.

Opinions like:

- This news article is misleading.
- I'd give this movie a four-star rating.
- I am mostly certain of the authenticity of the public key for Alice.
- This restaurant has great food.
- This restaurant has terrible service.
- This restaurant has decent prices.
- I think a government should tax minimally and stay out of people's affairs.
- I think a government should provide free health care for its citizens.

Opinions are described by a scalar rating according to some metric. Metrics could be anything; let's not limit them. Let's simply say that a metric is a description of the scale used by the scalar. Things could be anything; real things, web sites, people, avatars or public keys that represent people, etc.

(Thing, Metric, Rating)

To limit the amount of data we have to work with and defer decisions about ontology, let's use URIs to refer to the actual values Things and Metrics. But we wouldn't want an opinion made about the content of a URI to remain valid if the content changes, so let's store the hash values for Things and Metrics.

(H(Thing), H(Metric), Rating)
URIs = {
	H(Thing): http://...../
	H(Metric): http://...../
}

We wouldn't want an impostor to express fake opinions in our name, so let's sign the data. Also, opinions may change over time, so let's include a timestamp.

(H(Thing), H(Metric), Rating, timestamp, S((H(Thing), H(Metric), Rating, Datestamp)))

## Use cases / stories

## Questions

### How should we store this data locally? What are the space and bandwidth requirements?

- Hashes will likely be fixed-bitlength; sha1 is 160 bits. 
- Values could be a simple 8-bit byte; for metrics requiring only a boolean would use 0 for false/disagree and 255 for true/agree.
- Signatures vary but would likely require the same space as a hash plus perhaps another hash corresponding to a public key.
- Timestamps could be TAI64; 64 bits.
- Maps from hash values to URIs could live in any key-value store.

*Conclusion:* Let's use SQLite for the whole thing.

### How do we selectively distribute opinions?

I might not want to expose certain sets of opinions to certain people. How do we identify those we are sharing with, and how do we filter our opinion set for them?

While accepting that there's nothing short of DRM schemes that one could use to *force* trusted peers not to redistribute their opinion, what methods could one employ to *request* that they do not?

### How do we calculate "similar people"? How do we perform this calculation across the entire network and not just one's known peers?

Aggregation services? Query forwarding?

### How do we determine the "network's opinion"?

SVD is a simple linear algorithmic technique. One we have this corpus of opinion data, need to investigate clustering algorithms.

- Can we instead of distributing the entirety of the opinon corpus, distribute a pre-calculated "network opinion" rating to peers? In other words, distribute the computational load of the clustering algorithm across the network.

### How do we receive updated opinions?

Polling? Publish/subscribe among peers?

## Related Reading

[Attack Resistant Trust Metrics, Raph Levien](http://www.levien.com/thesis/compact.pdf)
- [Advogato's Trust Metric](http://advogato.org/trust-metric.html)
