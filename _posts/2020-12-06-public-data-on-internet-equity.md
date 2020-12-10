---
layout: post
author: Jamie Saxon
title:  "Publicly-Available Data on Internet Access and Equity"
date:   2020-12-06
categories: broadband equity public data ACS CPS neighborhoods
header-includes:
  - \hypersetup{colorlinks,linkcolor=[rgb]{0,0.3,0.8},urlcolor=[rgb]{0,0.3,0.8}}
---

Modern society is mediated through the Internet.
Equity in society requires equity in usable broadband Internet access:
  for communication, education, health, and quality of life or entertainment.
This was true before the coronavirus pandemic and it will be true when the pandemic subsides.
But the need today is more acute than it has ever been.

This is a multifaceted problem:
  the connections available to people and their bandwidth,
  the devices that they use on those connections, and 
  their digital "literacy" in leveraging those resources.
Understanding and responding to digital literacy requires good data
  from a variety of angles.
In a series of three posts, I want to lay out 

1. existing public data sources on internet access and equity (this post),
2. simple technical methods for [measuring performance at home][post2], and 
3. some of the more-elaborate strategies required for [measuring consumption][post3] and application performance.

But aside from the fact that it's kind of _fun_ to fuss with your router,
  why should we pursue those dierect measurements at all?
After all, hardware is expensive, deploying it is difficult, and data is free:
  at first blush, there are a lot of data sources out there.
Those data are the subject of this post.

To make a long story short:
  the available data use very rough metrics of "broadband" (presence of a 25/3 Mbps connection), 
  or they have significant sampling limitations.
Notwithstanding, they are very valuable, so I'll review some of them here.
The map below draws on several of these data sources,
  to present neighborhood-level Internet access in the twenty largest American cities.

<figure>
<embed src="https://saxon.cdac.uchicago.edu/broadband/map.html" style="padding: 20px 0 0 0; width:100%; height: 85vh;">
<figcaption>
Base map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, 
<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>; layers by <a href="http://carto.com/">CARTO</a>.  
Map data from US Census
<a href="https://www.census.gov/programs-surveys/acs/about.html">American Community Survey</a>, 
<a href="https://opendata.fcc.gov/Wireline/Fixed-Broadband-Deployment-Data-Jun-2019-Status-V1/sgz3-kiqt">FCC Form 477</a>, and 
<a href="https://registry.opendata.aws/speedtest-global-performance/">Speedtest by Ookla</a>.
Map by <a href="https://saxon.cdac.uchicago.edu">James Saxon</a>, 
<a href="http://cdac.uchicago.edu">Center for Data and Computing</a>,
<a href="http://www.uchicago.edu">University of Chicago</a>.
</figcaption>
</figure>

This project on broadband Internet 
  continues my study of resource equity in urban neighborhoods,
  but aligns it with the "classical strengths" of my current research group.
I moved over the summer from the 
  [Harris School of Public Policy][harris] and the 
  [Center for Spatial Data Science][csds] to the 
  [Center for Data and Computing][cdac] and the 
  [NOISE Lab][noise], directed by 
  [Nick Feamster][feamster].


#### FCC Form 477.
Perhaps most prominent is the venerable 
  [FCC 477 data][fcc-data]: 
  ISPs' self-reports of offered services, as shown in this 
  [flashy map][fcc-map].
From these data[^1], it is possible to calculate
  broadband availability and indeed
  the number of ISPs providing service at any threshold. 
The problems with the data are well-documented.
The reports are usually dated and they tend to over-represent real options.
A Census block counts as having a service if it is available to
  just _one address_ in that block.
Even within this generous definition, 
  Major, Teixera, and Mayer 
  [recently showed ][no-wans-land]
  that the 477 reports often overstate what the ISPs' own subscription sites
  will actually allow you to order.
It is also notable that the 477 reports capture _availability_ rather than _subscriptions_, 
  meaning that it doesn't represent what people actually experience or use on the ground.
Within the literature on health access, it has long been established
  that access is more meaningfully measured via _consumption_ or _use_
  than pro forma availability; the same holds for Internet.
Finally, there is often a difference between 
  what's available and what's physically installed.
Comcast lists huge swaths of Chicago as having 985/35 Mbps connections available,
  but bringing the fiber to the home may _still_ involve a fleet of trucks
  (and adeptly-applied pressure).


