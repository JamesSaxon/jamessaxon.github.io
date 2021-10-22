---
layout: post
author: Jamie Saxon
title:  Practical Steps for Improving your Wi-Fi
date:   2021-10-05
categories: broadband performance wifi
---

Over the past year in the Center for Data and Computing, 
  we've built some tools for measuring home Internet performance.
I've described this in past posts on [broadband][broadband].
With those tools installed in our homes,
  we can rigorously measure what you already know:
  the Wi-Fi stinks!
The speeds that you pay for from your Internet Service Provider
  often fail to reach the devices that you use!

A number of collaborators and community members have asked
  what they can do to improve this situation.
Fortunately, a few simple changes
  can improve the performance of your network,
  and potentially save you money.
This post will explain those tricks,
  but here they are in short:

1. Place your Wi-Fi far from big hunks of metal and other electronics, _especially_ your microwave.
2. If at all possible, put your Wi-Fi high up, like on a cabinet or a bookshelf.
3. Buy your own modem/router combo.

### Move the Wi-Fi away from metal and microwaves!

This is probably the most important point.
Your Wi-Fi transmits data as electromagnetic waves,
  and metal can reflect or shield these waves.
I used to work on this physics experiment:

<img src="/assets/img/trt_annotated.jpg" alt="Transition Radiation Tracker" style="padding:10px" height=250px class=center />

It is wrapped in metal.
All the beautiful, orange copper is called a Faraday cage.
(Not the support cage, the shiny stuff.)
The purpose of this metal wrapping 
  is to block electromagnetic radiation
  from interfering with the sensitive electronics inside.
Simply put, metal blocks electromagnetic waves.

This is where I used to have my Wi-Fi:

<img src="/assets/img/wifi_placement.jpg" alt="Old placement" style="padding:10px" height=470px class=center />

I had it there because that is where the cable came out of the wall,
  and about as far as it reached.
But, in this location, my Wi-Fi access point
  was nearly engulfed in sheets and chunks of signal-blocking metal:
  the dishwasher, the oven, and the radiator!
It wasn't _quite_ a Faraday cage, but it was still stupid,
  and it certainly blocked the signal.
Get smart!  Move your Wi-Fi away from metal!

So why is your microwave oven _particularly_ bad.
Your Wi-Fi communicates by emitting and receiving microwaves.
The FCC lets Wi-Fi use two frequencies or _bands_:
  2.4&nbsp;GHz and 5&nbsp;GHz.
Two bands -- more data!
But the bands also serve different purposes.

Lower frequencies are better at penetrating through stuff
  (walls, furniture, or people),
  but you can embed more data in higher frequencies.
More data means faster "speeds."
So: 5&nbsp;GHZ is good for speed
  while 2.4&nbsp;GHz is good for coverage.
Unfortunately,
  microwaves and old, cordless phones 
  can also use that 2.4&nbsp;GHz band.
When they do, they interfere with the Wi-Fi's signals.

The reason that your 
  microwave works at around 2.4 GHz is interesting.
The food that you heat in your microwave is largely water.
While fairly high frequencies (10-100&nbsp;GHz)
  are good at dumping heat (power) into water,
  they do not penetrate deep _into_ it (see [here][microwave-physics]).
Since you want to heat your whole bowl of rice
  and not just toast it,
  history stumbled on 2.45&nbsp;GHz for microwave ovens.

What is the meaning of all this?
Well there are two thick, brick walls
  between the Wi-Fi in my kitchen
  and the back bedroom of my apartment.
When I connect to the Wi-Fi from the bedroom,
  my computer sensibly chooses the 2.4&nbsp;GHz band 
  since the signal from that lower-frequency signal
  is stronger there.
