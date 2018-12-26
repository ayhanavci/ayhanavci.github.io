---
permalink: /works/
title: "All past projects"
author_profile: true
breadcrumbs: true
---
{% include toc title="Project List" icon="list-ul" %}

## Introduction

The list of most of the projects I have worked on starting from year 2000 till the end of year 2017 in various companies. I have decided to document them so there can be a reference when I'd like to look back, perhaps while talking to collegues. The actual project names will not be displayed here openly and the description will be very brief. In some projects I had additional roles such as Lead Dev, PM, Analysing, Designing, Testing etc. But in all projects, I have actively participated in coding. My [Tumblr page here](http://ayhanavci.tumblr.com/) describes my roles in projects.

## Occupational Safety and Health

Web based ERP & Decision making application for occupational safety specialists and health personnel.

Features:

---

Employee management, Task management, Work Planner, Calendar Planner, Subcontractor management, Risk Management, Monitoring, Measurement, Accident reporting, Patient management and all the other modules needed for enterprise level job safety & health management for chain customers.

Technologies:

---

C#, Javascript, .NET, MVC, Devexpress, Xml, MSSQL Server

## E-Invoice, E-Archive Portal

Accounting platform with e-invoice, e-archive, e-book integrated with e-State (e-Devlet).

Features:

---

High availability e-Invoice web platform. Save, send, create, view, query status, reject, accept, report, export, archive, print invoices and accounting books online.

Technologies:

---

C#, Javascript, .NET, MVC, Devexpress, Xml, MSSQL Server

## Airline Operation Center & Air Traffic Control

Aircraft and Airline Operations management system. Airline Traffic Control Application, Pilot Application, related support services.

Features:

---

* Eurocontrol / FAA standards compliant.
* Support for roles: Crew Control, Maintenance, Dispatch, Ramp Control, Met Office, Flight Safety Office, DG Specialist, OOC Manager.
* SIP based.
* Interactive Map, showing all aircraft of the airline company, Radar, GPS integration.

Technologies:

---

C++, C#, .NET, WPF, SIP, Windows Azure, NT Services, MSSQL Server

## ECall MSD Parser

An ECall supporting car modem sends a data called Minimum Set Of Data. The minimum set of data contains information about the incident including time, precise location, vehicle identification, eCall status (as a minimum, indication if eCall has been manually or automatically triggered) and information about a possible service provider.

ECall MSD Parser analyses the voice packages and exposes this minimum set of data.

Features:

---

* Analyses g711 voice packages.
* Raises an event when it detects MSD.
* Prepares a MSD data structure for use.

Technologies:

---

C++, Voice modem, Car ECU, GSM, voice codecs

## Html5 Video & Audio Call

HTML5 web based video&audio call integration to existing call centers. Doesn’t use any invasive plugins (nor RIA). Customers may connect to agents via their browsers and video & audio & text chat or conference call.

Features:

---

* Utilizes new HTML5 standards for video & audio calls.
* Live Video, audio call and text transmission between participants.
* Call center integration via sockets or database.
* Server side signalling control over clients.
* Conference call up to 4 participants in a single room.
* Call Recording (under development)

Technologies

---

HTML5, Javascript, C++, Node.js, javascript, WebRTC, getusermedia, HTML5, MSSQL Server

## SIP Proxy Server

Multipurpose - Multiplatform SIP IP telephony server.

Features:

---

* Sip Registrar / Authentication server
* Regular Expression Based Sip Dial Planner
* Sip Event BLF / EventList BLF server
* FailOver Sip Server Clustering
* Sip Traffic Logger
* Compatible with all SIP devices & applications.
* Custom Sip Parser

Technologies:

---

SIP, C++, TCP/IP, Thread&Socket Pooling, Boost Library, MSSQL Server, WinAPI, NT Services

## SAP Telephony Integration

Lightweight implementation of SAPphone and related SAP ICI CRM services. 

Features

---

* Three way bridge between SAP, SIP SoftPhone and SIP Telephony Server.
* Implementation of PhoneCall, PhoneLine, Container, System, Dispatcher, Item, User and Root services of SAP.
* Full support of SAPphone features.
* Works as the telephony engine for SAP.

Technologies:

---

C++, C#, .Net, SAP, Web Services, IP Telephony, SIP, NT Services, MSSQL Server

