---
layout: ll
---

#LimitlessLED Dev Documentation
{:toc}

##Talking to your lights
The wireless bridge communicates to the globes, and you communicate to the wireless bridge via <em>UDP</em>.<br />

##Ports
The original LimitlessLED bridges communicated on port *50000*, but with the current bridge this is now port ***8899***

##Commands
Each UDP packet/command is 3 bytes. 

>###Command, Value, Control
ie, 0x22, 0x00, 0x55

Whenever you are sending a command, you must send the three values. 

* The ***control*** value is always 0x55. 
* ***Value*** is most often 0x00, but when you are setting a colour, that ranges from 0x00 to 0xFF - that is, 0-255.
* ***Commands***  vary from globe type to globe type. "Turn globe on" for RGB (0x22) is different than for the white LEDs (0x35).

**Most commands need at least a 100ms delay between them, but up to 500ms may be needed.**

###RGB+W Commands

####Basic operations
Command | Operation
------------- | -------------
0x41 | RGBW Colour LED All Off
0x42 | RGBW Colour LED All Off
0x43 | Disco Speed Slower
0x44 | Disco Speed Faster
0x45 | Group 1 ALL ON
     |(SYNC/PAIR RGB+W Bulb within 2 seconds of Wall Switch Power being turned ON)
0x46 | Group 1 ALL OFF
0x47 | Group 2 ALL ON
     |(SYNC/PAIR RGB+W Bulb within 2 seconds of Wall Switch Power being turned ON)
0x48 | Group 2 ALL OFF
0x49 | Group 3 ALL ON
     |(SYNC/PAIR RGB+W Bulb within 2 seconds of Wall Switch Power being turned ON)
0x4A | Group 3 ALL OFF
0x4B | Group 4 ALL ON
     |(SYNC/PAIR RGB+W Bulb within 2 seconds of Wall Switch Power being turned ON)
0x4C | Group 4 ALL OFF
0x4D | Disco Mode     

####Setting colour to white
* GROUP ALL  0x42    100ms followed by:	0xC2
* GROUP 1    0x45	100ms followed by:	0xC5
* GROUP 2    0x47	100ms followed by:	0xC7
* GROUP 3    0x49	100ms followed by:	0xC9
* GROUP 4    0x4B	100ms followed by:	0xCB

###Setting colour
First set the group you want on (ie, 0x45 for  group 1), then 0x40 for the command and 0x00 to 0xFF for the colour (255).  ie, 0x45, 0xFF, 0x55 sets it to blue.

###RGB Commands
Command | Operation
------------- | -------------
0x21 |	All globes **off**
0x22 |	All globes **on**
0x23 |	Brightness up
0x24 |	Brightness down
0x25 |	Disco Speed Faster*
0x26 |  Disco Speed Slower
0x27 |  Next Disco Mode
0x28 |  Previews Disco Mode
0x20 |  Change Colour To Value. ie, 0x20, 0xFF, 0x55 sets it to blue.

###White Commands
Command | Operation
------------- | -------------
0x35 | All On
0x39 | All Off
0x3C | Brightness Up
0x34 | Brightness Down (There are ten steps between min and max)
0x3E | Warmer
0x3F | Cooler (There are ten steps between warmest and coolest)
0x38 | Zone 1 On
0x3B | Zone 1 Off
0x3D | Zone 2 On
0x33 | Zone 2 Off
0x37 | Zone 3 On
0x3A | Zone 3 Off
0x32 | Zone 4 On
0x36 | Zone 4 Off
0xB5 | All On Full (Send >=100ms after All On)
0xB8 | Zone 1 Full (Send >=100ms after Zone 1 On)
0xBD | Zone 2 Full (Send >=100ms after Zone 2 On)
0xB7 | Zone 3 Full (Send >=100ms after Zone 3 On)
0xB2 | Zone 4 Full (Send >=100ms after Zone 4 On)
0xB9 | All Nightlight (Send >=100ms after All Off)
0xBB | Zone 1 Nightlight (Send >=100ms after Zone 1 Off)
0xB3 | Zone 2 Nightlight (Send >=100ms after Zone 2 Off)
0xBA | Zone 3 Nightlight (Send >=100ms after Zone 3 Off)
0xB6 | Zone 4 Nightlight (Send >=100ms after Zone 4 Off)

