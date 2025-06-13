---
layout: post
author: Jamie Saxon
title:  Natural Shapes and Shadows
date:   2021-11-04
categories: art
---

tl;dr: I made lamps about trees and ripples:

{% include imagecap.html url="shapes_shadows/shoulders/1110324.jpg" width="65%" description="<em>Shoulders.</em>" %}

A few years ago, through my academic work on [park access][parks]
  and my project to [cut mountains from paper][paper-mountains],
  I got interested in using natural patterns to make soothing objects.
Frederick Law Olmsted, the designer of Central Park in New York 
  and Jackson Park in Chicago, 
  described the influence of nature beautifully, [in 1865][olmsted-yosemite]:

> the enjoyment of scenery employs the mind without fatigue and yet exercises it, tranquilizes it and yet enlivens it; and thus, through the influence of the mind over the body, gives the effect of refreshing rest and reinvigoration to the whole system.

More recently, E. O. Wilson's _Biophilia_ set the stage for biophilic design,
  though I was personally drawn to simulate not just living forms
  but _natural_ phenomena more broadly.

It is at once inspiring and a little exasperating
  how academic work can rigorously (and _ad nauseum_) validate
  what we already "know," or believe.
The restorative psychological (executive function, etc.)
  and physiological (heart rate, cortisol) influence of
  exposures to nature have been measured rigorously (e.g., [1][markevych], [2][kaplan]).
Parks and open spaces yield further health benefits
  as a venue for exercise and by fostering communities.