#### Survey data.
You can also just _ask people_ if they have Internet.
The US Census does that in two surveys: 
  the [American Community Survey][acs-site] (ACS, the successor to the Census long form), 
  and the National Telecommunications and Information Administration's (NTIA) supplement to the Current Population Survey (CPS).
The ACS captures [device][acs-comp] ownership and 
  [broadband][acs-broadband] subscriptions in households.
The sample size is about 1% of the US population annually.
"Broadband" is understood dichotomously as the presence or absence of a 25/3 Mbps connection.
The ACS API provides estimates down to the Census Tract level,
  and using the [IPUMS microdata][ipums] it is possible to construct measures 
  of the "digital divide" -- by gender, income, race, geography, etc.[^2]

The [NTIA data][ntia-data] ([docs][ntia-docs], [code][ntia-code])
  include both the subscriptions and device ownership of the ACS,
  along with additional data on which services people use and where they do so.
A state-level map and time trends by Rafi Goldberg at the NTIA is [here][ntia-map].
On the other hand, the NTIA supplement is less frequent and has a much-smaller sample
  (as well as a slightly different sample frame).
The CPS is stratified at the state level, and I have found that finer-grained measurements
  are simply not credible within uncertainties.
For example, I find it hard to believe that among the twenty largest US cities,
  San Francisco is the [worst-provisioned][ntia-sf-gist] in Internet.

#### Distributed, web-based measurements.
[Ookla][ookla] ([speedtest][speedtest]) and 
  [M-Labs][mlab] ([speed test][mlab-speedtest]) 
  both perform distributed measurements of network latency, jitter, and throughput
  via the browser and dedicated apps.
The M-Lab data are readily available on [BigQuery][mlab-bigquery] 
  and have been influential for policy.
It is the hard-coded first link in a Google search for "speed test."
But the data have somewhat important limitations, 
  as [recently described][feamster-cacm] by Nick Feamster (my adviser at CDAC) and Jason Livingood (Comcast)
  in the CACM.[^3]
The server infrastructure is [not widely distributed][mlab-servers], and it has not proven entirely reliable.

There are also some low-level technical issues.
In some instances, the 10-second test may not suffice for a TCP link to saturate.
More to the point, the `ndt` protocol has not included multiple TCP threads
  until [quite recently][mlab-ndt7].
Now, `ndt7` uses [BBR][wiki-bbr] congestion control when it's available, 
  and this purports to do better.
But most of the data measure the performance of a single TCP thread, 
  which saturates far below the bandwidth of most modern access links.
The other parameters of the M-Labs tests (latency, jitter, etc.) 
  might better approximate the intended concepts, were the servers not so sparse.

By contrast, the Ookla/speedtest measurements are technically on surer ground,
  and the infrastructure is rather more robust.
Still, that sample isn't entirely unbiased either.
This can be seen by contrasting
  Ookla device counts with Census households with Internet subscriptions.
People who choose to run speedtests are not representative of the broader population, 
  and they generally don't run them at unbiased (random or regular) times.
