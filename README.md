# IoT BASED SMART HOME
An IoT Based Smart Home Model developed using cisco, consisting of features like remote control access, intrusion detection system, fire control, theft detection and IOT based smart appliances.
## FEATURES
 • 🏠 Designed a smart home system that integrates multiple IoT devices for automation and monitoring. <br>
 • 🕵🏻‍♀️ Implemented an intrusion detection system using motion and window sensors.<br>
 • 👩🏻‍🚒 Developed a fire response mechanism using smoke detectors and timed sprinklers. <br>
 • 💻 Provided remote access and control over home appliances and security features. <br>
 • 🌡️ Monitoring environmental parameters and automate responses (e.g., humidity, temperature).  <br>
 
## Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Equipment](#equipment)
- [Project Review](#project-review)
- [Code Block](#code-block)
- [Installation](#installation)
- [Implementation](#implementation)
- [Disclaimer](#disclaimer)

## TECH STACK
![Static Badge](https://img.shields.io/badge/Cisco-blue?style=plastic) <br>
![Static Badge](https://img.shields.io/badge/Python-grey?style=plastic&logo=python) <br> 

## EQUIPMENT
The major equipment that I have develop this simulated model are: <br>
1-MCU <br>
2- MOTION DETECTORS. <br>
3- SMOKE DETECTOR.<br>
4- FIRE SPRINKLER.<br>
5- RELAY MODULES. <br>
6- WATER LEVEL MONITORS. <br>
7- COAXIAL CABLE SPLITTER. <br>
8- MODEM. <br>
9- SWITCH. <br>
10- WEBCAMS. <br>
11- SMART BULDS AND APPLIANCES. <br>
12- BUZZER. <br>
## PROJECT REVIEW

![Smart Home Dashboard](https://github.com/user-attachments/assets/e84f4106-5274-49c7-afa6-ff4af0502a68)   <br>
![Smoke Detector System](https://github.com/user-attachments/assets/5f63048c-aba8-43e0-a829-15c2c81724cc) 

## CODE BLOCK
MCU3 code for fire monitoring
```bash
from gpio import *
from time import *

def handleSensorData():
	value = digitalRead(0)
	if value == 0:
		customWrite(1, '0')
	else:
		customWrite(1, '1')
	
def main():
	add_event_detect(0, handleSensorData) 
	
	while True:
		delay(1000)

if __name__ == "__main__":
	main()
```
BR3 Smoke Detector
```bash
from gpio import *
from time import *
from ioeclient import *
from physical import *
import math
from environment import *

ENVIRONMENT_NAME = "Smoke"

state = 0
level = 0
ALARM_LEVEL = 40

def main():
    setup()
    while True:
		loop()
		
def setup():
    IoEClient.setup({
        "type": "Smoke Detector",
        "states": [{
            "name": "Alarm",
            "type": "bool",
            "controllable": False
        },
        {
            "name": "Level",
            "type": "number",
            "controllable": False
        }]
    })

    restoreProperty("Alarm Level", 40)
    IoEClient.onInputReceive(onInputReceiveDone)
    add_event_detect(0, detect)
    
    state = restoreProperty("state", 0)
    setState(state)

def onInputReceiveDone(data):
    processData(data, True)
    
def detect():
    processData(customRead(0), False)

def restoreProperty(propertyName, defaultValue):
    value = getDeviceProperty(getName(), propertyName)
    if  not (value is "" or value is None):
        if  type(defaultValue) is int :
            value = int(value)

        setDeviceProperty(getName(), propertyName, value)
        return value
    return defaultValue

def loop():
    global ENVIRONMENT_NAME
    value = Environment.get(ENVIRONMENT_NAME)
    if value >= 0:
        setLevel(Environment.get(ENVIRONMENT_NAME))
    #print(value)
    sleep(1)


def processData(data, bIsRemote):
    if len(data) <= 0 :
        return
    data = data.split(",")
    setState(int(data[0]))


def sendReport():
    global state
    global level
    report = str(state) + "," + str(level);   # comma seperated states
    IoEClient.reportStates(report)
    setDeviceProperty(getName(), "state", state)
    setDeviceProperty(getName(), "level", level)


def setState(newState):
    global state
    state = newState

    if newState is 0:
        digitalWrite(1, LOW)
    else:
        digitalWrite(1, HIGH)

    sendReport()


def setLevel(newLevel):
    global level
    if level == newLevel:
        return

    level = newLevel
    if level > ALARM_LEVEL:
        setState(1)
    else:
        setState(0)

    sendReport()

if __name__ == "__main__":
    main()
```
## INSTALLATION
### Clone The Repository
```bash
git clone https://github.com/haniyakk/IOT-Based-Smart-Home.git
cd RemoteHouse
```
now you can download the project locally on your system.
## Implementation
   1️⃣. Setup of NodeMCU with relay modules for controlling appliances like lights, fans, AC, coffee maker, and sprinkler system. <br>
   2️⃣. Integration of PIR sensors at windows and garage for intrusion and motion detection.<br>
   3️⃣. Connection of smoke detectors in the garage and house interior to trigger alarms and sprinkler systems after a set time. <br>
   4️⃣. Configuration of webcam modules to activate upon suspicious activity detection. <br>
   5️⃣. Design of the remote dashboard for controlling appliances and monitoring system alerts. <br>
   6️⃣. Implementation of smart lawn sprinkler automation. <br>
   7️⃣. System testing and simulation on software platforms. <br>

## DISCLAIMER

This project is a non-commercial academic model, created for learning and demonstration purposes only. It is just my 3rd semester's project, built as part of my coursework. It is not intended for production or commercial use.

🗂️ Note: In order to run this project you need to install Cisco Packet Tracer.
