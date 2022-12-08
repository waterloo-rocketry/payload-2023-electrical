WELCOME TO PAYLOAD: "NOT QUITE DEPLOYABLE"

Here is the folder in the google drive where most of our research happens (if you don't have access, and need it, ask!)
https://drive.google.com/drive/folders/1Cd6_AShmbkWPmAZkzhvAZI7hbHMKju_5?usp=sharing


If you can't find the project you're looking for, make sure to check the branches

CONFLUENCE (but don't expect it to be updated)
https://uwaterloo.atlassian.net/wiki/spaces/ROCKETRY/pages/42574873580/Payload

**How to Start Editing Schematics**
- Clone the "canhw" repo from https://github.com/waterloo-rocketry/canhw
- Enter canhw and make sure you are on the most up to date version of master branch
- Make sure that both the payload-2023-electrical and canhw repos are in the same parent directory! KiCAD will not be able to find our custom libraries otherwise

# 2023 Electrical Overview
Please see the updated .drawio file, which contains a detailed electrical block diagram

The 2023 electrical system is made up of 5 boards:

### GPS board
- Can be found at https://github.com/waterloo-rocketry/canhw/tree/master/gps
- GPS is a very simple board. It gets data from a GPS reciever and forwards it over CAN. 
- Data from GPS board will be the x/y position measurement input to state estimation
- A copy will be mounted on GrandPapa

### Logger board
- Can be found at https://github.com/waterloo-rocketry/canhw/tree/master/logger
- Logger is also a very simple board. It reads in all the CAN messages it sees and saves them to an SD card
- Logger is extremely useful for all sorts of diagnostics, but will allow us to collect all the sensor data as the rocket flies, do some post-flight analysis, and hopefully figure out if state estimation did a good job
- A copy will be mounted on GrandPapa

### Minisensor
- Can be found at https://github.com/waterloo-rocketry/canhw/tree/master/sensor_mini 
- Minisensor gets data from an altimeter, IMU (an accelerometer, magnetometer, and gyroscope packed into one chip) and optionally one or more pressure transmitters, RTDs, strain gauges, the list goes on (there are some generic amplifier circuits that have a variety of uses).
- For its application to payload, we only care about the IMU and altimeter, which provide most of the data to state estimation
- Will be wired to GrandPapa to maximize flexibility and because there's no more space on it lol

### COTS PIC Dev Board (DM320209)
- Datasheet can be found at http://ww1.microchip.com/downloads/en/DeviceDoc/Curiosity_PIC32MZEF2.0_Development_Board_Users_Guide_DS70005400A.pdf
- This beefy boy runs state estimation cause we did not have time to design a second board from scratch and matrix multiplication (which is basically what state estimation is) is really computationally intensive. An NVIDIA GeForce RTX 4090 GPU was proposed as an alternative but was ultimately struck down because "it would cost double payload's yearly budget" and "doesn't obey the 3U Cubesat standard".

### GrandPapa
- Can be found RIGHT HERE under "/GrandPapa_Board"
- GrandPapa is our power management, battery charging, and communications board. It pulls from the rocket to power the payload's systems, and pulls from the payload's battery supply when external power is no longer available. It also manages communication between RocketCAN (the rocket's main communication network) and the payload's internal network. Because CAN is a broadcast protocol (every board sees every message on the network all the time), and payload will be constantly transmitting sensor data, we do not want those messages interfering with whatever is happening on RocketCAN or vice versa
- See the schamtic notes for details on the power path