---
layout: post
author: Jamie Saxon
title:  Building an Olm-meter
date:   2021-03-22
categories: urban measurement electronics
---

I love GPS location data.
The data themselves are simple, beautiful, and evocative, 
  and the applications are endless.
From just times, places, and identifiers,
  we can learn so much about how people use cities.

Using these data I've written several papers on 
  park access and neighborhood mobility.
One of the chief questions that I got while developing this work 
  is _why I trust these data_.
In the past, I've addressed this problem in three ways:
1. **Comparisons to the Census**:
   I compare device counts to population counts from the Census 
   to measure demographic bias.  As many of the location intelligence 
   companies themselves have shown, they are quite representative -- 
   though wealthy and educated areas are a bit more-represented
   and Hispanic areas a bit less so.
2. **Robustness Checks**: 
   I show that the specific observables constructed for _each study_,
   and the relationships ultimately described, are robust to 
   controls for the data rates.
3. **Picking at Scabs**: 
   After (substantial) data cleaning, basically every "feature"
   that I have seen in the data has turned out to be real.
   In many cases, I've confirmed things by going and _visiting_
   locations to understand what the data meant; 
   my favorite example of this was the parking lot 
   by the Jackson Park Lagoon, which is used for grilling and partying.

Ultimately, these exercises gave me confidence with the data.
But Luc Anselin (my advisor in CSDS) 
  kept pushing on this question,
  and from an abundance of ignorance, 
  I responded that we could create other groundtruths
  using electronics or computer vision, 
  and compare those to counts from the GPS location data.
Of course, I had never used computer vision or network cameras 
  before, so there was that.
I'll describe that process in another post.

Here, I'll describe a box with a
  ultrasonic and passive infrared sensors,
  built using the [Particle][particle] ecosystem,
  and uploading data to InfluxCloud over 3G.
It looks like this:

<figure>
  <a href="/assets/img/olm/1k/13_done.jpg" data-lightbox="olm-assembly" data-title="A completed 'Olm-meter.'"><img class=center height=350px src="/assets/img/olm/1k/13_done.jpg" /></a>     
<figcaption>
<p class=center>
A completed 'Olm-meter.'
</p>
</figcaption>
</figure>

I call it an Olm-meter, since 
  it's intended to count people in [parks][flo-wiki].

### The Electronics

The electronics for this box are uncomplicated;
  it took just a few hours to make a prototype.
The entire challenge was (and continues to be) 
  to make it work outside.
Let's start with the easy part!

The box contains two sensors: ultrasonic and passive infrared.
I wanted multiple sensors, to be able to compare performance over time.
One could also watch for [broken laser beams][ada-laser-beams], or any other manner
  of motion detection devices.
Ultrasonic sensors just let out a chirp and the listen for the echo.
As for PIRs, Adafruit has a very nice explanation of how they [actually work][adafruit-pir],
  the short version of which is that they house two IR sensors side by side, 
  behind "interwoven" Fresnel lenses (as in a lighthouse).
If that doesn't any make sense, it's because it's delightfully ingenious,
  and well worth your read.
In my initial tests, I used the HC-SR04 for the ultrasound and HC-SR501 for the PIR.
Both of these cost only about $2.
For the final version, I switched the sonic sensor to the waterproof JSN-SR04T.

The wiring diagram is _basically_ just this:
<figure>
<a href="/assets/img/olm-meter_fritzing.png" data-lightbox="olm-assembly" data-title="'Fritzing' diagram of the Olm-meter."><img height=350px src="/assets/img/olm-meter_fritzing.png" alt="" class=center /></a>
<figcaption>
<p class=center>
'Fritzing' diagram of the Olm-meter.  The Particle Electron at left powers, controls, and receives data from PIR and ultrasonic sensors, top and bottom respectively. (Click to expand.)
</p>
</figcaption>
</figure>

You wire ground to ground, 
  voltage to voltage, and 
  inputs and outputs to the digital GPIO pins.
There are a few caveats, though.
The sensors run at 5V, and 
  while I initially tested with a separate
  5V supply alongside the 3.3V battery,
  I found this a bit annoying.
