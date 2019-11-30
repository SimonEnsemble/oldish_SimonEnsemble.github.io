---
layout: post
title: cookies and coffees
tags: [combinatorics]
comments: true
snippet: The "stars and bars" formulation is an intuitive way to solve a particular combinatorics problem. Here, for memorability, we formulate a combinatorics problem as cookies and coffees.
author: Cory Simon
--- 

**A dog :dog:, bear :bear:, and monkey :monkey: go out to eat together at a restaurant. For desert, the server generously brings five cookies (:cookie:'s). How many different ways can the five cookies be distributed among the three diners? The cookies are indistinguisable.**

For example, one outcome is:


:dog:: :cookie: :cookie:

:bear::

:monkey:: :cookie: :cookie: :cookie: 

***

*The answer to this combinatorics problem is readily apparent once cast into the [stars and bars](https://en.wikipedia.org/wiki/Stars_and_bars_(combinatorics)) framework. We use cookies and coffees to illustrate the idea more memorably.*

To easily visualize how the cookies might be distributed among the diners, the cunning :dog: arranges the five cookies along a line on the table

:cookie: :cookie: :cookie: :cookie: :cookie:

then, to represent the example distribution of cookies above, uses coffee (:coffee:) at the table to partition the cookies into bins:

:cookie: :cookie: :coffee: :coffee: :cookie: :cookie: :cookie:

The cookies:
* to the left of the left-most :coffee: belong to :dog:
* between the two :coffee:'s belong to :bear: (none in this instance)
* to the right of the right-most :coffee: belong to :monkey:

We only need two coffees to denote the partition of cookies among the diners-- one less than the number of diners. 

That the order of partitions go from :dog:, :bear:, to :monkey: (left to right) is arbitrary and, for counting the possible distributions, immaterial.

An important concept here is that we can denote any distribution of the cookies among the diners by shuffling the cookies and coffees.

Thus, in this cookies and coffees representation, it is apparent that the number of ways to distribute the cookies among the diners is equal to the number of ways to arrange the cookies and coffees, where the cookies and coffees are treated as indistinguishable. 

Thus, the number of possible allocations of cookies to the diners is:

$$\dfrac{(5+2)!}{5!2!}$$

The $(5+2)!$ is the number of ways to permute the cookies and coffees if they were distinguishable; the $5!$ and $2!$ in the denominator account for the indistinguishability of the cookies and coffees, respectively.

In general, the number of ways to distribute $c$ cookies to $d$ diners is:

$$\dfrac{(c + d-1)!}{c! (d-1)!}$$

because we need $d-1$ coffees to indicate the partitioning of the row of cookies.