## e-State Kiosk

Kiosk & Server management & maintenance system & applications used as a backbone for Turkey’s E-State system.

Features:

---

* Integration with various Turkish government software systems.
* Running on outdoor kiosk, handling physical and cyber security measures.
* Supporting Camera, Proximity, Temperature, Pressure sensors, GPS features
* Heavy logging
* Embedded web browser

Technologies:

---

C++, C#, Kiosk hardware, Sensors, MSSQL server

## ECall Car Hardware & Software

Car accident emergency call maker. Microcontroller integration to car engine control unit which starts an emergency call to 112 service by connecting to driver’s smartphone via bluetooth in case of an accident. This is a part of an ITEA2 experimental project. Project has been demontrated to ITEA2 board and has passed.

Features:

---

* In case of car accident, reads the corresponding data from ECU (via rs485)
* Checks the bitstream. If it matches, connects to bluetooth unit and sends signalling data to android phone.
* Application on the phone receives the command and initiates emergency call and sends accident & car details.
* Runs on any Atmega microcontroller.
* Runs on any android phone with bluetooth.
* Custom electronic design. Documented with schematics.

Technologies:

---

Processing language, Java, C++, Atmel microcontrollers, Bluetooth, Android, Engine Control Unit, GSM

[ECall Official](https://ec.europa.eu/digital-single-market/ecall-time-saved-lives-saved)

## Mobile Interactive Healthcare

Mobile hospital application & server for medical personal. 

Features:

---

* Hospital Department based.
* Chatrooms & chatroom management.
* User Management
* Image sharing
* Multicolor live collaborative drawing & noting support on medical images.

Technologies:

---

Java, C#, Javascript, HTML5 Node.js, Android, WPF, Html5

## Meeting Rooms

Information screens for company meeting rooms. Displaying & automatically updating all scheduled current & future meetings along with other useful data.

Features:

---

* No server application nor any management required. Data is retrieved automatically from exchange server. Any application can be used to set meetings.
* Smooth design.
* Meeting status, schedules, participants, leader are displayed.
* Company news are displayed. 

Technologies:

---

C#, MS Silverlight, Silverlight, Exchange Server & API, Embedded hardware.

## Arduino Alert Server

Small college project when Arduino was a new thing. Project consists of black boxes placed in desired locations so they can periodically feed sensor data to the server through wireless/wired network. Authorized users can watch the live data and an admin can set alarm tresholds and assign email addresses to the alarms. The background services notifies the users in case of over/under heat/pressure situations.

Features:

---

* Black box with Arduino, various shields and sensors inside.
* Temperature and pressure sensors.
* Administrators can set alarm tresholds. System sends notification in case of treshold breach.
* Authorized users can watch live data.

Technologies:

---

Processing, C#, ASP.NET, Arduino, Sensors

## Water Splash Kiosk

This project was for a summer campaign of the largest paper tissue company in Turkey. Project was something like this; various outdoor kiosks placed in crowded places in Istanbul. The kiosks had interactive software running as well as various sensors and a cold water splashing mechanism. People could go and activate the kiosk, playing games on it etc. When triggered,the kiosk would spray cold water on its users!

Features:

---

* Outdoor kiosk with various hardware sensors such as proximity, camera, temperature, gps etc.
* A backend software dealing with hardware, with the server and communicating with the frontend software.
* Games and ads running on kiosk, users can trigger water splash to cool themselves during hot summer days and then dry themselves using the provided paper tissue.

Technologies:

---

C#, C++, Flash, Sensors, Micro controllers, MSSQL server

## Fax Server

A soft fax server integrated with CTI applications

Features:

---

* Can communicate with fax hardware as well as other fax software
* Works with IVR
* Supports scripting

Technologies:

---

C++, SIP, Fax, CTI, MSSQL Server

## Softphone Android

Android softphone to compliment company's CTI environment (IVRs, Call Centers etc)

Features:

---

* Native softphone app that can be configured and controlled remotely
* Supports industry standarts while being tightly coupled with company specific software habitat.

Technologies:

---

Android SDK, Java, Sip

## Softphone Module

Activex DLL, native DLL, Lib softphone module for company's partners.

Features:

---

* Softphone Library and working softphone component for partner developers
* Fully supports SIP standarts including optionals
* Tightly coupled with company software habitat
* Supports G.711, G.729, G.723, GSM, SPeex codecs
* Runs on all web browsers, interfaces to work with all programming languages.
* Default Softphone app using the library is also provided

Technologies:

---

C++, ATL, COM, SIP, ActiveX

## Cisco IP Phone Information Services

Information XML web server for Cisco IP Phone XML services. Weather Forecast, Stock Exchange, Local and Global News, Phone Directory services for Cisco IP Phones.

Features:

* Supports all Cisco IP Phones with a menu screen.
* Weather Forecast, Stock Exchange, Local and Global News for all users.
* Authentication based Phone Directory access.
* Phone model awareness. Sends suitable XML depending on the phone model.

Technologies:

---

C++, C#, Cisco XML Services, Cisco Call Manager, Cisco IP Phones, Web Service, NT Services

## Listener For Cisco IP Phones

Cisco XML service and voice processing server to listen to any Cisco IP phone without any phone calls, just with XML services.

Features:

---

* Can forcefully start listening to environment of a specific IP phone without any need to phone hookup nor make any calls.
* Enabled authentication levels. Flexible assignment of access rights to determine who can listen who.
* Can work IP unicast to directly record.
* Can work IP multicast to record & listen & feed the voice to a third participant.
* Can record the voice to a server, browse the records after.

Technologies:

---

C++, C#, Cisco XML Services, Cisco Call Manager, Cisco IP Phones, Web Service, NT Service, MSSQL server.

## Environment Control Server

Reads temperature, air pressure, air flow, humidity, voltage, fog, vibration, flood data from any modbus enabled sensor and stores the information on the server. The data is used to trigger any user defined alert via email or sms.

Features:

---

* Supports all Modbus enabled sensors.
* Easy to use interface.
* Sms & Email alert mechanisms.
* User interface for sensor data reports.

Technologies:

---

C++, Sensor devices, MSSQL server

## Application Server for Nortel IP Phones

VNC server & Application server for VNC based Nortel IP phone prototypes, enabling a set of information services, messaging & voice services, IVR integration.

Features:

---

* Works with beta Nortel phone that runs experimental VNC client.
* Custom VNC server implementation
* Has Weather, Booking, Stock Exchange, News feed, Music streaming services displayed on the phone

Technologies:

---

C++, C#, VNC, Nortel IP Phones, XML services

## Cluster Management Service

Custom windows service built on Windows Clustering services. Manages and automates server applications on deployment.

Technologies:

---

C++, Windows NT services

## Push To Talk

Cisco XML services based push to talk service for Cisco IP Phones.

Features:

---

* Custom service using IP Multicast and IP Unicast as opposed to using SIP or H323 and RTP.
* Voice Chat servers, Voice chat groups for different departments.
* Administration and communication on phone menus (using Cisco XML Services).
* Phone call management, snooping on other calls for authorized personnel.

Technologies:

---

C++, C#, Web Services, Cisco IP Phones XML services, CCM, CCME, IP Multi&Unicast MSSQL Server

## Voice Mail Server (H323, TAPI)

XML Service & Voice Mail service working standalone, with cisco call manager and/or with cisco call manager express. Works with Cisco IP Phones. Integrated with cisco XML Services.

Features:

---

* Users can access the system and listen, forward, delete voice messages.
* H323 & Tapi support.
* Voice mail options access via XML services. Integrated with Cisco IP Phones. 
* Voice mail options access via IVR menu. 
* Callers can leave voice messages via various triggers.
* Script based business flow.
* Interprets scripts.
* Supports voice mixing.
* Call Logging, Recording.
* Supports all call features & common codecs.

Technologies:

---

C++, TAPI, H323, Cisco Call Manager, Web Services, Cisco Call Manager Express

## IP Multicast Voice Conference Server

An XML web service & telephony server that deals with conference calls and its management using IP Multicast instead of traditional telephony protocols. Service has an authentication enabled user interface on Cisco IP Phones, available to defined users.

Features:

---

* Signalling & Voice conference server using IP multicast.
* Dial, Ring, Answer, Reject, Busy, Transfer call control features all performed in a custom fashion, integrated with Cisco Phone user interface.
* Join conference, Leave conference, Add user to conference features.
* Cisco IP phones & Cisco Call Manager integration.
* Uses Cisco Unified IP Phone XML services.
* Authenticates users either from desktop application or with Cisco IP Phone XML applications.
* Maps calls to available IP multicast IP / Port range.
* Conference rooms with administration rights.
* Users with rights can forcefully add / remove users into / from the room with the application control over Cisco IP Phones.
* Runs as XML Web service and a NT Service.

Technologies:

---

C++, C#, IP Multicast, Routers, Cisco IP Phones, Cisco Call Manager, MSSQL server, Web services, NT services.

## H323 IVR Server

Highly Customizable H323 Voip IVR server compatible with cisco devices.

Features:

---

* H323 support.
* IVR & Call Center.
* Compatible with cisco ip phones / call manager express.
* Supports all call features & common codecs.
* Supports:
  * Supports voice mixing.
  * Call Logging, Recording.
  * Scenario script based.
* Can handle up to 1000 concurrent calls in a single server.
* Feature rich custom script language

Technologies:

---

C++, Cisco IP Phones, CCME, CCM, NT Services, H323, MSSQL Server, Scripting

## Storage Room Kiosk

Used in storage rooms to automate the stock management. Ergonomic design for easy barcode reading.

Features:

---

* Stock automation. Data integrated with web applications that can be accessed anywhere.
* Authentication levels based data & gui access.
* Reads barcodes.
* Prints various instruction bills for use of business logic.
* Email & call alert mechanism for various stock triggers.

Technologies:

Hardware sensors, outdoor kiosks, C++, C#

## Shopping Mall Information Kiosk

Typical interactive shoping mall kiosk application with maps, ads, sensors, logging, remote management.

Technologies:

---

Hardware sensors, outdoor kiosks, C++, C#

## Credit Application Outdoor Kiosk

An application running on a custom design banking kiosk hardware.

Features:

---

* Voice & Video based customer guidance.
* Detects presence with proximity sensors & activates.
* Takes photo of the customer from different angles, scans signature of the customer from a pad, scans customer’s ID card and approves online credit application.
* Supports standart banking features, accepts & delivers banknotes in 10 different currencies.

Technologies:

---

Hardware sensors, C++, C#, Web services

## Complaint Management

Web Based CMS of an oil company. Customers can leave complaints and can keep track of them. Complaints are queued and are handled by agents to be delivered to related managers.

Features:

---

* Customers can get online and choose from multiple categories.
* Customers can keep track of their complaints, optionally may receive an automated email or can get a phone call from an agent.
* Complaints are saved into database and can be retrieved in a user defined way.

Technologies:

---

Asp.NET, C#, MSSql server

## Visa & Mastercard authenticator (MPI)

Merchant plugin which authenticates to both Mastercard and Visa, given to customers to use in their web pages.

Technologies:

---

C++, C#, .NET

## Virtual POS

3D secure integration module for banks and highly secure Virtual POS server handling all transactions of bank customer companies. 3 of the largest 10 banks in Turkey has been using the product for over 15 years.

Technologies:

---

C++, MSSQL Server

## Voice Mail Server (Analog & ISDN)

Voice mail server. Enables leaving call messages and user access based operations on them.

Features:

---

* Users can access the system and listen, forward, delete voice messages.
* ISDN (BRI, PRI), SS7, Analog support.
* Supports all call features & common codecs, voice mixing.
* Compatible with Aculab & Dialogic boards.
* Autodetects supported call control hardware.
* High performance, low resource consuming. (No library used other than base hardware vendor api)
* Script based scenerio.

Technologies:

---

C++, Aculab, Dialogic, ISDN PRI & BRI, DSP, SS7

## IVR with multi telephony board support

Highly Customizable IVR server application.

Features:

---

* ISDN (BRI, PRI), SS7, Analog support.
* IVR & Call Center.
* Supports all call features & common codecs.
* Compatible with Aculab, Brooktrout, Dialogic boards.
* Autodetects supported call control hardware.
* High performance, low resource consuming. (No library used other than base hardware vendor api)
* Scenario script based.
* Speech recognition, voice authentication.

Technologies:

---

C++, Aculab, Dialogic, Brooktrout, ISDN PRI & BRI, DSP, SS7

## IVR Scenario Drawing Tool

Graphical application for easier IVR script generation with drag drop boxes.

Technologies:

---

C++