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
Using more than one to cover an area is preferable than to have a single one struggling to cover the entire area

---

<b>GOALS</b>

The goals here are simple:

The user should connect to wifi and forget about it
If the user is walking around, the connection should not be dropped, even if in a voip call or in the middle of a download
Wifi should be secure enough to avoid most attacks, but should also be retrocompatible with older devices (but not ancient, WEP/WPA1 should be discarded as insecure, i'm writing about WPA2/WPA3 compatibility) to avoid anoyances, retrocompatibility should take precedence over security
It should be like a toilet flush, you push a button, it works.

---

<b>PLANNING</b>

Choose an OpenWRT or DD-WRT compatible router if possible, it enables many features not usually present in the stock firmware, in my experience OpenWRT proved to be faster and more stable of all solutions tested

Plan the number of total access points and define where you will place them, i recommend a minimum of two, preferably mounted on the ceiling, or in a place where people cant interfere with the signal and more importantly, touch the access point

Those access poits will work as a <b>dumb ap</b> meaning their only function is to provide wireless networking, and nothing else, no other network services, to for this you will have to decide:

A common password for management purposes (not the wifi password)
Hostname for the access point
Fixed Ip Address (do not use dhcp, it will make managing it a nightmare)
Preferably non overlapping channels on the networks 2.4ghz and 5ghz, but its not the end of the word if the channels overlap a bit
The name you want your wifi network
The password for your wifi network

For example:

SSID (the name of the Wifi network): wifinetwork
Password: wifi@here
ap-bedroom    - 192.168.1.11 - Channel 2.4ghz: 1  / Channel 5ghz: 36
ap-livingroom - 192.168.1.12 - Channel 2.4ghz: 6  / Channel 5ghz: 40
ap-kitchen    - 192.168.1.13 - Channel 2.4ghz: 11 / Channel 5ghz: 44

Then flash OpenWRT or any other solution that you prefer, but this text will continue based on OpenWRT

What will be configured:
Hostname
IP Address
A wireless ssid (the name of the network that shows for devices trying to connect)
A wireless password
Roaming (you can walk around without needing to change the network)
Fast Roaming (same as above but you won't lose any connectivity in doing so)
The best possible retrocompatible security

---

<b>CONFIGURATION</b>

After flashing your OpenWRT, access the default ip of http://192.168.1.1 and click on login as there is no password set yet

Go to SYSTEM -> SYSTEM -> HOSTNAME, insert the hostname of this ap and Save & Apply
Go to SYSTEM -> ADMINISTRATION, change your password and save
Go to NETWORK -> INTERFACES -> LAN, click on EDIT

In GENERAL SETTINGS tab
    IP Address: The IP for this ap, for example 192.168.1.11
    IPv4 Netmask: 255.255.255.0
    IPv4 Gateway: 192.168.1.1 (change if this is not your default gateway)
    IPv4 Broadcast: 192.168.1.255

In DHCP Server Tab
    Check "Ignore Interface", this disables DHCP on this access point
    
Click save, on the lower part of the screen on Save & Apply, click on the small down arrow and choose Apply Unchecked, it will turn the button to red, click it wait for about a minute or so and access the ap by the new ip you define on lan interface, if you can't access it anymore by the old or the new ip, you did something wrong, reset your router and redo the configuration
 
 Access it again, go to NETWORK -> WIRELESS
 
 Remove the Default OpenWRT SSID
 
 Click add on the 2.4ghz radio
    DEVICE CONFIGURATION
        GENERAL SETUP Tab
            Change the channel, try to use non overlapping channels on the 2.4ghz network
            Width: 40mhz
        ADVANCED SETTINGS
            Check Force 40MHz Mode
            Beacon Interval: 200

Forcing width to 40mhz means the clients can achieve maximum theoretical output of 150mbps on this radio, wich is 50mbps faster than a standard 10/100 fast ethernet cable

Note: Setting the beacon interval at 200 instead of the default 100 on the 2.4ghz radio will encourage clients to try the 5ghz radio first unless the signal is very weak

    INTERFACE CONFIGURATION
        GENERAL SETUP
            Mode: Access Point (WDS)
            ESSID: wifinetwork
            Network: LAN
        WIRELESS SECURITY
            Encryption: wpa2-psk
            Key: wifi@here
            Check 802.11r Fast Transition
            Mobility Domain: 4f57
            FT protocol: FT over the Air
        SAVE

The Mode Access Point WDS means you can add later a wifi bridge or repeater to this network without needing special configuration

The encryption suggested is wpa2-psk because at the time of this writing is the most compatible, you can use whatever you like <b>as long as all the access points are configured with the same encryption and key</b>, also its not recommended to use anything bellow wpa2 (like wpa/wep)

Fast Transition allows faster roaming of the client between the access points, meaning you can be in a voip call or downloading and roam around the environment, the call wont cut and the download wont stop. FT over The Air makes this process faster.

Click add on the 5ghz radio

    DEVICE CONFIGURATION
        Channel: 36
        Width: The largest width available
        Leave unchecked: Allow legacy 802.11b rates
        Leave unchecked: Force 40MHz mode
        Don't change beacon interval
  
    INTERFACE CONFIGURATION
        GENERAL SETUP
            Mode: Access Point (WDS)
            ESSID: wifinetwork
            Network: LAN
        WIRELESS SECURITY
            Encryption: wpa2-psk
            Key: wifi@here
            Check 802.11r Fast Transition
            Mobility Domain: 4f57
            FT protocol: FT over the Air
        SAVE
        
What this configuration will achieve is leaving the shorter range, high throughput 5ghz radio being the preffered network, while the higher range, lower throughput 2.4ghz as the backend network

But since they both share the same name (SSID) and password (Encryption), this means the clients will only see one network, will only connect and input password once, and transparently roam on the environment and between the networks, the end user won't even noticing

You can check by making a voip call, starting a download, etc and roaming around. When the device roam from one access point to the other you will notice the strenght bar going lower and then suddenly full again

Combine this with a good positioning of the access points and it is possible to achieve a level of quality in wireless networks akin to a "magic infinite ethernet cable"

Good Luck.
