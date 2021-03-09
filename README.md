# QoE_Wifi
A how-to on how to achieve the best quality of experience possible on wifi for your users.

---

<b>INTRODUCTION</b>

Infrastructure should be transparent to the end user, it should be stable and work as such the user should'ndt even remember it
Lets imagine a toilet flush, to you think about yours sometimes? do you worry if its not working? most probably not, you push a button, it flushes, and thats it, and thats all it should be

Infrastructure should simply work

In the case of wifi, your wifi should be set as such to povide the end users to a level of stability and quality akin to a invisible and magic plugged ethernet cable that follows them all around, they should not complain about wifi, they should not even remember it

They should pick their phones / tablets / notebooks, use them, and thats it

This is the goal here.

---

<b>WIFI</b>

Wifi has many excellent solutions around, and others not so much, for example i've worked with ubiquiti products before and they are excellent, mostly set up and forget, and centrally managed, so if you change the wifi password on the central (in my case a virtual machine running their solution) it changes on all your access points, BUT they are not cheap, and you can achieve the same quality if commodity access points running openwrt, albeit with more management overhead (you have to configure the access points one by one)

So this how-to will focus on OpenWRT running on cheap hardware, it will lower the cost of deployment to a fraction of that would be using an enterprise solution, offer the same features and work perfectly for home or small enterprise networks, but you you need to cover a school or something with high density traffic, go ubiquiti or other enterprise solution.

Wifi also had some characteristics that need to be considered, wifi used radio signals that have to deal with propagation, reflection... etc. To keep it simple imagine this: There is a wall in front of you, you cant see beyond it because light travels back and forth to your eyes, to the wall block your vision

Radio signals have two main "walls" for it: metal and water, metal can be found in many places, even darkened glass/colored glass have metal in it, some walls have metals, some doors have metals or are made of metal. Water can also be found in many different places, like rain, plumbing, mist, you know what also have a lot of water? you and me

So if you stand in front of a wifi access point and i stand in front of you and pick my phone, i won't have a signal? no, the signal will be most likelly lessened, but you will still have signal due to reflection

Radio signals propagated from a wifi access point are likely omnidirectional, meaning they propagate in all directions, some of the surfaces/walls/things reflect those signals back around, and when your device receives and sends data back it reaches the access point by the same reflection

This is why in some places you have (weak) wifi signal even though in theory you should have none

Reflection is not ideal but should be avoided if possible, its better than no signal

There is also signal strenght, which is limited by regulations on maximum power/etc and even if it wasnt it would not help much, imagine i mount a giant antenna on my giant access point and send a wireless signal all arround 1km, if i got to the edge of the signal and try to connect, it won't, because my tiny phone would not have power to send data back

To summarize:
Mount your access points preferably on the ceiling, or in places where there wont be much people passing in front of the antenna signal
Try to avoid reflections if possible
Using more than one to cover an area is preferable than to have a single one struggling with everyone

---

<b>GOALS</b>

The goals here are simple:

The user should connect to wifi and forget about it
If the user is walking around, the connection should not be dropped, even if in a voip call or in the middle of a download
Wifi should be secure enough to avoid most attacks, but should also be retrocompatible with older devices (but not ancient, WEP/WPA1 should be discarded as insecure, i'm writing about WPA2/WPA3 compatibility) to avoid anoyances, retrocompatibility should take precedence over security
It should be like a toilet flush, you push a button, it works.
