# Opinions: A Distributed Network of Metrics

- [Abstract](#abstract)
- [Introduction](0_introduction.md)
- [Problems](1_problems.md)
- [Proposal](2_solution.md)
- [Use Cases / User Stories](3_usecases.md)
- [Questions](4_questions.md)
- [References / Related Reading](5_references.md)

- [To-Do](todo.md)
- Specification \[in progress\]

## Abstract

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

The data is captured in such a way that it is difficult to forge, easy for end-users to extend to suit their own purposes, and may be easily processed by recommendation engines.

This is a project toward the development of:

1. an open specification for Opinions and their exchange, and
2. a software reference implementation which conforms to that specification

I aim to create an open specification suitable for acceptance as an [IETF Internet-Draft][] and eventual publication as an [Internet RFC][].

## Versions

This repository was created 2011-09-17.
The earliest timestamp I can find in my notes for this idea is 2011-09-12.

I worked briefly in January 2012 on a private fork of this project as part of an employer's self-directed research program.
I am no longer their employee. To avoid legal issues, no part of that fork is incorporated in this repository.

## Licenses

All documentation herein is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa-4.0i].

All source code for software herein is licensed under the terms of the [GNU Affero General Public License][AGPL], version 3 or later.

[FDL]: https://www.gnu.org/copyleft/fdl.html
[AGPL]: https://www.gnu.org/licenses/agpl-3.0.html
[likert]: https://en.wikipedia.org/wiki/Likert_scale
[IETF Internet-Draft]: http://www.ietf.org/id-info/
[Internet RFC]: http://www.ietf.org/rfc.html
[cc-by-sa-4.0i]: http://creativecommons.org/licenses/by-sa/4.0
