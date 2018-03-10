# Opinions: A Distributed Network of Metrics

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

[The Political Compass]: http://www.politicalcompass.org
[Myers-Briggs Type Indicator]: http://en.wikipedia.org/wiki/Myers-Briggs_Type_Indicator_(MBTI) "Wikipedia: Myers-Briggs Type Indicator"
[Keirsey Temperament Sorter]: http://en.wikipedia.org/wiki/Keirsey_Temperament_Sorter "Wikipedia: Keirsey Temperament Sorter"
[ipfs]: http://ipfs.io/ "a global, versioned, peer-to-peer filesystem"
