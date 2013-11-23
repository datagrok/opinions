# A Distributed Network of Metrics

An *opinion* is a small data structure suitable for the capture and exchange of data like:

- Facebook-style "likes" for a topic, statement, business, or URL,
- Reddit-style "upvotes" on a news articles and other URLs,
- Netflix-style "star" ratings for movies,
- Amazon-style "star" ratings for books and other products,
- OKCupid-style [Likert-scale][likert] survey responses,
- Yelp-style restaurant reviews,
- Agreement with various political positions,
- PGP-style levels of trust in various cryptographic keys,
- etc.

The majority of development notes are captured in DESIGN.md in this directory.

This is a project toward the development of:

- first, an open specification for opinion exchange, and
- second, various reference implementations of software which conform to that specification.

The primary goal for this project will be acceptance of the specification as an [IETF Internet-Draft][] and its eventual publication as an [Internet RFC][].

## Versions

In January 2012 this project was forked (by me, as the sole author) at commit `7680ead` and improved in secrecy. I am presently inquiring about getting those improvements open-sourced and merged back into this repository. Until then, this project will move forward from the point it existed prior to January 2012.

## Licenses

All documentation herein is licensed under the terms of the [GNU Free Documentation License][FDL].

All source code for software herein is licensed under the terms of the [GNU Affero General Public License][AGPL].

[FDL]: https://www.gnu.org/copyleft/fdl.html
[AGPL]: https://www.gnu.org/licenses/agpl-3.0.html
[likert]: https://en.wikipedia.org/wiki/Likert_scale
[IETF Internet-Draft]: http://www.ietf.org/id-info/
[Internet RFC]: http://www.ietf.org/rfc.html
