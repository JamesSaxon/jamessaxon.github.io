---
layout: post
author: Jamie Saxon
title:  Internet Access and Internet Equity
date:   2022-02-21
categories: broadband performance
datatable: true
---

With support from [data.org][data-org],
  we are working to better measure the performance of Internet in American cities.
Along with our technical work to develop measurement infrastructures,
  we want to highlight already-available public data
  make it comprehensible and help others to use it.

In this post, I present a "scorecard" of
  Internet access and equity, for the twenty largest metro regions in the United States.
By **access**, I mean the share of people have broadband Internet.
By **equity**, I mean the difference in access between various groups.
This analysis uses data from US Census Bureau's American Community Survey
  and the National Telecommunication and Information Administration (NTIA)
    Internet performance supplement to the Current Population Survey.
Still, this post is more about data than policy.
I am providing code, to invite others into the discussion.

I rank metro regions by:

1. share of households that are connected
2. differences in connection rates by race, ethnicity, and gender
3. same as (2), but controlling for income and age levels.

If you just want the answer, this is it:

| Metro             | Access (1) | ΔRace (2) | ΔₖRace (3) | ΔEthn. (2) | ΔₖEthn. (3) | ΔSex (2) | ΔₖSex (3) |
|:------------------|:----------:|:---------:|:----------:|:----------:|:-----------:|:--------:|:---------:|
| New York          |       87.8 |       7.4 |        3.2 |        6.0 |         3.3 |      1.4 |       0.4 |
| Los Angeles       |       89.3 |       4.1 |        0.4 |        9.6 |         5.8 |      0.8 |       0.6 |
| Chicago           |       87.7 |      11.7 |        5.6 |        8.1 |         5.3 |      0.8 |       0.6 |
| Dallas            |       89.5 |       6.3 |        0.4 |       13.4 |         8.2 |      0.6 |       0.1 |
| Houston           |       89.1 |       2.5 |       -1.8 |       12.1 |         5.9 |     -0.1 |      -0.4 |
| Washington        |       92.3 |       5.4 |        1.1 |        8.6 |         3.0 |      0.3 |      -0.5 |
| Miami             |       84.7 |       9.4 |        5.4 |        2.8 |         2.1 |      1.0 |       0.9 |
| Philadelphia      |       89.0 |      12.0 |        4.7 |        8.2 |         3.0 |      1.6 |       0.4 |
| Atlanta           |       89.6 |       6.8 |        2.5 |        9.7 |        -0.2 |      0.6 |       0.6 |
| Phoenix           |       89.0 |       4.7 |        5.6 |       15.1 |         9.6 |      0.2 |       1.3 |
| Boston            |       90.4 |       8.8 |        3.2 |       12.6 |         7.5 |      0.6 |       0.4 |
| San Francisco     |       91.8 |      10.3 |        3.2 |       10.2 |         5.5 |      1.6 |       0.5 |
| Riverside         |       89.5 |       3.9 |        1.4 |        2.5 |         1.9 |      1.7 |       0.5 |
| Detroit           |       87.4 |      16.2 |        6.9 |        3.1 |         2.6 |      2.1 |       0.9 |
| Seattle           |       93.0 |       4.5 |        1.2 |        7.1 |         7.3 |      1.4 |       1.0 |
| Minneapolis       |       90.1 |       8.7 |       -2.0 |        7.4 |         0.1 |      1.2 |      -2.5 |
| San Diego         |       92.7 |       1.9 |        0.9 |        6.1 |         4.4 |      0.7 |       0.0 |
| Tampa             |       89.0 |       9.4 |        6.3 |        5.2 |         6.4 |      1.8 |       1.5 |
| Denver            |       92.5 |       4.0 |        0.7 |       12.9 |         9.2 |     -0.0 |       0.9 |
| St Louis          |       87.8 |      12.6 |        2.4 |       -0.9 |         2.0 |      0.1 |      -1.3 |

<br>

Most of this will focus on the ACS data.
At the end, I will delve
  into the consistency of the ACS with the CPS.

### Access

The first question is: in which cities are broadband subscription rates highest?
With a single [API call][acs-api-call-1], 
  you can get the populations and broadband subscription rates 
  for every Census "place" or Metro area in the US, from the American Community Survey.
That is super easy, and we can retrieve analogous estimates for any [geography][acs-geog-5] in the US:
  states, counties, Census tracts, congressional districts, &c.[^1]
Using these calls, we see that for either cities metro regions, Seattle tops out the list.
The list for metro regions is above.

#### A theoretical issue: how to define the city?

But we already have an important subtlety to address:
  how to define cities?