I thus decided to power the whole thing from USB,
  to sidestep the power issue.
Adafruit has 5V USB batteries that actually provide a potential of 5V.
The other wrinkle is that the JSN-SR04T's input is 5V,
  which you should step down to 3.3V for the GPIO pin.

This works "out of the box" at home or in the lab, but of course
  we also want it to work outside.
Enter [Particle][particle].
Particle devices are basically Arduino devices
  (the same, c++ `setup()` / `loop()` scheme)
  but with wireless communications "solved."
With a single call of 
```
Particle.publish("my_var", var_to_publish);
```
a reading from the device is pushed to the cloud.
Friends at the SAIC introduced me to these, and they are great.
I began with the Wi-Fi based [Argon][argon],
  but jumped over to the 3G [Electron][electron]
  to go to the field.
It is notable that particle is now [free][particle-free]
  for up to 100 devices, which is amazing.

#### Part List

| Part  | Cost  | Use  |
|-------|-------|------|
| [Particle Electron][buy-electron]   | $71  | Microcontroller and wiresess communication  |
| [JSN-SR04T][buy-jsn-sr04t]          | $13  | Waterproof ultrasonic |
| [HC-SR04][buy-hc-sr04]              | $2   | "Stock" ultrasonic (not used)  |
| [HC-SR501][buy-hc-sr501]            | $2   | Passive Infrared  |
| [Silicone Caulk][buy-caulk]         | $6   | Waterproof PIR port  |
| [Gasket Rubber][buy-gasket]         | $9   | Sheet for cutting box gasket  |
| [Glands][buy-glands]                | $15 (bulk)  | Allow cable from JSN-SR04T to pass into box  |
| [Breadboard][buy-breadboard]        | $2   | Breadboard for mounting the Electron  |
| 3D-printed box                      | $40  | Put everything together.  |
| **Total**                           | ~$140 | (Some parts are bulk, etc.)  |

The price for the box is for the materials only, and includes neither the labor nor the equipment.
The price at Shapeways would have run to hundreds of dollars.
Note that this does not include the cost of other equipment either -- wire, solder, soldering irons, voltimeter.
It's hard to _really_ do this without a lab or a full closet.

### The Code

#### The Arduino Firmware

There are many Arduino tutorials online for the 
  [HC-SR04][sr04-tutorial] and 
  [HC-SR501][sr501-tutorial] (examples in links).
This project simply requires you to interleave those two parts,
  apply a little bit of processing, and upload the variables.
For example, two ultrasonic sensor "counts" (distance below threshold)
  20 ms apart do not count as "separate people."
So we must decide what thresholds to apply and 
  what separation in time counts as a "new" person.
I aggregate counts for a minute, and then push a fragment of Influx line protocol:
```
snprintf(pub_var, sizeof(pub_var), "p=%d,s=%d", pir_count, ultrasonic sensor_count);
Particle.publish("park_count", pub_var);
```

#### Particle / Influx Integration

You can create webhooks 
  when "events" arrive in the Particle cloud,
  to send data to a more-permanent location.

Particle actually has a page on [how to do this][telegraf-hook] using Telegraf.
Basically it's: ramp up an extra service in between
  Particle and Influx to make more-regular ingestion.
I think this is overkill.
You can have the Particle webhook 
  write directly to the Influx endpoint, 
  by sending the line protocol: 
{% raw %}
```
count,device=muir {{{PARTICLE_EVENT_VALUE}}} {{time}}
```
{% endraw %}
where `muir` is my device and {% raw %}`{{{PARTICLE_EVENT_VALUE}}}`{% endraw %} 
  is the tag I sent from the device.
You just have to copy in your organization and token into the HTML headers (Authorization: Token XYZ).

### The Box

The entire challenge with this project 
  was to make it portable and watertight.
I've solved most, though not all, of these issues.
I had not used CAD for 3D parts before, 
  and I opted to make a custom 3D box for this.
Iterating on this during COVID-times was somewhat slow,
  since many labs are limited right now.

I used FreeCAD for the design,
  mostly just using the Part / Part Design workbenches.
