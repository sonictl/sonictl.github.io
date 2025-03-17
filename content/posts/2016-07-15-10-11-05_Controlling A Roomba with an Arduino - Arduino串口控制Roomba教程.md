---
layout: post
title:  "Controlling A Roomba with an Arduino - Arduino串口控制Roomba教程"
date: 2016-07-15 10:11:05 +0800
tags: 
slug: p20160715101105
---

# Controlling A Roomba with an Arduino - Arduino串口控制Roomba教程







## [Controlling A Roomba with an Arduino](http://www.netfluvia.org/layer8/?p=127 "Controlling A Roomba with an Arduino")



 [arduino](http://www.netfluvia.org/layer8/?cat=18),[tech](http://www.netfluvia.org/layer8/?cat=6)
 [Add comments](http://www.netfluvia.org/layer8/?p=127#respond)



Jun
10
2009


 串口控制roomba， roomba编程
I set out this weekend to get an  [Arduino](http://arduino.cc) board to control my  [Roomba](http://www.google.com/url?sa=t&source=web&ct=res&cd=6&url=http%3A%2F%2Fstore.irobot.com%2Fproduct%2Findex.jsp%3FproductId%3D2804960&ei=qOktSo7dAovcMdnwnPMJ&usg=AFQjCNG5t0mKPd2nPNkiSCAvSbLoTWhgdw&sig2=nDxK4_jvNA4Av37PlU5p2g).  (The Roomba has a great – and generally open – interface, and  [iRobot](http://irobot.com/) deserves significant credit for encouraging creative repurposing/extensions of their products.)  I’ve got a few project ideas in mind, but for an initial step just wanted to verify that the [Arduino](https://so.csdn.net/so/search?q=Arduino&spm=1001.2101.3001.7020) could a) send control commands (“move forward”, “turn right”, etc.) from the Arduino, and b) read sensor data (“something is touching my left bumper”, “I’m about to fall down the stairs”).  This post contains my notes, which hopefully will help others doing this sort through some of the issues in a bit less time than that I spent.  


### The Cable


I’ve had  [the necessary cable](http://www.sparkfun.com/commerce/product_info.php?products_id=8243) (straight-through 8 pin mini-DIN) to connect to the serial control connector on the Roomba quite a while, and I’d already successfully cut a hole in the top of the Roomba’s cover to expose the port.  (I used a Dremel, though I suppose you could drill and file if you were careful and patient.)


I cut the cable in half and soldered male header pins to the correct wires to make a 2-pin power/ground connector and a 3-pin connector that had the serial RX/TX line as well as the DD (Device Detect) pin.  (There’s no reason the serial and DD lines*need* to be next to each other, but it made for a simpler connector, and it’s easy to just assign them proximate ports on the Arduino’s digital pin bank.)  Tod Kurt, author of[Hacking Roomba](http://www.amazon.com/Hacking-Roomba-ExtremeTech-Tod-Kurt/dp/0470072717/sr=8-1/qid=1163208746?ie=UTF8&s=books) and part of the impressive[ThingM](http://thingm.com/) studio, has[a basic photoset showing a similar cable](http://hackingroomba.com/2006/11/28/new-roomba-prototyping-cable-for-arduino/).  At this point, if you’re looking to do this yourself, you may be wondering which wires in the cable need to go to which pins of the connectors.  I have no idea if there’s reliable consistency in the colors used inside of mini-DIN cables, so I’d suggest figuring this out yourself with your specific cable and a continuity detector of some sort.  The pin diagram on page 2 of[iRobot’s SCI (Serial Command Interface) documentation](http://www.netfluvia.org/layer8/www.irobot.com/images/consumer/hacker/Roomba_SCI_Spec_Manual.pdf) should be all you need.[![Arduino Roomba Cable](http://farm4.static.flickr.com/3612/3610053602_b2cc97f42d.jpg?v=0 "Arduino Roomba Cable")](http://www.flickr.com/photos/jrheling/3610053602/in/photostream/)


![](https://img-blog.csdn.net/20150902163621545?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 but,  
  ![](https://img-blog.csdn.net/20150902163749551?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### Powering the Arduino


The power connector from the Roomba is carrying +16V, which is more than enough to power the Arduino (I’m using the Diecimila and Duemilanove versions of the board).  According to[the docs](http://arduino.cc/en/Main/ArduinoBoardDuemilanove), this is within the acceptable input range, but outside of the recommended range.  In my early attempts to hook it up, I seem to have fried a Diecimila board by connecting Roomba’s +V line to the ‘Vin’ header on the Arduino (sometimes labeled ‘9V’).  I got it working by running the Roomba’s +V line through a 5V voltage regulator and then directly into the ‘5V’ header on the Arduino (note:  it’s important to*only* do this with a regulated supply, since the 5V header bypasses the Arduino’s onboard regulator).  This worked fine, even if it does mean that the minimal setup is no longer just an Arduino and a cable.  After I fried the Diecimila, I switched to a Duemilanove, though to my knowledge there is no relevant difference in terms of power requirements or tolerance.


### 


### OK, It Should Work Now


Based on the research I’d done, I expected that things would more or less just work at this point.  For the software side of my proof of concept, I was using[Tod Kurt’s RoombaBumpTurn sketch](http://roombahacking.com/software/arduino/RoombaBumpTurn.pde).  The sketch sends the Roomba forward, then checks the left and right bump sensors in a loop, turning ‘away’ from the objects it collides with.


I was excited on my first attempt to see the Roomba ‘wake up’ shortly after connecting my cable to the board.  Then:  nothing.  Hmm.


By default, the Roomba’s SCI talks at 57000 bps, and the code was assuming that data rate.  I didn’t recall having changed it during previous times hacking around with the Roomba, but just in case I changed the serial speed to 19200 and, sure enough, the Roomba jolted forward shortly after powering up the Arduino.  OK, cool, so there was at least basic connectivity.


I didn’t really have a reason to run at the slower serial speed, so I threw a quick one-time change into the setup() function to reset the baud rate back to 57000:  
 



```
// [re]set baud rate to 57600
sciSerial.print(129, BYTE);  // BAUD
sciSerial.print(10, BYTE);   // 57600
delay(100);
```

### \*Now\* It Should Work, Really


With basic serial connectivity established, I went on to verify that basic bumping and turning behavior worked as expected.  In short, it didn’t.  Most of the time letting the Roomba collide with something (or even just manually pressing the sensor) did nothing;  perhaps 10% of the time it would cause the Roomba to turn.  In order to verify that I really was getting commands to the Roomba, I added a short sequence of actions called from the setup() method:  
 



```
void dance() {
  xbSerial.println("top of dance()");
  updateSensors();
  goForward();
  delay(1000);
  goBackward();
  delay(1000);
  stopMoving();
}
...
void stopMoving() {
  sciSerial.print(137, BYTE);   // DRIVE
  sciSerial.print(0x00,BYTE);   // 0x0000 == 0
  sciSerial.print(0x00,BYTE);
  sciSerial.print(0x00,BYTE);
  sciSerial.print(0x00,BYTE);   // 0x0000
}
```

The Roomba jogged forward, and backward, just as I’d expect.  This left me pretty much convinced that sending commands was working fine, and that my problem was in reading sensor values.


At this point I really started to feel the lack of debugging information.  While I was pretty sure my problem was in sensor reading, it was certainly time to validate my overall expectations about program flow, as well as inspect return values from the sensor checks, etc.  Debugging is always more of a challenge on an embedded platform with no display/keyboard, but my typical answer for this when using the Arduino — “I’ll just println() to the serial port and monitor it over USB” — doesn’t work here, for two reasons.  First, the serial port is being used by the connection to the Roomba.  As if that weren’t enough (and it is), the Roomba is driving across the floor, and I wouldn’t want to chase after it with my laptop.


### Remote Wireless Debugging with XBee


Fortunately, there is a good answer to debugging the code running on the Arduino sitting on top of my Roomba as it scoots away from me: [Digi’s XBee modules](http://www.digi.com/products/wireless/point-multipoint/xbee-series1-module.jsp).  I’ve been playing around with these for a different project recently, and at a basic level they provide an easy wireless link with a serial interface.  Interfacing with the Arduino is a snap with the[XBee adapter kit](http://www.adafruit.com/index.php?main_page=product_info&cPath=29&products_id=126) from the outstanding  [adafruit industries](http://www.adafruit.com) ([sparkfun](http://www.sparkfun.com/commerce/categories.php) also has[a breakout board](http://www.sparkfun.com/commerce/product_info.php?products_id=8276) I’ve used with success, but it requires a bit more of a surrounding circuit — nothing tricky, but not as easy as adafruit’s).


### 


The attentive reader might now be wondering:  I just explained that my serial port was already spoken for, and the XBee module has a serial interface.  So how does this really solve my problem?  Enter[NewSoftSerial](http://arduiniana.org/libraries/NewSoftSerial), an updated version of the Arduino software serial library, which basically lets you drive a serial interface from any two digital I/O pins on the Arduino.  [Yes, I could also use this to debug directly over a serial line, which would be an option if you didn’t have a pair of XBee modules and didn’t mind potentially chasing after your Roomba.]  I had a pair of XBee modules already configured to talk to each other (details of that are far outside the current scope).  One module got connected to a couple of pins on the Arduino, and the other to my laptop via[the trusty FTDI USB TTL-232 cable](http://www.adafruit.com/index.php?main_page=product_info&cPath=33&products_id=70).  On the laptop I then ran a simple serial terminal with ‘screen’ (any terminal program would do), and*voila* — I can println() from the vacuum cleaning scuttling across the floor and see it on my laptop from across the room (or halfway down the block, for that matter).


With basic visibility established, I was able to dig further into what wasn’t working with regard to sensor readings.   The sensor reading routine in RoombaBumpTurn is pretty straightforward:


 



```
void updateSensors() {
  Serial.print(142, BYTE);
  Serial.print(1,   BYTE);  // sensor packet 1, 10 bytes
  delay(100); // wait for sensors
  char i = 0;
  while(Serial.available()) {
    int c = Serial.read();
    if( c==-1 ) {
      for( int i=0; i<5; i ++ ) {   // say we had an error via the LED
        digitalWrite(ledPin, HIGH);
        delay(50);
        digitalWrite(ledPin, LOW);
        delay(50);
      }
    }
    sensorbytes[i++] = c;
  }
}
```

After adding some basic println() breadcrumbs (not shown here), it quickly became clear that this function simply wasn’t returning a value that suggested it had correctly read the bump sensors.  The clearest indication of this was when I added some code to check the value of i at the end of updateSensors().  The SCI doc is pretty clear — you ask for sensor packet 1, you get 10 bytes back.  However, I was consistently getting 2 bytes, or 4 bytes, or 3.  Once in a while I’d get 10 bytes, but it was rare.


This is good.  This is progress.


### Other People’s Problems


I googled for notes from others doing similar things, and found a number of examples.  Actually, I found a surprising (but at this point encouraging) number of people having problems reading Roomba sensors from Arduinos.  Some of the closest matches were:


* “[Roomba SE Receiving Serial Data](http://www.robotreviews.com/chat/viewtopic.php?f=4&t=11269)” on the Robot Reviews forum  
 A long thread, most of which is exploring a possible hardware inconsistency between the Roomba’s serial signal and Arduino’s serial port.  The original poster is also working with the RoombaBumpTurn code, it seems, but near the end of the thread it spontaneously starts working, with no changes having been made (huh?).
* “[SERIAL COMMUNICATION NOT WORKING!!!](http://www.robotreviews.com/chat/viewtopic.php?f=4&t=10744&start=20)” on Robot Reviews  
 This one starts out with a similar problem description, but concludes that the Arduino Diecimila (the previous generation) works with the Roomba, while Duemilanove (the current) doesn’t.  I am certainly not a hardware guy, but from my understanding of the differences between these boards that doesn’t sound likely.
* “[Software Serial](http://www.arduino.cc/cgi-bin/yabb2/YaBB.pl?num=1236713062/4)” on the Arduino Forum  
 A relatively short thread, this starts out referencing the hardware issues speculated on in the first thread, and concludes with the original poster successfully getting sensor readings after switching the Arduino<->Roomba serial connection to use a software serial driver rather than the Arduino hardware.


I also encountered a few posters who noted that they’d had different amounts of success by changing the amount of time the code waits after sending the sensor read command to the Roomba and starting trying to read results.  The default in RoombaBumpTurn is 100ms, one person noted that 64ms seemed to work, and I found I’d get more readings (roughly) the lower I set the delay, but I’d also get more phantom readings (i.e. where the Arduino thought a bump sensor had been activated when none had).  In any case, regardless of the duration of the delay I never anything close to accurate, and never was able to consistently see 10 bytes of data coming back from the Roomba.


### Solved!


The theme of inconsistent serial interfaces between the Roomba and the Arduino was too prevalent to ignore.  I was skeptical about it largely because, as I understood the hypothesis, it should have meant that no Roomba-controlling Arduino code connected via the built-in hardware serial interface would work, and there are a number of hints (including, primarily, the RoombaBumpTurn code) that it was working for some people.  After more or less running out of other things to try, however, I decided to give it a shot, and switched my hacked-up copy of RoombaBumpTurn around so that it expected the XBee to be connected to the hardware interface and used NewSoftSerial to connect to the Roomba SCI port.


### 


And that did it.  Immediately, everything worked.


### In Conclusion


I think the good folks in the  [Robot Reviews forum](http://www.robotreviews.com/chat/viewtopic.php?f=4&t=11269) have nailed the issue, and that the current levels produced by the Roomba are too low to consistently register on the Arduino’s UART.  I can’t explain how that is consistent with the “now it works .. I don’t know what I changed” stories I found in a few places, or how anyone has consistently controlled Roombas from the Arduino’s hardware serial port.  In addition to using a software serial interface, user kg4wsv on the Arduino forum suggests a pair of back-to-back TTL signal inverters to buffer and bolster the current on the signal coming from the Arduino;  I haven’t tried that yet, but it could be a useful alternative if your program was going to use frequently talk over the SCI connection (as software serial is relatively expensive, computationally).


With all of this sorted out, my Roomba is now running the simple bump & turn algorithm (I added a few pauses to make it easier to see exactly what sensor readings the code got).


  
 


=====================


also refer:  [Serial port connection and configuration](http://www.robotappstore.com/Knowledge-Base/3-Serial-Port-Baud-Rate-Configuration/17.html)


  
 




