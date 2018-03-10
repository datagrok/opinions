# Opinions: A Distributed Network of Metrics

## Introduction

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

[Article Feedback Tool]: http://en.wikipedia.org/wiki/Wikipedia:Article_Feedback_Tool "Wikipedia: Article Feedback Tool"
[Likert scale]: http://en.wikipedia.org/wiki/Likert_scale "Wikipedia: Likert scale"

[^pgp-trust]: OpenPGP defines a metric for trust in RFC4880 §5.2.3.13, but it is rarely exposed to users as it is difficult to understand, and can easily be conflated with various meanings of the phrase "trust a key." If A signs B's key and I trust that A is who they say they are, does that mean B is who they say they are?
