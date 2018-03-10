# Opinions: A Distributed Network of Metrics

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

[^pgp-redundant-avatar]: Depending on the signature scheme we choose, the signature key ID may be equivalent to `H(Avatar)`, and may be already included in the notion of "signing" some data. For example, OpenPGP RPC4880 §5.2.3.5 defines an "Issuer" signature subpacket which is the eight-octet OpenPGP key ID. If present, we need not encode it twice. However: it is entirely valid to sign an opinion structure at a different time than it applies to, so it is entirely valid to include our timestamp even though OpenPGP defines in §5.2.3.4 a "Signature Creation Time" mandatory signature subpacket.