If you are familiar with basic geometric manipulation 
  a la GDAL, then this will all make sense.
I measured every piece of electronics with calipers,
  and parameterized the entire piece from that "spreadsheet."
This way, changing the entire box was "automatic"
  as a function of the composite parts.[^1]
The design is here:

<script src="https://gist.github.com/JamesSaxon/aa2d687727aa435df484bee716c9dcb7.js"></script>

As you can see, there are two holes in the front
  for the PIR and sonic senor.
The ultrasonic sensor is waterproof and on a lead, which enters 
  via a gland (not shown) into the main box.
The lens of the PIR is detachable and it plugs into
  the box directly, sealed with silicon caulk.
The PIR itself plugs into its mount.
On the back there are mounts for 
  a little breadboard (holding the Electron)
  and the controller-board for the JSN-SR04T.
The large box on the right side is for the battery,
  and the one on the left is for the antenna.

The lid slides over the box, and 
  incorporates holes for mounting the whole thing
  on an octagonal light-pole.
The complete box was printed using FDM by the Hack Arts Lab at the University.
Since 3D printing rubber is a bit more complicated,
  the gasket between the two pieces is laser cut.

Is this compact?  No.
Is there wasted space?  Yes.
Does it work?  Mostly.  It's fine.

### Assembly Pics

<div class="img_set center">
<a href="/assets/img/olm/1k/00_parts.jpg"        data-lightbox="olm-assembly" data-title="Assembly day at home, in covid-times."><img class=thumb src="/assets/img/olm/th/00_parts.jpg"          /></a> 
<a href="/assets/img/olm/1k/01_particle_kit.jpg" data-lightbox="olm-assembly" data-title="The Particle Electron, out of the box."><img class=thumb src="/assets/img/olm/th/01_particle_kit.jpg"   /></a>   
<a href="/assets/img/olm/1k/02_chip.jpg"         data-lightbox="olm-assembly" data-title="The cell chip."><img class=thumb src="/assets/img/olm/th/02_chip.jpg"           /></a>  
<a href="/assets/img/olm/1k/03_online.jpg"       data-lightbox="olm-assembly" data-title="After plugging this in, the device immediately goes online and can be programmed from the console, though the IDE / USB has better functionality and no data costs.  (On the other hand, they've reduced the data costs for the first 100 devices -- does this include for firmware!?)"><img class=thumb src="/assets/img/olm/th/03_online.jpg"         /></a> 
<a href="/assets/img/olm/1k/04_gik.jpg"          data-lightbox="olm-assembly" data-title="Let's jump over to a breadboard.  I cut this down to reduce the size a little."><img class=thumb src="/assets/img/olm/th/04_gik.jpg"            /></a>   
<a href="/assets/img/olm/1k/05_ultrasonic.jpg"   data-lightbox="olm-assembly" data-title="Both the ultrasonic and PIR sensors had pins and plugs that made them much larger, for a box.  I removed those and soldered the wires directly to the PCBs."><img class=thumb src="/assets/img/olm/th/05_ultrasonic.jpg"     /></a> 
<a href="/assets/img/olm/1k/06_pir.jpg"          data-lightbox="olm-assembly" data-title="The passive infrared device -- of which you can see the facets of the Fresnel lens -- is extraordinarily ingenious.  It is definitely worth reading how they work!!"><img class=thumb src="/assets/img/olm/th/06_pir.jpg"            /></a> 
<a href="/assets/img/olm/1k/07_front.jpg"        data-lightbox="olm-assembly" data-title="Now solder the whole thing together.  In the future, this would be a simple PCB."><img class=thumb src="/assets/img/olm/th/07_front.jpg"          /></a>  
<a href="/assets/img/olm/1k/08_back.jpg"         data-lightbox="olm-assembly" data-title="Same thing, but the back."><img class=thumb src="/assets/img/olm/th/08_back.jpg"           /></a>  
<a href="/assets/img/olm/1k/09_box.jpg"          data-lightbox="olm-assembly" data-title="Now let's go to the printed box."><img class=thumb src="/assets/img/olm/th/09_box.jpg"            /></a>  
<a href="/assets/img/olm/1k/10_inside.jpg"       data-lightbox="olm-assembly" data-title="The lens of the PIR plugged directly into the box, with silicone goop for waterproof-ness."><img class=thumb src="/assets/img/olm/th/10_inside.jpg"         /></a>    
<a href="/assets/img/olm/1k/11_gasket.jpg"       data-lightbox="olm-assembly" data-title="The lead of the echo sensor goes through a gasket to enter the water-resistant volume."><img class=thumb src="/assets/img/olm/th/11_gasket.jpg"         /></a>  
<a href="/assets/img/olm/1k/12_wired.jpg"        data-lightbox="olm-assembly" data-title="Connecting the last sensor wire in situ, since the cable goes through the gland."><img class=thumb src="/assets/img/olm/th/12_wired.jpg"          /></a>     
<a href="/assets/img/olm/1k/13_done.jpg"         data-lightbox="olm-assembly" data-title="And now all done -- and off to the park!"><img class=thumb src="/assets/img/olm/th/13_done.jpg"           /></a>     
</div>



