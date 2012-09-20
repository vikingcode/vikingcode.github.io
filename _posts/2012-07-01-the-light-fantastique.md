---
layout: page
title: Using Arduino to add ambient light adjustment to your monitor(s)
permalink: thelightburns.html
---

> The title lied, you can't directly use an Arduino to control the brightness of your monitor, there is some software needed on the PC side of things required. Okay, technically you could if you had a DVI/HDMI/DisplayPort interceptor hooked up to an Arduino, but that is far too complex and expensive. My approach is much cheaper ($40AUD although can be had for half that), USB powered, and is tasty looking to cats[^2] (may/may not be a great selling feature).

##Problem
The initial problem arose when a friend and I were playing games (Battlefield 3, specifically) at night. We both use [F.lux](http://stereopsis.com/flux/) which is fantastic *until* you start playing games - the gamma/colour shifts cause sections of games to be very difficult to see. This varies game to game, but in the 'artistic' styling of BF3's lighting, it was causing issues. My friend asked "is there someway I could control the brightness of my monitors in software like on a laptop?" My initial response was one of laughter, but halfway through that thought I pondered and remembered there was some bi-directional communication between monitor and PC. After further investigation, that turned out to be [Display Data Channel/Control Interface](http://en.wikipedia.org/wiki/Display_Data_Channel)

"Well", I thought, "That seems simple enough but that is somewhat of a boring solution. I still have to use my fingers to adjust brightness via keyboard now"

![](http://www.howtogeek.com/geekers/up/sshot4f07447e46648.jpg)

Doing things manually *sucks*, but standard monitors and PCs don't ship with ambient light sensors - my computer couldn't know when it was "dark" other than by the time of day - a poor judge based on how many lights are on in the room at once. I share a study with my wife, if we're both in here it is significantly lighter.

There are hacks[^1] to use a webcam as ambient light sensors, but given the only webcam I have is a *horrible* older Microsoft Lifecam, that wasn't an option. Arduino! I had always wanted a justifiable project for an Arduino, but this was the first that wasn't too complex that it'd end up being shelved halfway through. It just happened to be that [Freetronics](http://www.freetronics.com/) had very recently released the [LeoStick](http://www.freetronics.com/products/leostick#.UFpM0o0gfX4) - a tiny Arduino Leonardo compatible board, powered by USB.

##The Solution
###Hardware (Arduino)
Disclaimer: This is probably the third thing I've soldered in my life. Nobody taught me, so I'm making it up as I go along. If you're a electronics buff and thing I'm doing it wrong, yes, I probably am. 

You will need

* Soldering iron
* Solder
* Hookup wire
* [Leostick](http://www.freetronics.com/products/leostick#.UFpM0o0gfX4) or equivalent USB powered Arduino
* [Light sensor](http://www.freetronics.com/products/light-sensor-module#.UFpNUo0gfX4) shield

> I'm looking at purchasing the DealExtreme '[Arduino Nano](http://dx.com/p/nano-v3-0-avr-atmega328-p-20au-module-board-usb-cable-for-arduino-118037?item=3)' clone, and a [comparable light sensor](http://dx.com/p/light-sensor-photoresistor-module-for-arduino-blue-152409?item=2) - this would total $16 compared to the **$40** of the Freetronics approach. The DX Arduinos aren't *as* nice, but they'll do exactly the same job, at less than half the price.

This is a *really* simple hardware project - three wires and you're done.

*  Connect ground (GND) from the Arduino to the sensor board,
*  A0[^3] from the Arduino to the OUT on the sensor,
*  And either the 3.3v or 5v from the Arduino to the VCC on the sensor board.

Wire has to be used, the pins are a little bit too far apart to be useful, but very short cables will probably work best.

####Finished Result
<INSERT IMAGES HERE>


###Software ('Sketch' for the Arduino)
This is possibly the most basic sketch you can get, but that is because I'm *basically* just using the Arduino as a cheap USB<->light sensor bridge, it does *very* little work.

{% highlight raw %}

int lightLevel; 

void setup()
{
  Serial.begin(38400);
}

void loop() 
{
  lightLevel = analogRead(A0);
  Serial.println(lightLevel, DEC);
  delay(1000);
}

{% endhighlight %}

Once loaded onto the Arduino, it reads in the current value of A0 (which is what the sensor is wired up to), and dumps the raw result onto the serial port. This loops forever, repeating once a second.

###Software (The Light Fantastique)
Technically this *could* be presented to Windows as part of the Sensor and Location platform, but that is far trickier and wouldn't give full control. Instead by using *The Light Fantastique* (TLF) you can use the ambient light sensor or hotkeys, and control dissimilar multiple monitors with relatively values of brightness. 

What does that mean? No two monitors are the same - 50% brightness on one might be 100% on another. With a small amount of manual calibration, *TLF* remembers the difference so when you add "5" to brightness, it knows that monitor A actually wants 2.5 and monitor B wants 5.0

## Improvements
These aren't *planned* but possible improvements:

### Hardware
* Cost - getting a cheaper package would be better for everybody. USB IC + photoresistor is what is needed, but I don't have the skills to make such a device. I don't even know where to start
* Case - 3D printers could make *fantastic* cases for it, but places like Shapeways/etc are just too damn expensive in either the manufacturing cost or/and shipping. Alternatively Lego could be used, or perhaps even a small 'pill box' from a junkshop

### Software
* "Replace F.lux". Flux capability in combination with hardware assisted ambient light could be awesome, particularly with the control mechanisms (hotkeys)
* "Game mode" - detect game process launching, go to preset brightness/colour mode/etc
* Networked support - one sensor/server, push values across a network to other clients so you don't need multiple sensors in one room.

##Compatibility
MCCS and DDI/CI are interesting beasts with odd compatibility. *Most* LCDs these days should have support, even many CRTs over VGA did. However, combinations of drivers, cables and ports can effect this - for example, my U2711 is compatible over DL-DVI but if I switch to DisplayPort it can't find it. This isn't *just* with my app, but it seems the lower level Win32 API just loses the capability over certain combinations.

In this house, we only have a variety of Dell UltraSharp's with varying types of IPS panels. I can confirm it *does* work on U2410, U2711, and U2311. On Dell monitors, DDC-CI *may* not be enabled by default.

This will *only* work on Windows Vista and up - the specific Win32 APIs were not introduced until Vista.

##Warning
I have successfully 'nerfed' the software mode on my Dell monitors while developing this software (all colour modes disappeared), but nothing a factory reset didn't fix. While I'd caution you to *use at your own risk*, at this stage it's fairly safe.

[^1]: I use hack in the sense that it was something it was not designed to do.
[^2]: Bengals, man, bengals are the craziest of cats, and we've got a bengal kitten. He hasn't had any bones to chew for the last month, so instead he tried to chew on my arduino.
[^3]: Any of the analog ports are fine, A0 is just what I'm using in this example. If you change that, you need to change it in the sketch.