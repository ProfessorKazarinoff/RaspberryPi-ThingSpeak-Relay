# RaspberryPi-ThingSpeak-Relay
A student project in ENGR114 at Portland Community College that uses a Raspberry Pi connected to ThingSpeak.com to flip on and off a relay

## Problem Statement: 

Our group was tasked with taking the light relay project constructed by another group last quarter, and making it wireless so that it was more practical to use. We needed to be able to use either a Raspberry Pi or Arduino Board to connect to the WiFi and be able to control when the light turns on and off without having to be connected to the relay system.

## Hardware Setup

### Bill of Materials:

Part Name
Vendor
Item Description
URL
Price Approx.
Raspberry Pi 3 Model B Motherboard
Micro Center
Python Coded
Shop Link
$29.99
($34.99 Regular)
SparkFun Electronics Breadboard and Arduino Holder
SparkFun
Base to hold the breadboard
Shop Link
$3.95
Breadboard
SparkFun
Connector
Shop Link
$4.95
SparkFun Beefcake Relay Control Kit
SparkFun
Relay
Shop Link
$7.95
Resistor (1kΩ 5%)
SparkFun
Resistance
Shop Link
$0.95 (For 20)
Rectifier Diode 10A
CHANZON
Prevents short circuit
Shop Link
$6.45 (For 10)

### Hardware Hookup Guide:


## Code: 

Matlab Code (from a .m file) - 
function [command] = relay()
%RELAY Turns light on and off on command
%   Relay sends the command to ThingSpeak to turn on light or off

command = input('Would you like to turn the light on or off? (enter "light_on" or "light_off"): ', 's')
if ~(strcmp(command,'light_on') | strcmp(command,'light_off'))
    error('func must be either "light_on" or "light_off"')  
end

% TalkBack app ID
TalkBack_ID = '23497'; 
% TalkBack command ID
Command_ID = '1322093'; 
% TalkBack app API key
TalkBack_apikey = '0CXH5JFXV72M2BHO'; 
url =  strcat('https://api.thingspeak.com/talkbacks/',TalkBack_ID,'/commands.json');
response = webwrite(url,'api_key',TalkBack_apikey,'command_string',command)

if (strcmp(command,'light_on'))
    webwrite('https://api.thingspeak.com/update?api_key=XXXXXXXXXXXXXXXXXXX&field1=0','0')
end
if (strcmp(command,'light_off'))
    webwrite('https://api.thingspeak.com/update?api_key=XXXXXXXXXXXXXXXXXXXXXX&field1=1','1')
end
end

Python Code (from a .py file) - 
#!/usr/bin/env python
from urllib.request import urlopen, Request
import json,time
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)
GPIO.setup(4, GPIO.OUT)
while True:
    request = Request('https://api.thingspeak.com/talkbacks/23497/commands/execute?api_key=XXXXXXXXXXXXXXXXXX')
    response = urlopen(request)
    command = response.read()
    command = command.decode()
    command = command
    print(command)
    if command == 'light_on':
        GPIO.output(4,GPIO.HIGH)
    if command == 'light_off':
        GPIO.output(4,GPIO.LOW)
    time.sleep(5)  


## Results:

We used a Raspberry Pi so that we could code using Python and connect it to the WiFi. After it’s connected, we could relay through ThingSpeak in order to use a Matlab function to turn the light on or off. 




## Future Work:

Currently, the wireless light relay requires a user input to turn the light on or off, which could possibly be automated by connecting the Raspberry Pi to the WiFi and communicating with ThingSpeak in order to have the lights turn on and off based on when it’s morning and light. The times for this could be collected using data online providing when sunrise and sunset occur each day.

### Licence

GNU General Public Licence 3.0