### The Data

Finally, we have data.
It's apparent that the two sensors
  are recording very different numbers of hits per minute.
I am currently tuning this, to understand it better
  and make them more consistent over time.
As it stands, they can actually _flip_ (sonic > PIR) 
  when I rejigger the device!

<figure>
<a href="/assets/img/influx_park_counts.png" data-lightbox="olm-assembly" data-title="Person counts in the InfluxCloud, uploaded by the Particle Electron over 3G."><img height=350px src="/assets/img/influx_park_counts.png" alt="" class=center /></a>
<figcaption>
<p class=center>
Person counts in the InfluxCloud, uploaded by the Particle Electron over 3G.
</p>
</figcaption>
</figure>

### Works in Progress

Obviously, data quality is the core issue --
  I'd like to actually use the data.

The other big thing to solve is continuous power.
One battery lasts for almost exactly one day.
Swapping them out is OK for a week or so,
  but clearly it does not scale.
I can't install solar without some sort of blessing from the city,
  but all these poles have power in them, and I'd like
  for them to just give me access!




### Notes.
[^1]: The one thing that was not clear to me was how to apply filleting, so that it did not get lost if clearances or part sizes changed.

[adafruit-pir]:    https://learn.adafruit.com/pir-passive-infrared-proximity-motion-sensor/how-pirs-work
[flo-wiki]:        https://en.wikipedia.org/wiki/Frederick_Law_Olmsted
[ada-laser-beams]: https://www.adafruit.com/product/2122
[argon]:           https://docs.particle.io/argon/
[electron]:        https://docs.particle.io/electron/
[particle]:        https://www.particle.io/
[particle-free]:   https://blog.particle.io/announcing-particle-ethersim-and-new-pricing/
[sr04-tutorial]:   https://create.arduino.cc/projecthub/abdularbi17/ultrasonic-sensor-hc-sr04-with-arduino-tutorial-327ff6
[sr501-tutorial]:  https://www.makerguides.com/hc-sr501-arduino-tutorial/
[telegraf-hook]:   https://docs.particle.io/tutorials/integrations/influxdata/
[buy-electron]:    https://store.particle.io/collections/cellular/products/electron-3g-americas
[buy-jsn-sr04t]:   https://www.amazon.com/dp/B07V6HSN8B/
[buy-hc-sr04]:     https://www.amazon.com/dp/B07RGB4W8V/
[buy-hc-sr501]:    https://www.amazon.com/dp/B012ZZ4LPM/
[buy-caulk]:       https://www.amazon.com/dp/B01B5RBOA6/
[buy-gasket]:      https://www.amazon.com/dp/B07B52D32S/
[buy-glands]:      https://www.amazon.com/dp/B07KFCGFYF/
[buy-breadboard]:  https://www.amazon.com/dp/B071R3BFNL/



<script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox-plus-jquery.js" integrity="sha512-0rYcJjaqTGk43zviBim8AEjb8cjUKxwxCqo28py38JFKKBd35yPfNWmwoBLTYORC9j/COqldDc9/d1B7dhRYmg==" crossorigin="anonymous"></script>


