---
title: "Arduino, Android and the ECU"
excerpt_separator: "<!--more-->"
excerpt: "An Itea concept demonstration for ECall I completed back in 2013.  Connecting a car ECU with Arduino and sending information via Android device to 112."
description: "Interfacing Car Engine with Android via Arduino"
header:
    og_image: /assets/images/simplicity3.png
categories:
  - Projects
tags:
  - Arduino
  - Android
---
{% include toc title="Contents" icon="list-ul" %}

## Introduction

During a chat with a friend, interfacing Arduino wirelessly with Android subject came up and I talked about a related project that I did back in 2013. When I went back home, I searched my external disks for a while until I found the related files (at least most) and searched for my drawers for the tiny system that I had built.

The project is short enough, best to keep it documented on my blog for future references.

![Arduino Logo Image]({{ site.url }}{{ site.baseurl }}/assets/images/arduinologo.png)

## The Story

Back then, the company I used to work for contacted me for an [Itea](https://itea3.org/) demonstration on an extremely tight schedule. They asked me if I could complete it within the time frame. The request was something like the following;

"When a car accident happens, the car [ECU](https://en.wikipedia.org/wiki/Engine_control_unit) shall generate a specific signal through its RS485 interface. Build a system which grabs this signal, evaluates it, connects to an Android phone wirelessly and make a call to 112 emergency service"

*An ECU*
![ECU Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecu2.jpg){:height="300px" width="300px"}

This was for demonstration purposes so a simple working prototype was sufficient instead of an industrial design. But everything including the ECU were real, the system as described should really work (not a simulation) and the constraints were tight.

The project's name was [Amalthea](https://itea3.org/project/amalthea.html). What I was supposed to do was just a part of one of the demonstrations. To think of a solution, figure out & get the required hardware and implement it I had a bit more than 4 days. It was challenging enough so I accepted it and fun times ensued.

## Constraints

I don't want to dramatise the situation I was in especially since I really had a lot of fun dealing with it. But here was the situation;

* I had never worked with an ECU in my life nor was I going to be provided with one for my development or tests either. I was supposed to make it work without the required devices.

* I wasn't going to be provided with any other hardware, software or anyone to consult either. It just said something like "We provide this signal through rs485 port and here is how you should evaluate it and do the thing".

* The part I was supposed to do had very limited budget. Whatever hardware / software solution I came up with including the Android phone was supposed to be within the budget (and my payment too!).

* Due to limited time (4 days), I couldn't possibly buy online and import any hardware I might need, hardly even from another city within the country. I had to make do with what I had at home or at any local store.

* The solution had to be within the concept of creating an embedded [eCall](https://ec.europa.eu/digital-single-market/en/ecall-time-saved-lives-saved) solution inside the car. Also had to be interesting and innovative enough to present in front of Itea representatives. So I couldn't just use a PC, interface it with the ECU and call it a day.

* I figured it did not need to have any commercial concerns (such as security, efficiency etc.). It just needed to be effective within sterile demo conditions for a day or two. 

## Requirements

Back then, the internet didn't have as much information on Arduino, nor about some similar problem that I had. And I didn't know anyone IRL or online that could help me with it. I'm a software guy and an amateur when it comes to electronics. I knew programming PIC and Atmel, played around with MSP430 launchpad bullied a cute little Raspberry Pi with my evil experiments but never did a commercial project with any of them. I had no idea how to deal with RS485 part, nor how to test without an ECU.

Although the project was an eCall demonstration, the requirements I was asked for did not include the 140 bytes MSD (Minimum Set of Data) that needs to be transferred at the beginning of the call as described in eCall specs. (Although I worked on converting MSD to/from voice for a completely seperate project for a different company for the actual 112 service in Turkey)

*eCall*
![eCall Image]({{ site.url }}{{ site.baseurl }}/assets/images/realecall.png)

But all I needed to do was simply make a call and be done with it (No MSD). Roughly something like this;

![Simpe Ecall Image]({{ site.url }}{{ site.baseurl }}/assets/images/simplecall.png)

Where Input is an accident RS485 signal and the Output is a call made to 112. The Black Box should perform;

![Black Box Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecallchart.png)

## Components

I was probably going to use Arduino since it is very easy to deal with. As for wirelessly connecting with an Android phone, Wifi and Bluetooth seemed like ideas, never tried how XBee would work with Android. But wifi shield was a bit expensive so I just went with the bluetooth idea. I was going use a bread board and not bother soldering the parts.

1. Needed an Arduino.

2. To read the data, I figured I needed to somehow convert RS485 signal to RS232 for the Arduino.

3. Evaluating the data wasn't very important. The demo ECU was going to generate the same signal each time the button was pressed anyway. Just needed a very small piece of Arduino code.

4. Needed an Android phone with bluetooth and write an application that listens to bluetooth and make a call on demand.

5. Needed bluetooth capability for Arduino.

6. I did not have the ECU itself, so I needed to find a way to emulate it to test the system.

7. Cables, resistors, bread board and other small stuff.

### Arduino

I could use one of the Arduino Uno boards that I already had. But I wanted to give a try to Mega ADK to see if it could help me in any way. In the end it didn't matter.

*Arduino Mega*
![AMega Image]({{ site.url }}{{ site.baseurl }}/assets/images/ArduinoADK_R3_Front.jpg)

### Breadboard

Everything was going to be placed on this.

*Solderless Breadboard*
![Breadboard Image]({{ site.url }}{{ site.baseurl }}/assets/images/breadboard.png)

### RS232 to RS485

ECU interface was RS485. Arduino ports were RS232. I needed something between ECU & Arduino to convert the signal. After a brief research I found this;

*MAX 485 chip*
![MAX485 Image]({{ site.url }}{{ site.baseurl }}/assets/images/max485.jpg)

I was going to insert this into my breadboard, connect one side to Arduino and the other to ECU. This was going to be my adapter. Its price was next to nothing.

### Emulating ECU

Okay, I can read RS485 signals. But where do I find them without the ECU ? I went to my localstore and found this tiny and cheap gem.

*Dorji DAC23 Board*
![DAC23 Image]({{ site.url }}{{ site.baseurl }}/assets/images/dorjidac23.jpg){:height="300px" width="300px"}

This board basicly converts your USB port into RS485 port.

The plan was; 

* Insert DAC23 to USB port on my PC, identify it as an RS232 device
* Write a C# code to pump the required sequence of data to make it generate the RS485 signal that I need. 
* Then I could convert it back to RS232 via MAX485 to Arduino and perform my tests. 

Sounds a bit insane, but it worked great as an ECU emulator.

*Test Platform*
![Test Platform Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecutester.png)

* PC and DAC23 talk RS232
* DAC23 and MAX485 talk RS485
* MAX485 and Arduino talk RS232

Also, Arduino was connected to my PC via USB. So I read Arduino log output from PC console.

This is how it looked like during the RS485 tests. The black box is USB hub plugged into my PC with DAC23 on it with RX-TX (blue-yellow cables) going into MAX485 on mini breadboard, powering Arduino and receiving logs from the thick blue USB cable, white USB connected to my Android phone.

![DAC23 Tests Image]({{ site.url }}{{ site.baseurl }}/assets/images/dac23tests.jpg)

### Bluetooth

The parts above would deal with the ECU. And I needed bluetooth capability for Arduino so that it could send the event to make the call. After a brief internet search, i decided on this fella;

*HC06 Module*
![HC06 Image]({{ site.url }}{{ site.baseurl }}/assets/images/hc06.jpg){:height="300px" width="300px"}

That should do it.

### Android Phone

This was the only expensive component. If I had time to find a proper module, I wouldn't use a real phone. I just went to local store and purchased a Samsung Galaxy S3 Mini. It was a new one, I should have bought something second hand instead.

*Samsung S3 Mini*
![S3 Image]({{ site.url }}{{ site.baseurl }}/assets/images/sgs3mini_01.jpg){:height="300px" width="300px"}

## Putting It Together

These following are from the power point presentation I had prepared back then.

This is how the data flows through the system.

![Data Flow Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecalldataflow.png)

### Sequence Diagram

![Seqeunce Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecallseq.png)

### System Diagram

HC06 worked with 3.3 Volts, Arduino TX provided 5 Volts. So I used suitable resistors to dampen it.
I re-edited my old Fritzing drawing so it becomes more suitable to post in here, I might have broken a link or two in here. It has been years since I worked on this. 

![Drawing Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecalldrawing.png)

[Click here to zoom image]({{ site.url }}{{ site.baseurl }}/assets/images/ecalldrawing.png){:target="_blank"}

## The Arduino Code

The code written for these are very brief. Like I said before, I had to dig into my external drives to find related documents and code. I am not even sure if this was the final version or one of my test versions. Either way it shouldn't matter since the code portion really is easy to re-create in case of need.

Arduino code needed to;

* Listen to ECU
* Evaluate Data
* Make Call
* Stop

```cpp
int switchPin = 2;
byte makeCallArray[8] = {0x01, 0x06, 0x00, 0x01, 0x00, 0x01, 0x19, 0xCA}; //(Total 8-bytes)
byte receivedData[8];
int numBytesRead = 0;
int byteRead;
String strLog;

void setup() {

  Serial1.begin(19200, SERIAL_8N1);
  Serial.begin(9600);
  Serial3.begin(9600);
  
  // set the MAX485 to 'Receiver Output Enable' by switching its pin 2 to LOW
  digitalWrite(switchPin, LOW);
}

void loop() {

  while (Serial1.available() > 0)
  {
     byteRead = Serial1.read();

     if(byteRead >= 0) {
       strLog = "Data available ";
       strLog += String(byteRead);
       Serial.println(strLog);
       receivedData[numBytesRead] = byteRead;
       if (++numBytesRead == 8)
       {
         strLog = "Read 8 bytes";
         Serial.println(strLog);
         if (CheckBytes())
           MakeCall();
         numBytesRead = 0;
         break;
       }
     }
  }
  numBytesRead = 0;
  delay(100);
}

boolean CheckBytes()
{
 for (int i = 0; i < 8; ++i)
 {
   if (makeCallArray[i] != receivedData[i])  {
     strLog = "Bad data";
     Serial.println(strLog);
     return false;
   }
 }
 strLog = "Found match!";
 Serial.println(strLog);
 return true;
}

boolean MakeCall()
{
   strLog = "Making call to 112";
   Serial.println(strLog);
   Serial3.println("Make Call 112");
   for(;;);
  return true;
}
```

## Android & Tests

For the android application to work, I needed to pair HC06 with the phone first. If I recall correctly, HC06 pairing code was always 0000. So it could be paired right after the Arduino was powered up, which in turned powered HC06 on the bread board. The java code I wrote in Eclipse was only a few lines, checking up bluetooth socket for Make Call command, dial the parsed number and exit. During the tests, of course I used another number instead of 112.

As for the tester on PC, I had a very small C# code that simply writes (`System.IO.Ports.SerialPort.Write`) the signal sequence ({0x01, 0x06, 0x00, 0x01, 0x00, 0x01, 0x19, 0xCA}) to serial port (DAC23 attached) on a button click. 

## Final Product

This is how it ended up looking like :)
![Final Image]({{ site.url }}{{ site.baseurl }}/assets/images/ecallfinal.jpg)

[Click here to zoom image]({{ site.url }}{{ site.baseurl }}/assets/images/ecallfinal.jpg){:target="_blank"}

It is a bit dusty since it has been sitting in my drawers for over 4 years. The unplugged green and yellow cables are TX and RX for the ECU (or the DAC23 module during my tests)

## How It Turned Out

So after a few sleepless nights, I had decided it was ready to ship. It was a fun marathon of 4 days. In the morning I went to the office and the engineer from the Car company (Fiat) had arrived with a rather large ECU. We connected the wires, powered up the Arduino, paired the phone with HC06 and pressed a big red button next to the ECU and it WORKED on the FIRST try. This is something rare for those of us in our profession (software, I'm still a noob in hardware). Few days later I got the news that Itea demonstration went well too. Good memories that I wanted to share.

Sayanora,

Ayhan

## References

* [eCall](https://ec.europa.eu/digital-single-market/en/ecall-time-saved-lives-saved)
* [Amalthea Project](https://itea3.org/project/amalthea.html)
* [Arduino](https://www.arduino.cc/)
* [MAX 485 Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX1487-MAX491.pdf)
* [HC 06 Bluetooth Module](http://rasmicro.com/Bluetooth/HC%20Serial%20Bluetooth%20Products%20201104.pdf)
* [Arduino IDE for Arduino code](https://www.arduino.cc/en/Main/Software)
* [Eclipse for Android code](http://www.eclipse.org/)
* [Visual Studio for C# test code](https://www.visualstudio.com/)
* [Draw.io for diagrams](https://www.draw.io/)
* [Fritzing for electronic design](http://fritzing.org/home/)