###Syncronisation

##Example Code
####.NET  (C#)
Turn RGB lights on, cycle through all colours

	using (var udpClient = new System.Net.Sockets.UdpClient("192.168.1.255", 8899))
	{
		//this turns the lights on
	    udpClient.Send(new byte[] { 0x22, 0x0, 0x55 }, 3);
	
	     for (int i = 0; i <= 255; i++)
            {
            	//This cycles through the colours
                udpClient.Send(new byte[] { 0x20, Convert.ToByte(i), 0x55 }, 3);
                Thread.Sleep(500);
            }
	}

####Windows Phone (7.1 or higher) & WinRT (Windows 8)
Turn RGB lights on.

	var socket = new DatagramSocket();
	using (var stream = await socket.GetOutputStreamAsync(new HostName("192.168.1.255"), "8899"))
	{
		using (var writer = new DataWriter(stream))
		{
			writer.WriteBytes(new byte[] { 0x22, 0x0, 0x55 });
			writer.StoreAsync();
		}
	}
            
WinRT requires *Private Networks (Client & Server)* to be selected in the application capabilities if you want to communicate with lights on the same network.

##Third party apps/projects
* [CasualLight Voice Control of LimitlessLED Lights. by Joris studying at Waikato University, New Zealand.](https://chrome.google.com/webstore/detail/casuallight/kehomlifkcmfagnefddfmiiemeaiogfj)

* [Windows7/8 OpenSource LimitlessLED Application (commandline app written in Microsoft C#.Net). by Glen Balks from New Zealand.](https://github.com/qyiet/LimitlessLED)

* [HTML/Javascript LimitlessLED Application (ninjablocks)](https://github.com/jzGreen/ninja-limitlessLED)
 * [Youtube: NinjaBlocks/LimitlessLED and RFID](http://www.youtube.com/watch?v=n8gRqzbA9bs&feature=player_embedded)
  * [Youtube: Limitlessled and ninja blocks](http://www.youtube.com/watch?feature=player_embedded&v=AMaJZQns9Vc)

* [cloverleaf LimitlessLED Ruby github opensource by brandon](https://github.com/brandon-dacrib/cloverleaf/blob/master/app/helpers/limitlessled-rgb.rb)

* [LimitlessLED LightShow made in Perl programming language by Prof. Matt Carter from Bond University](https://github.com/hash-bang/Lightshow)
> provides a simple command line program to command the lighting system as well as also supports easy to program lighting macros – fake fire place lighting, time-of-day settings etc.
> Matt is also working on an XBMC/BoxEE plugin for media centers so that you can control the lights from your TV. Great work Matt, look forward to this one.

###Tasker
Now you can control your LimitlessLED lights on Android using AutoVoice and Tasker/Locale.

Jon Benson from Australia has built this great Tasker app.

Tasker stays in memory when launched once, so it displays almost instantly.
The current app (which simply allows sending UDP packets by acting as a plugin for Tasker/Locale) is free and will remain so.

Here is the Github webpage:
http://hastarin.github.io/android-udpsender/

and here is the Play Store link:
https://play.google.com/store/apps/details?id=com.hastarin.android.udpsender

To give you an idea of how it can be used, Jon has set this up so far…  

* Gesture control via my launcher (Nova) to turn all lights on/off/full bright/night mode and dim/brighten lights.
* Using the Scene shown above I can turn all/individual zones on/off and control bright/dim/warm/cool.
* NOTE: A swipe up/down in the colored square controls a bright/dim ramp and left/right for warm/cool.
* An NFC tag on my bed that when tapped will turn the lights off, or keep some in night mode if I have guests.
* When my phone connects to the Wi-Fi, and it’s between sunrise and sunset, my entry hall light is turned on.