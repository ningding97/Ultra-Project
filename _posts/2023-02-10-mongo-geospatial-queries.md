---
layout: epic
title: "Are you using the right Mongo geospatial query?"
date: 2023-02-10
categories: [MongoDB, geospatial]
author: [orta]
---

We recently got a report from one of our galleries in the Los Angeles area that
they weren't showing up on our
[Los Angeles exhibition listings](https://www.artsy.net/shows/los-angeles-ca-usa).

make the cut.

_Case closed_. Or so I thought.

<!-- more -->

After some back and forth with our partner I decided to investigate more
thoroughly, this time using some tricks of the trade from
[my other life](https://www.anandarooproy.com) outside of Artsy.

## Casting a wider net

If there was something wrong with our 25km radius query, I wanted to start by
casting a wider net and visualizing the results.

<!-- ```js
// a $geoWithin $center query

db.events.find({
  coordinates: {
    $geoWithin: {
      $center: [[-118.24, 34.05], 25 / 111.32],
    },
  },
})
``` -->


I modified the above query to cast a 50km net in order to see if there were some
edges cases that needed scrutiny. Taking the resulting JSON response, I fired up
[Placemark](https://www.placemark.io/), my favorite new tool for wrangling
geospatial data.

(Incidentally I recommend reading Tom Macwright's
[recent reflection on creating Placemark](https://macwright.com/2023/01/28/placemark.html)
as a bootstrapped indie developer.)

<figure class="illustration">
  <img
    src="{{ site.baseurl }}/images/2023-02-10-mongo-geospatial-queries/1.png"
    alt="Screenshot of a visualization in Placemark showing Los Angeles area exhibitions within a 50km radius."
  />
  <figcaption>All shows within a 50km radius</figcaption>
</figure>


## More than one way to draw a circle on the Earth

It was at this point that I recalled the specific form of the geospatial query
our code was performing, and consulted the
[MongoDB docs for the $geoWithin query](https://www.mongodb.com/docs/manual/reference/operator/query/geoWithin/).

Turns out that you can invoke this as a radius query in one of two ways, by
specifying
[$center](https://www.mongodb.com/docs/manual/reference/operator/query/center/)
or
[$centerSphere](https://www.mongodb.com/docs/manual/reference/operator/query/centerSphere/).

Per the
[docs](https://www.mongodb.com/docs/manual/reference/operator/query/center/#behavior)
for `$center`, this query…

> calculates distances using flat (planar) geometry

Let us pause for a moment to note that while only some maps are
[deceitful](https://press.uchicago.edu/ucp/books/book/chicago/H/bo27400568.html),
_all_ maps are untruths. In the sense that they flatten three dimensions down to
two, and inevitably distort the world in the process.


## A postscript on map distortion
This corresponds to what you get when you use MongoDB's `$geoWithin` `$center`
query on geospatial data:

<!-- <figure class="illustration">
  <img
    src="{{ site.baseurl }}/images/2023-02-10-mongo-geospatial-queries/7.png"
    alt="Tissot's indicatrix for equirectangular projection"
  />
  <figcaption>Tissot's indicatrix for equirectangular projection. Credit: Justin Kunimune, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons</figcaption>
</figure>
 -->

But we hope that understanding this crucial distinction between planar
(`$center`) and spherical (`$centerSphere`) calculations will help you make the
right choice when devising your own radius queries with MongoDB or other
geospatial engines.