Just as important,
  the "last-mile" bandwidth may not be an sufficient indicator of network performance
  (or indeed even of the ISP's link, if a test is performed from a device connected over a bad Wifi connection).
Speedtest chooses the test server with the lowest round-trip latency (roughly, the closest),
  but while major services have widely distributed servers 
  (c.f. [Google's][google-cdn] and [Netflix's][netflix-cdn] peering "offers")
  and others rely on CDNs, resources are not always close at hand.

#### SamKnows Whiteboxes.
Data collected by SamKnows for the FCC are in some ways a gold standard.
This program has been running since 2011
  and includes a [bevy of continuous, open-sourced tests][samknows-tests] on
  dedicated hardware that they call "whiteboxes":
  TCP (both single and multithreaded) and UDP bandwidth,
  latencies (ping, under load, and DNS), web browsing and loads, 
  video quality, even consumption, and so on.
The [data][samknows-data] are used for the annual 
  Measuring Broadband America reports.
But it too has some issues of _who_ is captured.
The sampling strategy aims to ensure that ISPs, rather than demographic groups, are adequately sampled.
So like Ookla devices, SamKnows households tend to skew wealthier than the general population.
In addition, there are just a few thousand households in the sample nation-wide,
  which is not enough to say how _certain groups_ fare within Chicago, for instance.
(There are no demographics and limited information about units' geography.[^4])

The European Network Coordination Centre, RIPE, 
  also operates a network of probes and anchors worldwide,
  measuring [latency and network topology][ripe-metrics]: ping, DNS, traceroutes, etc.
The RIPE probes used to be hardware devices attached via Ethernet to a router, 
  but RIPE has extended this to a [software package][ripe-sw] that "anyone" can run.

#### Citizen science and server logs.
There are of course many, many more strategies and datasets.
Telecom companies spend a fortune "drivetesting" their networks,
  rolling around to map access and gaps.
There's plenty of room for a citizen science, on the same thing.
As you go about your neighborhood and routines,
  your phone registers available access points.
The "game" of recording those systematically and competitively 
  is a popular activity for hackers,[^5]
  known as "wardriving" (from wardialing, from [WarGames][wargames]).
The [WiGLE][wigle] wardriving platform makes its data available, 
  and there are other institutionalized efforts such as [Mozilla Location Services][mls]
  (they call it "stumbling," but it's functionally equivalent).

I've dabbled with these methods -- both with apps and jury-rigging raspberry pis.
As you can see [here][wigle-chicago], it was pretty quick to cover my own 
  neighborhood in Chicago.
But the issues of coverage are not trivial.
Which networks can be seen from the street depends 
  on building materials, heights, interference, etc.
There are also sampling problems (who runs these tests), 
  but one can imagine a measurement of access point density "conditional on an observation."
At the University of Chicago, 
  [Monisha Ghosh][monisha]'s group has developed apps
  for finer-grained maps of mobile broadband spectrum and deployment.
I see tremendous potential in deploying these apps through a fleet (city vehicles or taxis[^6])
  or a public mapping program like [MAPSCorps][mapscorps].

And there's a ton more to be uncovered, waiting in server logs.
To my mind, Wikipedia logs would be
  the very best data for measuring the "homework gap":
  it is a high-traffic site
  hosting lightweight pages (no _fancy_ connection is required),
  of plausibly educationally-oriented materials.[^7]
As you might expect (or hope!) Wikipedia guards user privacy in their logs quite strictly,
  and there remains a challenge of IP geolocation, to attribute use and access to demography.
(This is a problem I'm currently working on.)
Still, Microsoft has used similar data -- page load times -- to 
  [derive estimates][microsoft-find] of who _really_ has broadband access, 
  at the county level ([data][microsoft-data]).
Their work suggests that real access is about half of what the FCC reports would suggest.
This is fantastic work, but it would be nice 
  to measure the variation at finer spatial granularity within cities.

That is what we're setting out to do.


#### Notes.
[^1]: The FCC data include, among other fields, advertised and contractual up and downstream bandwidths, per supplier (DBA, holding company, etc.), and whether service is provided to commercial or consumer properties.  In practice, I found that the contractual numbers -- naively more apposite -- are often basically missing, and so the advertised levels are to be preferred.
[^2]: Albeit at coarser geographies, called Public Use Microsample Areas (PUMAs).  These are regions defined to preserve the anonymity of microdata, and contain 100-200k people.
[^3]: Ookla lays out [similar criticisms][ookla-critique] in a recent piece.
[^4]: I am still debugging exactly what geographies are available, as many Census block IDs appear to be ill-formed.
[^5]: More likely, niche, though it depends on your peer group.
[^6]: A project by the Senseable City Lab at MIT [estimates][scl-taxis] the coverage of taxis as an opportunistic sensor deployment.
[^7]: It is perhaps interesting to note that for domain-level sites (like Wikipedia), either the sites' own logs or any high-level DNS resolver would do.  [Moura et al][dns-congestion] showed at IMC '20 that in the Netherlands, the concentration of DNS resolution has grown quite extreme.  We might expect it to be even more so in the US / .com TLD.  What this means is that Google and Amazon know if you're using Wikipedia, even if you don't search, because they are likely the ones who are telling you _how to get to Wikipedia_.  

[post2]:          {% post_url 2020-12-07-basic-measurements-of-access-networks %}
[post3]:          {% post_url 2020-12-09-measuring-internet-traffic %}

[harris]:         https://harris.uchicago.edu/
[csds]:           https://spatial.uchicago.edu/
[cdac]:           https://cdac.uchicago.edu/
[uchicago]:       https://www.uchicago.edu/
[noise]:          https://noise.cs.uchicago.edu/
[feamster]:       http://people.cs.uchicago.edu/~feamster/
[fcc-map]:        https://broadbandmap.fcc.gov/
[fcc-data]:       https://opendata.fcc.gov/Wireline/Fixed-Broadband-Deployment-Data-Jun-2019-Status-V1/sgz3-kiqt
[no-wans-land]:   https://dl.acm.org/doi/10.1145/3419394.3423652
[ipums]:          https://usa.ipums.org/usa/index.shtml
[ntia-map]:       https://www.ntia.doc.gov/data/digital-nation-data-explorer#sel=homeInternetUser&disp=map
[ntia-data]:      https://www.ntia.doc.gov/page/download-digital-nation-datasets
[ntia-docs]:      https://www.ntia.doc.gov/files/ntia/publications/november-2019-techdocs.pdf
[ntia-code]:      https://www.ntia.doc.gov/files/ntia/data_central_downloads/code/create-ntia-tables-stata.zip
[ntia-sf-gist]:   https://gist.github.com/JamesSaxon/644f79a75e99aa11dc3282f269d03cec
[ripe-sw]:        https://labs.ripe.net/Members/alun_davies/ripe-atlas-software-probes
[ripe-metrics]:   https://beta-docs.atlas.ripe.net/built-in/
[feamster-cacm]:  https://cacm.acm.org/magazines/2020/12/248801-measuring-internet-speed/fulltext
[ookla-critique]: https://www.speedtest.net/insights/blog/better-funding-decisions-accurate-broadband-network-data/
[acs-site]:       https://www.census.gov/data/developers/data-sets/acs-1year.html
[acs-comp]:       https://api.census.gov/data/2019/acs/acs1/profile/variables/DP02_0151PE.json
[acs-broadband]:  https://api.census.gov/data/2019/acs/acs1/profile/variables/DP02_0152PE.json
[monisha]:        https://people.cs.uchicago.edu/~monisha/
[ookla]:          https://www.ookla.com/ookla-for-good
[speedtest]:      https://www.speedtest.net/
[speedtest-aws]:  https://github.com/teamookla/ookla-open-data
[mlab]:           https://www.measurementlab.net/
[wiki-bbr]:       https://en.wikipedia.org/wiki/TCP_congestion_control#TCP_BBR
[mlab-ndt7]:      https://www.measurementlab.net/blog/speed-tests-accuracy/
[mlab-servers]:   https://www.measurementlab.net/status/#infrastructure-map
[mlab-bigquery]:  https://console.cloud.google.com/bigquery?project=measurement-lab&pli=1
[mlab-speedtest]: https://speed.measurementlab.net/#/
[microsoft-data]: https://github.com/microsoft/USBroadbandUsagePercentages
[microsoft-find]: https://blogs.microsoft.com/on-the-issues/2019/04/08/its-time-for-a-new-approach-for-mapping-broadband-data-to-better-serve-americans/
[google-cdn]:     https://peering.google.com/#/options/peering
[netflix-cdn]:    https://openconnect.netflix.com/en/appliances/
[wigle]:          https://wigle.net/
[wigle-chicago]:  https://wigle.net/map?maplat=41.78706665761995&maplon=-87.59326829071036&mapzoom=13&coloring=density
[mls]:            https://location.services.mozilla.com/map#2/35.0/9.0
[samknows-tests]: https://samknows.com/technology/qos-tests
[samknows-data]:  https://www.fcc.gov/oet/mba/raw-data-releases
[dns-congestion]: https://dl.acm.org/doi/abs/10.1145/3419394.3423625
[mapscorps]:      https://mapscorps.org/
[scl-taxis]:      http://senseable.mit.edu/urban-sensing/
[osm]:            https://www.openstreetmap.org/
[cc-by-sa]:       https://creativecommons.org/licenses/by-sa/2.0/
[carto]:          http://carto.com/
[me]:             https://saxon.cdac.uchicago.edu/
[wargames]:       https://en.wikipedia.org/wiki/WarGames



