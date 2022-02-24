---
layout: post
author: Jamie Saxon
title:  An Integrated Map of Internet Access
date:   2022-02-21
categories: broadband performance
---

A little over a year ago, 
  I created an [interactive map][jamie-map-1] of 
  indicators of Internet access,
  in the twenty largest American cities.
In the time since,
  a number of additional maps have come out,
  supplementing the FCCs' maps.
Although many are nice,
  I felt that I could make a modestly cleaner one.

* [FCC 477][fcc-map]: is limited to the 477 data itself (of course!), and is not so easy to navigate.  The FCC also has an [LTE map][lte-map] that is pretty nice.
* [Illinois Broadband][il-map]: is limited to Illinois!
* [NTIA Broadband Map][ntia-map]: has a limited number of variables. Cartographically, it is very hard to read, since all layers can be overlaid, and all are the same color.

The other big thing missing from these maps is that the data are not approachable.
The raw FCC data are around 10 GB, which is a lot for most people,
  and filtering them usefully is a pain.
The other maps didn't have a simple, tract level download,
  to get people started.
I wanted to provide that.
You can also find the code that generates those data in this [repo][repo].

The map should also still include what I view as the three most-important
available data sources on broadband:

1. The Census's indicators of both broadband and computer access, and other demogrpahics.
2. The FCC's 477 reports, aggregated in useful ways: fiber availability, number of ISPs, etc.
3. Ookla's Speedtest performance data.  These need to be reaggregated from quadtiles to Census tracts.[^1] 

Here is my map:

<embed src="https://saxon.cdac.uchicago.edu/~jsaxon/us_map/" style="padding: 20px 0 0 0; width:100%; height: 80vh;">

<br>

Naturally, it is made using Mapbox,
  because that is where I will work
  by the time you read this post.
Therefore, it has beautiful vector layers and it is responsive.
If you click to go full screen, hovering will also show you the values of observations.

The data include a lot of useful broadband variables, all packed up and aligned.
In just 4.2 MB, you get every Census tract in the US.
From the Census:
* `population`, `households`: tract population and number of households.
* `geoid`: Census tract GEOID.
* `f_broadband`: fraction of households with broadband access.
* `f_computer`: fraction of households with a computer.
* `f_ba`: fraction of the adult population that holds a bachelor's degree.
* `f_black`: fraction of the population that is Black.
* `f_hispanic`: fraction of the population that is Hispanic.
* `mhi`, `log_mhi`: tract median household income, and its logarithm.

From the FCC:
* `n_isp`: the number of Consumer ISPs.
* `n_dn10`, `n_dn100`, `n_dn250`: the number of ISPs at 10, 100, and 250 Mbps.
* `n_fiber_100u`: the number of fiber ISPs with at least 100 Mbps in the upstream direction.
* `fiber_100u_exists`: the fraction of blocks in the tract where the FCC believes fiber is available.
* `max_dn`, `max_up`: the maximum download and upload speeds, reported to the FCC.

From Ookla:
* `d_mbps`, `u_mbps`: average download and upload speeds observed by Ookla on fixed-line connections (reaggregated from quaditles).
* `lat_ms`: average latency observed on Ookla Speedtests.
* `tests`, `devices`: number of Ookla Speedtest measurements, and number of unique devices.
* `tests_per_cap`, `devices_per_cap`: the number of tests or devices, divided by the tract population.

Happy analyzing!


[^1]: I did this by apportioning tests based on the areal intersection between quaditles to tracts.


[jamie-map-1]: https://jamessaxon.github.io/broadband/equity/data/2020/12/06/public-data-on-internet-equity.html
[ntia-map]:    https://broadbandusa.maps.arcgis.com/apps/webappviewer/index.html?id=e2b4907376b548f892672ef6afbc0da5
[il-map]:      https://gis.connectednation.org/portal/apps/webappviewer/index.html?id=caedfe7ce8924660a4ce62de6a75a7fd
[fcc-map]:     http://broadbandmap.fcc.gov/
[lte-map]:     https://fcc.maps.arcgis.com/apps/webappviewer/index.html?id=6c1b2e73d9d749cdb7bc88a0d1bdd25b
[repo]:        https://github.com/JamesSaxon/internet-access-map