Then, around 11am or noon
  as my neighbors start microwaving food (covid, so we're all home),
  there's a ton of interference on that band.
My computer switches over to 5&nbsp;GHz, 
  but that signal can't make it through the walls.
So my signal stinks.

Do I _actually_ see "lunch-time blues?"
Yes, yes I do.

What's to be done?
If you can't move your Wi-Fi out of the kitchen,
  another reliable solution
  (if it's possible in your home),
  is to route an Ethernet cable to spaces
    in your home where you need high bandwidth,
    like your home office or 4K streaming device.
If that's not possible, you can look
  into signal repeaters or mesh networks -- 
  but I won't get into those here.

### Raise your Wi-Fi to the sky!

The stuff in your apartment is probably on the floor,
  because well, _gravity_.
If your Wi-Fi access point is on the floor (as mine was),
  the signals will likely have to go through or around that stuff,
  to reach you.
If your Wi-Fi is up high, 
  the signal can travel through less stuff.
The signal will be stronger when it reaches you
  if it travesl through less stuff en route.

### Buy your own equipment!

This tip is a little bit more complicated,
  but it can potentially save you a ton of money.

Out of convenience,
  most people rent a modem/router/Wi-Fi combo
  from their Internet Service Provider (ISP).
I certainly used to.
And in some respects, it simplifies things:
  one less thing to worry about,
  one less decision to make.
But Comcast charges $14 a month for rentals -- $168 a year.
For just a little more money, 
  you can buy yourself a better modem/Wi-Fi combo, 
  that you will own for years.

This saves you money not only on the rental itself;
  it also makes it easier to maintain a cheaper contract with the ISP.
When the "initial rate" of an ISP contract expires,
  usually after 12 months,
  you can simply call and tell the ISP
  to either reset your rate or cancel your contract.
Since you own the equipment,
  there's nothing to return and no hassle.
Then, have your spouse, partner, or housemate 
  sign up for service again at the same address
  (or do it for them).
Three times in the last few years,
  Comcast just gave me (or my in-laws) the service price I asked for.
Once, 
  I had to create a new contract in the "other" spouse's name.
But there was never an interruption in service.
By owning my modem, I pay just $25 a month
  for our home Internet, and it is entirely adequate.
My annual gab-fest with Comcast customer service,
  always performed with a glass of wine,
  takes about an hour but has saved me up to $500 per year.
Owning equipment makes the call a lot easier.

If you _do_ decide to replace the ISP rental,
  there are a few things that you need to be aware of.
1. You need not just a modem or a router,
     but a modem, router, and Wi-Fi access point.
   You can read about these distinct functions, [here][terms-myths].
   This said, you can easily buy a box that combines these three separate functions.
2. Whatever combo (or set of boxes) you buy,
    make sure that all components can support speeds
    well in excess of your current contract.
   For example, modem, router, and Wi-Fi
     are rated for 600 Mpbs, 1000 Mbps, and 1300 Mbps,
     respectively.
   I buy 60 Mbps, so these devices are more than capable
     for me, but if you have a gigabit (1000 Megabit)
     plan, you'll need fancier equipment
   Moreover, ISPs do tend to bump up speeds over time, and 
     you won't save yourself any money
     if you end up buying another router, next year.
   If you rent, 
     Comcast and AT&T do ensure that the device
     is adequate for your contract.
   If you buy, you become responsible for that check.
3. The choice among available routers can be a little daunting.
   PC Magazine has a good and up-to-date
    [review][pcmag] of good Wi-Fi routers for 2021, 
    but these will also require a separate modem.
   If you're after simplicity, I have recommended
     this [combo from Netgear][netgear]
     for speeds up to about half a gigabit per second (500&nbsp;Mbps).
   (Again: if you have a top-tier, gigabit Internet plan,
    you'll need fancier, more-expensive equipment.)
   If you're looking for something snazzy,
     Ubiquiti makes good equipment with excellent software,
     but the learning curve may be steeper.
   Personally, I have been very happy with my [R6900P router][my-router],
     also from Netgear, paired with [this modem][my-modem].

A little caveat on all this.
Comcast or AT&T _should_
  ensure that whatever they rent to you is adequate for your subscription.
If that were true, then buying your own equipment
  would save you money but not improve speeds.
However, in my own experience,
  I was able to improve my experience and speeds measured 
  with the equipment I bought.


[my-modem]:           https://www.amazon.com/gp/product/B06XH46MWW/
[my-router]:          https://www.amazon.com/gp/product/B07C65K9H9/
[netgear]:            https://www.amazon.com/NETGEAR-Nighthawk-Certified-Xfinity-Spectrum/dp/B00ZUPOF7Y/
[terms-myths]:        {% post_url 2021-02-26-broadband-faqs %}
[broadband]:          https://jamessaxon.github.io/broadband/
[net-data]:           {% post_url 2020-12-06-public-data-on-internet-equity %}
[net-measurements]:   {% post_url 2020-12-07-basic-measurements-of-access-networks %}
[net-traffic]:        {% post_url 2020-12-09-measuring-internet-traffic %}
[net-traffic-switch]: https://jamessaxon.github.io/broadband/measurement/consumption/2020/12/09/measuring-internet-traffic.html#1-wired-hardware-switch-mirroring
[how-much]:           {% post_url 2021-02-01-how-much-bandwidth-for-video %}
[comcast-ie]:         https://www.internetessentials.com/covid19
[apple-resp]:         https://www.ietf.org/archive/id/draft-cpaasch-ippm-responsiveness-00.html
[mapping-mitigating]: https://cdac.uchicago.edu/news/mapping-and-mitigating-the-urban-digital-divide/
[data-org]:           https://data.org/
[oplat]:              https://github.com/kyle-macmillan/OpLat
[speedtest]:          https://www.speedtest.net/
[mlab]:               https://speed.measurementlab.net/#/
[microwave-physics]:  http://www.sfu.ca/phys/346/121/resources/physics_of_microwave_ovens.pdf
[pcmag]:              https://www.pcmag.com/picks/the-best-wireless-routers


