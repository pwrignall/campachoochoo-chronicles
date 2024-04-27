---
title: Using Datawrapper to visualise airline route maps
description: Plotting airport pairs as great circles on a map with Datawrapper.
date: 2024-04-27
tags:
  - dataviz
  - gis
  - datawrapper
  - maps
extraWideMedia: true
opengraph:
  image: /assets/images/qgis-routes/00-datawrapper.png
---

Continuing from my [previous post](./2024-02-07-route-maps-with-qgis.md), I wanted to try using a different tool to plot great circles on a map and was enticed by what an online tool would offer.

Datawrapper is a data visualisation tool that I've long admired. It has sensible,
[opinionated defaults](https://blog.datawrapper.de/dualaxis/), abundant chart types and features, and a
[superb companion blog](https://blog.datawrapper.de/).

To display quantitative data by geography, Datawrapper offers the ability to create choropleth and symbol maps. To show
maps with icons and labels &mdash; as I'd like to do with this airline route map &mdash; we can use its locator map type.
The result will be a responsive, static map.

To import line markers to Datawrapper, the format must be in GeoJSON. As the output of [the script](https://github.com/pwrignall/gclines/) I used to prepare data for QGIS was CSV, I altered it to also produce a pair of GeoJSON files: one containing
the route lines and one with airport points and labels (Datawrapper can also load points from a CSV but I added the
GeoJSON option anyway).

Since the steps to [add line markers](https://academy.datawrapper.de/article/176-how-to-import-area-line-markers)
and [add point markers](https://academy.datawrapper.de/article/179-how-to-import-point-markers) are so well documented
by Datawrapper, I'll skip through the process except to mention that the there is a warning about the tool slowing
down if you load lots of point markers (for example with the 24 in the example I've used here).

After some basic styling and tweaking label positions to try to minimise the amount of overlaps, here's the result
directly embedded from Datawrapper:

<div style="min-height:520px"><script type="text/javascript" defer src="https://datawrapper.dwcdn.net/ug91F/embed.js?v=7" charset="utf-8"></script><noscript><img src="https://datawrapper.dwcdn.net/ug91F/full.png" alt="A hypothetical airline route map to demonstrate how to make them with Datawrapper" /></noscript></div>

I think the result looks a little better on desktop than mobile but with some tweaking of the options and adding two
sets of point labels, one optimised for desktop and the other for mobile, we can get some very nice results with little
fuss because of how much effort Datawrapper has clearly spent on its users' experience.

Being able to quickly and easily make a responsive, annotated map like this is invaluable and an excellent addition to
any data visualisation toolkit.