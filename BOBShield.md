**Content:**

[Introduction](#introduction-1)<br/>
[Arduino library](#arduino-library-1)<br/>
&nbsp;&nbsp;&nbsp;[Servo motor and sensor functions](#servo-motor-and-sensor-1)<br/>
&nbsp;&nbsp;&nbsp;[PID control functions](#PID-1)<br/>
[3D sketch](#3d-sketch-1)<br/>
[Circuit design](#circuit-design-1)<br/>
[Components](#components-1)<br/>
[PCB layout](#pcb-layout-1)<br/>
[Gallery](#gallery-1)<br/>
[About](#about-1)<br/>
[Authors](#authors-1)<br/>

# <a name="introduction-1"/>Introduction

BOBShield or Ball on beam shield is didactical device for education feedback control principles and is a part of the AutomationShield project. The project BOBShield is device which consist from microservo motor, plastic transparent tube, ball, Time of Flight distance sensor and potentiometer with which PID control is achieved. Components such as the microservo motor, distance sensor and potentiometer are controlled by the Arduino. These components can be controlled by Arduino Zero or Uno or Due. Arduino is powered by USB. The average power of a USB port is about 5 volts. Main goal of the device is to stabilize the ball in the center of the tube. The length of tube is 100mm and diameter is 10mm. The ball diameter is 8mm and can be made from various material, such as glass, plastic, wood, cork or metal. Metal ball turned out to be the best solution. The tube is placed in the center of tube holder, one side of the holder is connect to microservo motor and other side of tube holder is place in a ball bearing, which is insert in plastic holder. The microservo motor is also insert in plastic holder and fixed with steel screws. The plastic holders are fix with steel screws to printed circuit board. Microservo motor tilts the tube in clockwise and counter-clockwise directions. TOF distance sensor Adafruit VL6180X marking the position of the ball, the sensor contains a very tiny laser source, and a matching sensor. He can handle about 5mm to 100mm of range distance. Sensor is put in holder on the end of  the tube, on other side of the tube is simple closure. PID controller control the position of the tube and is placed on printed circuit board.                                                                                                         This device is inexpensive and mechanically simple for construction.

# <a name="arduino-library-1"/>Arduino library

**C/C++ API**

The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and functions for PID controller and feedback control device. All functionality associated with the BOBShield is included in the BOBShield.h header.
The following subsections describe the methods used for manage microservo motor, sensor and PID controller.
The function which declaring PIN and initializing sensor is `BOBClass::begin()` .
* <a name="servo-motor-and-sensor-1"/>Servo motor and sensor functions<br/>
Function for start of the servo motor is:
`BOBClass::calibration()`
In this operation

* <a name="PID-1"/>PID control functions

# <a name="3d-skecth-1"/>3D sketch
The whole model was designed in CAD software and forwarded to 3D print service. There are five parts to be printed, a motor holder, ball bearing holder in which one side of the tube holder is insert, tube holder, sensor holder and simple closure. Other assembly parts, a sensor, Arduino microcontroller, potentiometer and servo motor are downloadable from GrabCAD database. Printed circuit board is rendered from DIPTrace software.
![Picture1](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_1.jpg)
![Picture2](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_2.jpg)
![Picture3](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_3.jpg)

# <a name="circuit-design-1"/>Circuit design
The circuit schematics has been designed in the CAD software [DIPTrace](https://diptrace.com/), Freeware version. You may download the circuit schematics for the BOBShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_Circuit.rar).

![Circuit design](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Circuit.png)

Power for circuit is coming from pin 5V. This pin powering microservo motor SM, capacitor C1 and diode D1. From the pin with 3.3V is powered Time Of Flight sensor J and potentiometer POT1. Everything is connect to the pin GND ground. The digital pin 9 is connect to microservo motor from which comes signal for angular position of the servo motor. Analog signal A0 is connected potentiometer POT1, through which the servo motor position is controlled. Also analog pins A4/SDA and A5/SCL are connected to Time Of Flight sensor J and these connctions are used for detection signal of TOF sensor and feedback signal of TOF sensor J. 



# <a name="components-1"/>Components
To make an BOBShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|   Part   |             Name             | Type, Value, Note | PCS |
|:--------:|:-----------------------------|:----------------|-----|
|     J    |       Adafruit VL6180x       |     [485-3316](https://www.mouser.sk/ProductDetail/Adafruit/3316?qs=sGAEpiMZZMuYaq4aOfOV%252BNGpcmpxct%252BzTY0qY%2FO75Rw%3D)    |  1  |
|    D1    |             Diode            |   [625-RGF1D-E3](https://www.mouser.sk/ProductDetail/Vishay-Semiconductors/RGF1D-E3-67A?qs=sGAEpiMZZMtoHjESLttvktgFZl1w4a%2F%2F3p6qGDZZc4o%3D)  |  1  |
|    C1    |   Capacitor, tantallum 0805  | [581-F980J107MSA](https://www.mouser.sk/ProductDetail/AVX/F980J107MSA?qs=sGAEpiMZZMukHu%252BjC5l7YXOgdEVzCIlfrJV01KbJCe0%3D) |  1  |
|     M    |   Metal Geared Micro Servo   |   [426-SER0039](https://www.mouser.sk/ProductDetail/DFRobot/SER0039?qs=sGAEpiMZZMuYaq4aOfOV%252BLexKvAPmd2jLf6dNsIPlOo%3D)   |  1  |
|          |       Stackable Header       |  [474-PRT-11417](https://www.mouser.sk/ProductDetail/SparkFun/PRT-11417?qs=sGAEpiMZZMuWWq7rhECaKREdwluNxBetc4EOoXderyo%3D)  |  1  |
|          |         FFC 7W cable         |    [25001-0706](https://www.mouser.sk/ProductDetail/Molex/25001-0706?qs=%2Fha2pyFadugZmsfhHu5zKysic76yyPDnAtsFMVzXfdw%3D)   |  1  |
|          |    FFC 7W connector to PCB   |    [5-520314-7](https://www.mouser.sk/ProductDetail/TE-Connectivity-AMP/5-520314-7?qs=%2Fha2pyFadugNrkwlo5BkC5EJBJfGK4mrwxpJfvEWOpo%3D)   |  1  |
|          | FFC 7W connector to breakout |   [67013-007LF](https://www.mouser.sk/ProductDetail/Amphenol-FCI/67013-007LF?qs=%2Fha2pyFaduiSCRu%252BHsRKk0mUg9V%252BraZZnrjJDL1VIyTIiluBEDAwSA%3D%3D)   |  1  |
| ARDUINO1 |       Arduino Uno Rev3       |                 |  1  |
|          |              PCB             |                 |  1  |
|   POT1   |         Potentiometer        |                 |  1  |
|          |            Screws            |                 |     |
|          |             Nuts             |                 |     |
|          |       3D printed parts       |                 |  5  |
|          |             Tube             |                 |  1  |
|          |         Ball-bearing         |                 |  1  |

# <a name="pcb-layout-1"/>PCB Layout

The printed circuit board (PCB) has been designed in the CAD software [DIPTrace](https://diptrace.com/), Freeware version.  The PCB has two layers and fits within the customary 100x100mm limit of most board manufacturers. The DIPTrace PCB layout of the BOBShield can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_PCB_R1_Final.zip) and the BOBShield Production files can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_Gerber_Production_R1.zip).

![The upper part of the PCB](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Upper_part_of_the_PCB.png)

On the picture of the upper layout can be seen black line is starting from digital pin 9 and going to the pin where micro servo motor has connector, it is signal for micro servo motor. Power for black line come from 3.3V pin and go to pin AREF. Analogue REFerence it allows to feed the Arduino a reference voltage from an external power supply. It would be feed with 3.3V into the AREF pin – perhaps from a voltage regulator IC. From 5V pin exit red line which is  power for resistor and diode and continuous to the ServoPinout. Blue line is ground for SensorPinout diode resistor and ServoPinout.

![Bottom part of the PCB](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Bottom_part_of_the_PCB.png)

On the bottom layout can be seen black line starting from power of 3.3V going to + of SensorPinout then continue to USB. Blue line is ground. The other two black lines starting from pins Serial Clock Line (SCL) and Serial Data Line (SDA) and going to SCL and SDA on SensorPinOut. I2C uses only two bidirectional open collector or open drain lines for SDA and SCL pulled up with resistors. Typical voltages used are +5 V or +3.3 V.
# <a name="gallery"/>Gallery



# <a name="about-1"/>About
The shield was developed as a semester project for subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics in 2018/2019. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

# <a name="authors-1"/>Authors

* **Hardware and 3D model design:** Tibor Konkoly, Patrik Kvasný, Marko Michal, Marek Krippel 
* **Software design:** Lukáš Vadovič, Matúš Bíro, Samuel Mladý
* **Wiki documentation:** Marek Krippel, Rastislav Haška     





