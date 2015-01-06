# A Distributed Network of Metrics

An *opinion* is a small data structure suitable for the capture and decentralized exchange of data like:

- Facebook-style "likes" for a topic, statement, business, or URL,
- Reddit-style "upvotes" on a news articles and other URLs,
- Netflix-style "star" ratings for movies,
- Amazon-style "star" ratings for books and other products,
- OKCupid-style [Likert-scale][likert] survey responses,
- Yelp-style restaurant reviews,
- Agreement with various political positions,
- PGP-style levels of trust in various cryptographic keys,
- etc.

We capture this data in such a way that it is difficult to forge, easy for end-users to extend to suit their own purposes, and may be easily processed by recommendation engines.

The majority of development notes are captured in [DESIGN.md][] in this directory.

This is a project toward the development of:

1. an open specification for opinion exchange, and
2. various reference implementations of software which conform to that specification and demonstrate how to use and manage opinion data.

The author aims to create a specification suitable for acceptance as an [IETF Internet-Draft][] and eventual publication as an [Internet RFC][].

## Versions

The earliest timestamp I can find in my notes for this idea is September 12, 2011. This repository was created September 17, 2011.

In January 2012 this project was forked (by me, the sole author) at commit `7680ead` and improved in secrecy as part of my employer's self-directed research program, similar to [Google's famous "twenty percent time."][google-20-time] I expected that the result of that work could be easily merged back into this repository, but I was asked to wait to do so.

I have been waiting for more than three years now for merge approval. Until I receive it, to avoid any potential for legal issues this project will move forward from the point it existed prior to January 2012. I expect that I will end up re-implementing in my own time all of the research I did once already.

## Licenses

All documentation herein is licensed under the terms of the [GNU Free Documentation License][FDL].

All source code for software herein is licensed under the terms of the [GNU Affero General Public License][AGPL].

[FDL]: https://www.gnu.org/copyleft/fdl.html
[AGPL]: https://www.gnu.org/licenses/agpl-3.0.html
[likert]: https://en.wikipedia.org/wiki/Likert_scale
[IETF Internet-Draft]: http://www.ietf.org/id-info/
[Internet RFC]: http://www.ietf.org/rfc.html
[DESIGN.md]: DESIGN.md
[google-20-time]: http://www.nytimes.com/2007/10/21/jobs/21pre.html