[Marc Berman's][berman] lab at UChicago
  has measured how people process and 
  respond to the visual cues of natural forms,
  and described the ways in which these forms are incorporated in architecture.
These include self-similar patterns (fractals),
  multiple scales, semantic cues (ideas and words), colors, and so forth.
The idea of delineating and quantifying the influences 
  of nature that we all experience affected me strongly.

In short:
  I set out to use light
  to express the subtle curves of tree branches, 
  and to echo the calm of ripples on a pond.

### Modeling Natural Processes

I began by modeling processes and patterns.
Algebra and computation offer two divergent paths for doing this.
We can derive the solutions to mathematical equations for
  things like ripples in a coffee cup, vibrations on a drum, or waves in water.
Or we can define the algorithms or "rules"
  for phenomena like the leaves or branches of trees,
  whose stochastic realizations are unique but intelligible.

#### Ripples and Waves

Just as sculptors study anatomy, I began by reviewing the physics.
Science helps focus observations;
  the [Feynman Lectures][feynman] (47-51) are wonderful for this.
How deep is the water?
Are the ripples caused by gravity or by surface tension?
How does a single crest move with respect to a group (group vs phase velocities)?
The individual crests may move forward or backwards within a group,
  and different physics dominates at different scales.

Still, when I began to simulate the full lagoon, I was unsatisfied with my results.
I may revisit this later: lots of people have figured it out.
Just look at the animation of water in _Moana_
  or the video game _Uncharted_.
But [this presentation][uncharted] chronicles the years of work 
  it took, to render the (dynamic, shaded) water of a single scene!
Too much for my hobby project!

#### Solutions to the Laplace Equation on an Arbitrary Surface

Failing to capture mathematically what I saw in the park,
  I fell back on one of the more enjoyable things that I learned from physics:
  the harmonic "ripples" of a membrane clamped along a boundary -- a drum.
These "harmonics" are the ways
  that your drum likes to shake,
    just like the fundamental of a piano string 
      and its overtones.
Or you and your dance playlist.
If you've known me for a bit,
  I have probably raved at you about Bessel functions
  while banging on the table 
    and scrutinizing the ripples in my coffee.[^1]

You can solve this equation
  (the Laplace equation with Dirichlet boundary conditions)
  for any surface or shape, like so:[^2]

```
ℒ = Laplacian[u[x, y], {x, y}];
ℬ = DirichletCondition[u[x, y] == 0., True];
poly = Rectangle[{0, 0}, {1, 2}];
{vals, funs} = NDEigensystem[{ℒ, ℬ, u[x, y], {x, y} ∈ poly, 100];
```

Without thinking too carefully, 
  I played with a bunch of shapes in Mathematica, 
  and found solutions that I liked for a 1&times;2 rectangle:

<div class="img_set center">
  <a href="/assets/img/shapes_shadows/cont_1_2_15.png" data-lightbox="all" id="ss01" data-title="Solutions 15 and 16."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_15.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_21.png" data-lightbox="all" id="ss02" data-title="Solutions 21 and 22."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_21.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_31.png" data-lightbox="all" id="ss03" data-title="Solutions 31 and 32."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_31.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_34.png" data-lightbox="all" id="ss04" data-title="Solutions 34 and 35."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_34.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_35.png" data-lightbox="all" id="ss05" data-title="Solutions 34 and 35."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_35.png" alt=""/></a>
  <br>
  <a href="/assets/img/shapes_shadows/cont_1_2_49.png" data-lightbox="all" id="ss06" data-title="Solutions 49 and 50."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_49.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_68.png" data-lightbox="all" id="ss07" data-title="Solutions 68 and 69."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_68.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_69.png" data-lightbox="all" id="ss08" data-title="Solutions 68 and 69."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_69.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_74.png" data-lightbox="all" id="ss09" data-title="Solutions 74 and 75."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_74.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/cont_1_2_75.png" data-lightbox="all" id="ss10" data-title="Solutions 74 and 75."><img class="thumb_soln" src="/assets/img/shapes_shadows/cont_1_2_75.png" alt=""/></a>
</div>

These have regular but non-trivial curves
  that feel a bit art nouveau.
Now, if you've taken freshman physics,
  you may think that I have made a mistake with my math!
The solutions to wave equations for rectangular surfaces
  are separable horizontally and vertically:
  sine waves each way!
That means: a rectangular grid of ups and downs.
We should find nothing so interesting!
Nothing really, to "employ the mind without fatigue,"
  or "enliven it" at all!

The trick is that for rectangles like mine, 
  where the horizontal and vertical directions are multiples of each other, 
  you get "degenerate" solutions.
Formally, this just means that the vibrating energy of the "drum"
  is equal for two of the boring configurations.
For instance, 1 ripple up and 3 across,
  has equal energy on a square drum, 
  to 3 up and 1 across.
When that is true, you can replace solutions _A_ and _B_ with 
  _A_&nbsp;+&nbsp;_B_ and _A_&nbsp;&minus;&nbsp;_B_,
  or indeed any "random" linear combination of _A_ and _B_![^3]
As it happens, 
  those "random" solutions are what Mathematica gave me, 
  and I first chose "degenerate" solutions purely on aesthetic grounds.
Only later did I wonder why I had gotten _any_ interesting solutions,
  stop to think about the math,
  and select the "random" solution that I liked best.[^4]
You can read about it more this in [this paper][kang-wei],
  or find nice visualizations [here][drussell].

### Leaf Venation

As for venation patterns, the classic (and very readable) paper seems
  to be [Modeling and visualization of leaf venation patterns][runions]
  by Runions et al.
This straightforward, biologically-inspired algorithm
  yields excellent results, out of the box.
The algorithm is initialized with

<ol type="A">
  <li> A starting point, i.e. a stem.</li>
  <li> A set of destinations, distributed across an area, i.e. a leaf.  Biologically, the destinations are sources of <em>auxin</em>, which attract growth.</li>
</ol>

It then loops on:

1. Associate the auxin sources with the closest point on the existing veins.
2. The existing veins are extended towards the set of sources for which they are the closest.
3. Remove auxin sources that are close enough to an existing vein.

This algorithm has been coded, described, and applied with beautiful results by [Jason Webb][webb]
  and by [Nervous System][hyphae], who in fact used it to make exquisite lamps and fonts 
    (as well as an explanatory [video][hyphae-vid]).

As for me, I just made an _M_-leaf for my son, Morgan.
My (very-rough) code is in [this gist][my-venation].

{% include imagecap.html url="shapes_shadows/m_venation.png" width="60%" description="Venation pattern for an <em>M</em>, generated using the algorithm by Runions et al." %}

Though this did not fit my immediate aesthetic aims,
  the basic algorithm is clearly capable of very nice results.
I look forward to revisiting this for generative grillwork/lattices.

### Capturing Natural Processes

As a counterpoint to my mathematical toys,
  I set about taking pictures of trees and water.
The challenge was that I needed to 
  be able to laser cut my images!

#### Old-School Printing Techniques

My initial strategy for this
  was to write code to translate raster pictures into
  vector formats that were guaranteed to be fully-connected:
  lithographies/wood-cuts and hexagonal half-tones.
This means using the brightness of the image to 
  specify the width of cuts or the areas of hexagons,
    and determine how much light to let through a piece of wood.
I found after the fact that the laser-cut half-tone 
  process has been done before, most notably by [Eric Forman][forman].
Here are some ripples from the Fox River and the Jackson Park Lagoon,
  as images, half-tone hexagons, and "lithographies":

<div class="img_set center">
  <a href="/assets/img/shapes_shadows/07_water.png" data-lightbox="all" id="pw07" data-title="Fox River I, photograph."><img  class="thumb_rip" src="/assets/img/shapes_shadows/07_water.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/07_hex.png"   data-lightbox="all" id="ph07" data-title="Fox River I, 'half-tone.'"><img class="thumb_rip" src="/assets/img/shapes_shadows/07_hex.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/07_lines.png" data-lightbox="all" id="pl07" data-title="Fox River I, 'woodcut.'"><img   class="thumb_rip" src="/assets/img/shapes_shadows/07_lines.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/16_water.png" data-lightbox="all" id="pw16" data-title="Fox River II, photograph."><img  class="thumb_rip" src="/assets/img/shapes_shadows/16_water.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/16_hex.png"   data-lightbox="all" id="ph16" data-title="Fox River II, 'half-tone.'"><img class="thumb_rip" src="/assets/img/shapes_shadows/16_hex.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/16_lines.png" data-lightbox="all" id="pl16" data-title="Fox River II, 'woodcut.'"><img   class="thumb_rip" src="/assets/img/shapes_shadows/16_lines.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/17_water.png" data-lightbox="all" id="pw17" data-title="Jackson Park Lagoon I, photograph."><img  class="thumb_rip" src="/assets/img/shapes_shadows/17_water.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/17_hex.png"   data-lightbox="all" id="ph17" data-title="Jackson Park Lagoon I, 'half-tone.'"><img class="thumb_rip" src="/assets/img/shapes_shadows/17_hex.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/17_lines.png" data-lightbox="all" id="pl17" data-title="Jackson Park Lagoon I, 'woodcut.'"><img   class="thumb_rip" src="/assets/img/shapes_shadows/17_lines.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/18_water.png" data-lightbox="all" id="pw18" data-title="Jackson Park Lagoon II, photograph."><img  class="thumb_rip" src="/assets/img/shapes_shadows/18_water.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/18_hex.png"   data-lightbox="all" id="ph18" data-title="Jackson Park Lagoon II, 'half-tone.'"><img class="thumb_rip" src="/assets/img/shapes_shadows/18_hex.png" alt=""/></a>
  <a href="/assets/img/shapes_shadows/18_lines.png" data-lightbox="all" id="pl18" data-title="Jackson Park Lagoon II, 'woodcut.'"><img   class="thumb_rip" src="/assets/img/shapes_shadows/18_lines.png" alt=""/></a>
</div>


In principle this works pretty well, 
  but in practice it requires large, contrasty images.
One improves the fidelity of the representation by 
  increasing the number of lines or hexagons,
  but the material, wood, sets an upper limit on this number.
The half-tone process did yield evocative results&nbsp;--
  your eye can "graze" over the hexagon patterns
  much as you track the crest of a single ripple in a pool
  or seek the sharpest bend in a smoothly arcing tree branch.
But I wanted viewers to understand consciously that
  they were looking at water, and my results were too abstract for this.
This was all the more true with the "lithographies."

#### Picturing Man and Nature

Next, I set out to capture silhouetted images
  that I could threshold directly.
In other words: take pictures of shadows,
  and convert those into laser-cut designs.
Photographing and processing a scene into a "legible" silhouette
  is harder than I first supposed.
It took me many months to get just a few acceptable images.
Here are trees set against trusses,
  natural against manmade forms:

<div class="img_set center">
  <a href="/assets/img/shapes_shadows/tree_a.jpg"  data-lightbox="all" id="tree_a"  data-title="Winter: Saginaw Forest, Michigan."><img  class="thumb_vert" src="/assets/img/shapes_shadows/tree_a.jpg" alt=""/></a>
  <a href="/assets/img/shapes_shadows/truss_a.jpg" data-lightbox="all" id="truss_a" data-title="Lake Shore and Michigan Southern Railway, Bridge No. 6 (now Norfolk-Southern). Calumet River, Chicago."><img  class="thumb_vert" src="/assets/img/shapes_shadows/truss_a.jpg" alt=""/></a>
  <br>
  <a href="/assets/img/shapes_shadows/truss_b.jpg" data-lightbox="all" id="truss_b" data-title="Lake Shore and Michigan Southern Railway, Bridge No. 6 (now Norfolk-Southern). Calumet River, Chicago."><img class="thumb_vert" src="/assets/img/shapes_shadows/truss_b.jpg" alt=""/></a>
  <a href="/assets/img/shapes_shadows/tree_b.jpg"  data-lightbox="all" id="tree_b"  data-title="Spring: Jackson Park, Chicago."><img class="thumb_vert" src="/assets/img/shapes_shadows/tree_b.jpg" alt=""/></a>
</div>

The similarities are clear:
  thicker load-bearing masts and the increasingly delicate branches and "twigs."
Both exhibit fractal patterns 
  that follow from shared structural requirements.
But while the components of the bridge are straight and regular
  and the "limbs" are supported from multiple points,
  trees' branches curve and fork without loops.
However, since any weight must be supported,
  the shadows are all connected and
  lend themselves to laser cutting.

### Creating Light and Shadow

Finally, I processed the images
  and wrote code to format them for the laser cutter.
I cut the trees and bridges,
  backed them with linen paper to diffuse the light, 
  and designed a "box" to hold them.

<figure class="img_set center">
  <a href="/assets/img/shapes_shadows/shoulders/1110244.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110244.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110245.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110245.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110248.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110248.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110250.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110250.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110252.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110252.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110256.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110256.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110258.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110258.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110269.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110269.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110290.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110290.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110293.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110293.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110295.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110295.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110311.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110311.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110313.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110313.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110314.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110314.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110324.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110324.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110317.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110317.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110320.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110320.jpg"/></a>
  <a href="/assets/img/shapes_shadows/shoulders/1110325.jpg" data-lightbox="all" data-title="Shoulders."><img class="thumb" src="/assets/img/shapes_shadows/th/1110325.jpg"/></a>
  <figcaption>
  <p class=center>
  <em>Shoulders:</em> Norfolk Southern rail bridge over the Calumet River, Jackson Park Chicago, and the Saginaw Forest.<br>Laser-cut wood panels backed with linen paper.
  </p>
  </figcaption>
</figure>

I called this lamp "Shoulders,"
  because it's mostly images of Chicago,
  and because they are bearing loads.

Of course, once we have a box,
  we can "trivially" assemble different ideas:
  time and length scales, cycles, natural vs designed, etc.
For example, this lamp sets two pictures of ripples
  against two of the harmonics for the 1&nbsp;&times;&nbsp;2 surface from above.
This grouping counterposes not man and nature
  but analytical solutions to physical equations
    against the phenomena as we observe them in the world.
Universal laws vs ephermeral realities, if you will:

<figure class="img_set center">
  <a href="/assets/img/shapes_shadows/ripples_b.jpg" data-lightbox="all" data-title="Ripples."><img class="thumb_pad" style="width: 35%;" src="/assets/img/shapes_shadows/ripples_b.jpg"/></a>
  <a href="/assets/img/shapes_shadows/ripples_a.jpg" data-lightbox="all" data-title="Ripples. (Caligraphy by Thich Nhat Hanh in the background.)"><img class="thumb_pad" style="width: 35%;" src="/assets/img/shapes_shadows/ripples_a.jpg"/></a>
  <figcaption>
  <p class=center>
  <em>Ripples:</em> thresholded images of the Jackson Park Lagoon, with solutions to the Laplace equation for a 1&nbsp;&times;&nbsp;2 surface.
  </p>
  </figcaption>
</figure>

Yet again, perhaps more prosaic, I spent literally a year
  taking shadow pictures of leaves through the four seasons:

<figure class="img_set center">
  <a href="/assets/img/shapes_shadows/seasons.png" data-lightbox="all" data-title="Seasons."><img class="thumb_pad" style="width: 70%;" src="/assets/img/shapes_shadows/seasons.png"/></a>
  <a href="/assets/img/shapes_shadows/fall.jpg" data-lightbox="all" data-title="Seasons: Fall."><img class="thumb_pad" style="width: 22.4%;" src="/assets/img/shapes_shadows/fall.jpg"/></a>
  <figcaption>
  <p class=center>
  <em>Seaons:</em> trees in Jackson park, through the year.
  </p>
  </figcaption>
</figure>

Happy Fall!

Notes:

[^1]: This is probably my favorite basic "factoid" from physics, though the resonant depolarization of the electron beam at LEP, used to get the mass of the _Z_ boson, is a close second.  Ask me about it for bonus ravings!
[^2]: For this _particular_ surface, `DEigensystem` works too, since it is algebraically solvable / trivial.
[^3]: For the eigensystem, the two solutions built from _A_ and _B_ must be mutually orthogonal.
[^4]: That is, I chose the angle of rotation for pairs of separable solutions.

<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox-plus-jquery.js" integrity="sha512-0rYcJjaqTGk43zviBim8AEjb8cjUKxwxCqo28py38JFKKBd35yPfNWmwoBLTYORC9j/COqldDc9/d1B7dhRYmg==" crossorigin="anonymous"></script>

<script>
 lightbox.option({
   'resizeDuration': 0,
   'imageFadeDuration' : 0,
   'wrapAround': true
 })
</script>



[paper-mountains]:  {% post_url 2020-01-10-paper-mountains %}
[berman]:           https://doi.org/10.1016/j.jenvp.2019.02.007
[feynman]:          https://www.feynmanlectures.caltech.edu/I_51.html
[runions]:          http://algorithmicbotany.org/papers/venation.sig2005.pdf
[hyphae]:           https://n-e-r-v-o-u-s.com/projects/albums/networks-sketches/
[hyphae-vid]:       https://n-e-r-v-o-u-s.com/projects/albums/networks-sketches/content/hyphae-growth-process-diagram-in-2d/
[webb]:             https://medium.com/@jason.webb/space-colonization-algorithm-in-javascript-6f683b743dc5
[uncharted]:        https://www.gdcvault.com/play/1015309/Water-Technology-of
[kang-wei]:         https://dx.doi.org/10.1177/1077546313500062
[olmsted-yosemite]: https://www.nps.gov/parkhistory/online_books/anps/anps_1b.htm
[drussell]:         https://www.acs.psu.edu/drussell/Demos/rect-membrane/rect-mem.html
[my-venation]:      https://gist.github.com/JamesSaxon/aaa5614eff18d1ed70564635f0729891
[parks]:            https://saxon.cdac.uchicago.edu/parks/
[markevych]:        http://dx.doi.org/10.1016/j.envres.2017.06.028
[kaplan]:           https://doi.org/10.1111/j.1467-9280.2008.02225.x
[forman]:           https://www.instructables.com/ScaleScape-Halftone-Light-Panel-series/