Census places or "cities" are jurisdictions.
Metro regions are a functional description of how we actually live.
Our choice of geography to examine will affect the answers we get.
The share of households without broadband
  is modestly higher in cities than metro regions:
  14.9% of households in New York City don't have home broadband, 
    but that number falls to 12.2% in the metro region.
Because poverty is usually higher in cities,
  this effect appears in most regions.
For instance Philly dips from 15.8% to 11.0%
  and the numbers in Chicago fall from 16.4% to 12.3%.
  
Cities are boundaries by which a
  "we" both living and dead has agreed to divvy up, or segregate, America:
  what representation and tax burdens we accept or refuse to share.
If we want to "blame" someone for poor performance,
  we can use "Census place" numbers to point fingers at the mayor's office.
But doing this, while partially appropriate in a democracy,
  would _understate_ the digital divide:
    it is often starker than the place-based numbers indicate.
The people who can can afford to live outside the city limits and choose to do so
  are more likely to be online.

In this post, I'll focus estimates for metro regions.
But the reason is as much technical and practical
  as conceptual (the notion of responsibility, above).
The technical reason is that the Census does not
  release "microdata" (individual responses)
  for individual cities, but it _does_ release them
  for metro areas.
We have no choice about the geography.

### Equity

#### Absolute differences between groups.

Our next question is: who gets access, and who doesn't?
Although the Census API does break down broadband estimates
  by age, income, and educational attainment (in very broad categories)
  it does not do this for race, ethnicity, or gender.
These are the categories we'll examine here.

Doing so requires us to use _Public Use Microdata Samples_ (PUMS)
  from the Census.
PUMS are the individual responses from that people gave to the 
  American Community Survey.
They are also now accessible through an API, 
  but while that API does include race as a [queryable variable][acs-pums-vars],
  and can tabulate results for us,
  it does not include any geography finer than state.
(Example tabulation for [Illinois][acs-pums-call-1].)

We can download the full data from
  Integrated Public Use Microdata Samples ([IPUMS][ipums])
  service at the University of Minnesota.
Accounts and data are free
  but the license requires you to download your own data extract.
To make progress, we'll also have to write some code:
  this is wrapped up in this [repo][repo].

The second set of columns above thus show the difference 
  in the shares of people with broadband access, in each Metro region,
  between three groups:

1. Black and non-Black respondents
2. Hispanic and non-Hispanic respondents, and 
3. Women and men

In the table above, these differences are shown 
  in the columns ΔRace, ΔEthn., ΔSex.

The numbers quoted are, simply, the differences in the (sample-weighted) probabilities
  that respondents of different groups do or don't have access.
But again, we have a theoretical question.
If we want the difference between Hispanic and non-Hispanic access,
  the numbers are correct.
But this encompasses a number of differences.
For instance, the Hispanic population is younger and lower income.
Ages and income levels unsurpringly explain the vast majority of variation 
  between neighborohoods' broadband subscription rates.
So we could pose a different question:
  are a Black respondent and a White respondent
  of the same age and income equally likely to be online?

#### Conditional Estimates

There are many ways of addressing this question,
  but what I'll do here is called a $k$-nearest neighbors regression.
What this means is that for every respondent in group
  _A_ (Black, Hispanic, or Female) in a metro area
  we'll find the _k_ respondents that are most like them in group 
  _B_ (non-Black, non-Hispanic, or Male), in the same metro.
We'll take the average across the _k_ matched "neighbors,"
  and compare the response that the respondent actually gave, regarding their Internet access.
Again, this answers the question:
  "if I, a member of group _A_ held constant certain characteristics about myself like age and income
   but were in group _B_, would I be more or less likely to have Internet access?"

The answers for this question are presented in the Table above, 
  in the columns ΔₖRace, ΔₖEthn., ΔₖSex.

### Consistency of the ACS and the CPS (Technical!)

In our work, we were careful to reproduce official estimates
  before proceeding to the ones that we've shared above.

#### American Community Survey API: Broadband or _Any_ Access?

The ACS API variable that we used was [`DP02_0153PE`][153PE],
  which is the share of total households "with a broadband Internet subscription."
Note first that this does not include "group quarters" like jails or dorms.
But second, while working with the IPUMS data,
  I found that it was not the _broadband_ variable [`CIHISPEED`][cihispeed]
  but [`CINETHH`][cinethh] that reproduced the API estimates at the state level.
`CIHISPEED` is based on the questionnaire text:

> Do you or any member of this household have access to the Internet using a broadband (high speed) Internet service such as cable, fiber optic, or DSL service installed in this household?

