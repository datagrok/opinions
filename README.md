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
2. (a) software reference implementation(s) which conform to that specification

We aim to create an open specification suitable for acceptance as an [IETF Internet-Draft][] and eventual publication as an [Internet RFC][].

## Versions

The earliest timestamp I can find in my notes for this idea is September 12, 2011.
This repository was created September 17, 2011.

I worked briefly on a private fork of the project in January 2012 as part of a former employer's self-directed research program, based on commit `7680ead`.
I was not granted permission to open-source it, and I later changed employers.
To avoid any legal risk, I have discarded my copy of that fork.

## Licenses

All documentation herein is licensed under the terms of the [GNU Free Documentation License][FDL].

All source code for software herein is licensed under the terms of the [GNU Affero General Public License][AGPL].

[FDL]: https://www.gnu.org/copyleft/fdl.html
[AGPL]: https://www.gnu.org/licenses/agpl-3.0.html
[likert]: https://en.wikipedia.org/wiki/Likert_scale
[IETF Internet-Draft]: http://www.ietf.org/id-info/
[Internet RFC]: http://www.ietf.org/rfc.html
[DESIGN.md]: DESIGN.md