whereas `CINETHH` is supposed to be _any_ Internet connection:
> Do you or any member of this household have access to the internet?
> <br>&nbsp;&nbsp;[ ] Yes, by paying a cell phone company or Internet service provider
> <br>&nbsp;&nbsp;[ ] Yes, without paying a cell phone company of Internet service provider 
> <br>&nbsp;&nbsp;[ ] No access to the Internet at this house, apartment, or mobile home 

At the state level,
  the correlation between the API and my reconstructed value was 0.998 with `CINETHH`
  but just 0.892 with `CIHISPEED`.
To my mind, this appears to be a data error or a labelling error in the API.

#### The Current Population Survey

The analysis above is based entirely on the ACS data,
  but the CPS provides similar reports.
An analysis using those data is also included in the [git repo][repo].

Their corresponding survey question is `HEINHOME`:
> Does anyone in this household, including you, use the Internet at home? This includes accessing the Internet with a cell phone, computer, tablet, or other device.

They also have `HEHOMTE1`, 
> I am going to read a list of ways that people access the Internet from their homes, other than a mobile data plan.
> At home, (do you/does anyone in this household) access the Internet using:
> <br>High-speed Internet service installed at home, such as cable, DSL, or fiber optic service [...]

So `HEINHOME` seems to match `CINETHH`, and `HEMOMTE` matches `CIHISPEED`.

As above, before proceeding to my own estimates,
 I paintstakingly reproduced the ones that Rafi Goldberg made for the NTIA's state-level [map][ntia-map].

Based on this, the values that we want to compare are `HEINHOME` in the CPS and the 
  (apparently actually) `CINETHH` from the ACS's API.
Of course, we must also be consistent between units of observations (households vs person-level), 
   and in the decision of whether or not to include group quarters.
Here, I am matching the ACS choices: focusing _any_ access, in non-GQ households.
The consistency between the ACS and the CPS was, on the whole, not great.
For CBSA's the correlation between the two measures
  (ACS broadband via the API and the CPS's `HEINHOME`) was just 0.42.
For states, the correlation from the ACS API value 
  to `HEHOMTE1` was 0.72 (with the Universe described) but just 0.54 for `HEINHOME`.
Numerically, the ACS values are _much_ higher than the CPS ones.


One last note:
Despite its smaller sample size, the CPS does make it possible to identify the largest twenty cities (Census places) in the United States.
This is because they do not also have the smaller "PUMA" geographies that the ACS reports in its microdata.
Based on this, I reproduced the population for the twenty largest cities. 
I was very surprised, and frankly still incredulous, to find San Francisco near the bottom of the heap 
for pretty much any definition of Internet access.  San Jose was at the top.
But the differences were also not small!!
The NTIA analysts suggest that it might simply be an unusual statistical noise, but that seems unlikely.
For certain estimates, San Francisco was 3&sigma; below the rest of California (about a 1 in 300 chance).


[^1]: A greater variety of geographies is available, using [5-year][acs-geog-5] estimates (5 years of data) than for just [1-year][acs-geog-1] estimates.  Using the links, you can compare available geographies.

[153pe]:            https://api.census.gov/data/2019/acs/acs5/profile/variables/DP02_0153PE.json
[acs-api-call-1]:   https://api.census.gov/data/2019/acs/acs1/profile/?get=NAME,DP02_0087E,DP02_0153PE&for=place
[acs-geog-1]:       https://api.census.gov/data/2019/acs/acs1/geography.html
[acs-geog-5]:       https://api.census.gov/data/2019/acs/acs5/geography.html
[acs-pums-call-1]:  https://api.census.gov/data/2019/acs/acs1/pums?tabulate=weight(PWGTP)&col+HISPEED&row+RACBLK&for=state:17
[acs-pums-vars]:    https://api.census.gov/data/2019/acs/acs1/pums/variables.html
[data-org]:         https://data.org/
[ipums]:            https://usa.ipums.org/usa/
[repo]:             https://github.com/JamesSaxon/internet-access-and-equity
[cihispeed]:        https://usa.ipums.org/usa-action/variables/CIHISPEED#questionnaire_text_section
[cinethh]:          https://usa.ipums.org/usa-action/variables/CINETHH#questionnaire_text_section
[ntia-map]:         https://www.ntia.doc.gov/data/digital-nation-data-explorer#sel=homeInternetUser&disp=map



<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.11.4/css/jquery.dataTables.css">
<script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.11.4/js/jquery.dataTables.js"></script>

<script>
$(document).ready( function () { 

  table = document.getElementsByTagName('table')[0];
  table.id = "results";
  table.classList.add("display");
  $("#results").DataTable({paging: false, searching: false, info: false })

} );
</script>



