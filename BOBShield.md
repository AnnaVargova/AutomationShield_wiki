#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#c)<br/>
&nbsp;&nbsp;&nbsp;[Other APIs](#other)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[Feedback control](#control)<br/>
&nbsp;&nbsp;&nbsp;[System identification](#ident)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>

[Introduction](#introduction-1)<br/>
[Arduino library](#arduino-library-1)<br/>
&nbsp;&nbsp;&nbsp;[Servo motor and sensor functions](#servo-motor-and-sensor-1)<br/>
[3D sketch](#3d-sketch-1)<br/>
[Circuit design](#circuit-design-1)<br/>
[Components](#components-1)<br/>
[PCB layout](#pcb-layout-1)<br/>
[Gallery](#gallery-1)<br/>
[About](#about-1)<br/>
[Authors](#authors-1)<br/>

# <a name="introduction-1"/>Introduction

BOBShield or Ball on beam shield is a didactical device for education feedback control principles and is a part of the AutomationShield project. The project BOBShield is a device which consists of the microservo motor, the plastic transparent tube, the ball, the Time of Flight distance sensor and the potentiometer with which PID control is achieved. Components such as the microservo motor, distance sensor and potentiometer are controlled by the Arduino. These components can be controlled by Arduino Zero, Uno or Due. Arduino is powered by USB. The average power of a USB port is about 5 volts. The main goal of the device is to stabilize the ball in the centre of the tube. The length of the tube is 100 mm and the diameter is 10 mm. The ball diameter is 8 mm and can be made from various materials, such as glass, plastic, wood, cork or metal. A metal ball turned out to be the best solution. The tube is placed in the centre of tube holder, one side of the holder is connected to the microservo motor and the other side of the tube holder is attached to a ball bearing, which is embedded in the plastic holder. The microservo motor is also inserted in a plastic holder and fixed with steel screws. The plastic holders are fixed with steel screws to the printed circuit board. The microservo motor tilts the tube in the clockwise and the counter-clockwise directions. A ToF distance sensor Adafruit VL6180X is marking the position of the ball, the sensor contains a tiny laser source, and a matching sensor. It can handle about 5 mm to 100 mm of range distance. The sensor is put in the holder on the end of the tube. On the other side of the tube there is a simple closure. PID controller controls the position of the tube and is placed on the printed circuit board. This device is inexpensive and mechanically simple for the construction.

# <a name="arduino-library-1"/>Arduino library

**C/C++ API**

The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and functions for PID controller and feedback control device. All functionality associated with the BOBShield is included in the BOBShield.h header. 
The following subsections describe the methods used to manage microservo motor, sensor and PID controller. The function which declares PIN and initializes the sensor is `BOBClass::begin()` .
* <a name="servo-motor-and-sensor-1"/>Servo motor and sensor functions<br/>
 `void BOBClass::initialize()`<br/>
This function check if sensor is available and if the function find sensor, then the next function come is calibration.<br/>
`void BOBClass::calibration()`<br/>
This operation start calibrating the sensor, tilt beam go to the minimum to -30 degrees so the ball fall toward a simple closure and then sensor perform 100 measurements in 1 second. Then tilt beam go to the maximum to 30 degrees, the ball fall toward the sensor and again the sensor perform 100 measurements in 1 second. And save both minimum and maximum values after each ending of measurements.<br/>
`void BOBClass::actuatorWrite(float fdeg)`<br/>
This function write to actuator some parameters as predefined boundary for servo range, mapping inputs defined by user in degrees (-30/30) in to values understandable for servo (65/125) in degrees.<br/>
`float BOBClass::sensorRead()`<br/>
SensorRead function return the corrected value of sensor and set actual position to position — calibrated minimum also set actual position to position — predefined value.<br/>

# <a name="3d-skecth-1"/>3D sketch
The whole model was designed in CAD software and forwarded to the 3D print service. There are five parts to be printed: stand with servo, stand without servo, the tube holder, the sensor case and the simple closure or blinding. You may download 3D printed parts from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_3D_printed_parts.rar). The entire assembly you may download from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_final_assembly.rar). Other assembly parts: a sensor, a Arduino microcontroller, a potentiometer and a servo motor are downloadable from [GrabCAD database](https://grabcad.com/library). Printed circuit board is rendered from DIPTrace software.
![Picture1](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_1.jpg)
![Picture2](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_2.jpg)
![Picture3](https://github.com/gergelytakacs/AutomationShield/wiki/fig/3D_Model_BOBShield_3.jpg)

# <a name="circuit-design-1"/>Circuit design
The circuit diagram has been designed in the CAD software [DIPTrace](https://diptrace.com/), Freeware version. You may download the circuit schematics for the BOBShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_Circuit.rar).

![Circuit design](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Circuit.png)

The power for the circuit is coming from the pin 5V. This pin powers the microservo motor SM, the capacitor C1 and the diode D1. Time of Flight sensor J and the potentiometer POT1 are powered by the pin with 3.3V. Everything is connected to the pin GND ground. The digital pin 9 is connected to the microservo motor from which the signal comes for the angular position of the servo motor. The analog signal A0 is connected (to the) potentiometer POT1, through which the servo motor position is controlled. Also the analog pins A4/SDA and A5/SCL are connected to the Time of Flight sensor J and these connections are used for the detection signal of the ToF sensor and the feedback signal of the ToF sensor J.



# <a name="components-1"/>Components
To make the BOBShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|   Part   |             Name             |  Stock number  | Value | PCS |
|:--------:|:-----------------------------|:----------------|-------|-----|
|     J    |       Adafruit VL6180x       |     [485-3316](https://www.mouser.sk/ProductDetail/Adafruit/3316?qs=sGAEpiMZZMuYaq4aOfOV%252BNGpcmpxct%252BzTY0qY%2FO75Rw%3D)    | Optical Sensor, Time of Flight Dist. Ranging SNSR |   1  |
|    D1    |             Diode            |   [625-RGF1D-E3](https://www.mouser.sk/ProductDetail/Vishay-Semiconductors/RGF1D-E3-67A?qs=sGAEpiMZZMtoHjESLttvktgFZl1w4a%2F%2F3p6qGDZZc4o%3D)  | Power Switching 1 Amp 200 Volt 150ns 30 Amp IFSM |   1  |
|    C1    |   Capacitor  | [581-F980J107MSA](https://www.mouser.sk/ProductDetail/AVX/F980J107MSA?qs=sGAEpiMZZMukHu%252BjC5l7YXOgdEVzCIlfrJV01KbJCe0%3D) | Tantalum Capacitors - Solid SMD 100uF 6.3V 20% 0805, 2x1.25x0.9mm |   1  |
|     M    |   Metal Geared Micro Servo   |   [426-SER0039](https://www.mouser.sk/ProductDetail/DFRobot/SER0039?qs=sGAEpiMZZMuYaq4aOfOV%252BLexKvAPmd2jLf6dNsIPlOo%3D)   | Metal Geared 9g Micro Servo |   1  |
|          |       Stackable Header       |  [474-PRT-11417](https://www.mouser.sk/ProductDetail/SparkFun/PRT-11417?qs=sGAEpiMZZMuWWq7rhECaKREdwluNxBetc4EOoXderyo%3D)  | SparkFun Accessories Arduino - R3 |   1  |
|          |         FFC 7W cable         |    [25001-0706](https://www.mouser.sk/ProductDetail/Molex/25001-0706?qs=%2Fha2pyFadugZmsfhHu5zKysic76yyPDnAtsFMVzXfdw%3D)   | Jumper Cables STD. CABLE JUMPER |   1  |
|          |    FFC 7W connector to PCB   |    [5-520314-7](https://www.mouser.sk/ProductDetail/TE-Connectivity-AMP/5-520314-7?qs=%2Fha2pyFadugNrkwlo5BkC5EJBJfGK4mrwxpJfvEWOpo%3D)   | 100X100 REC 1X07P |   1  |
|          | FFC 7W connector to breakout |   [67013-007LF](https://www.mouser.sk/ProductDetail/Amphenol-FCI/67013-007LF?qs=%2Fha2pyFaduiSCRu%252BHsRKk0mUg9V%252BraZZnrjJDL1VIyTIiluBEDAwSA%3D%3D)   | DUFLEX HSNG SR |   1  |
| ARDUINO1 |       Arduino Uno Rev3       |                 |       |  1  |
|          |              PCB             |                 |       |  1  |
|   POT1   |         Potentiometer        |                 |       |  1  |
|          |            Screws            |                 |       |     |
|          |             Nuts             |                 |       |     |
|          |       3D printed parts       |                 |       |  5  |
|          |             Tube             |                 |       |  1  |
|          |         Ball-bearing         |                 |       |  1  |

# <a name="pcb-layout-1"/>PCB Layout

The printed circuit board (PCB) has been designed in the CAD software [DIPTrace](https://diptrace.com/), Freeware version. The PCB has two layers and fits within the customary 100x100 mm limit of most board manufacturers. The DIPTrace PCB layout of the BOBShield can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_PCB_R1_Final.zip) and the BOBShield Production files can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_Gerber_Production_R1.zip).

![The upper part of the PCB](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Upper_part_of_the_PCB.png)

In the picture of the upper layout a black line can be seen which is starting from digital pin 9 and continues to the pin where micro servo motor has a connector, it is the signal for the micro servo motor. The power for the black line comes from the 3.3V pin and goes to the pin AREF. Analogue REFerence allows to feed the Arduino reference voltage from an external power supply. It would be fed with 3.3V into the AREF pin – perhaps from a voltage regulator IC. The resistor and diode are powered by the red line which outputs from the 5V pin and continues to the ServoPinout. The blue line is ground for the SensorPinout diode resistor and the ServoPinout.

![Bottom part of the PCB](https://github.com/gergelytakacs/AutomationShield/wiki/fig/BOBShield_Bottom_part_of_the_PCB.png)

At the bottom layout the black line can be seen starting from power of 3.3V going to + of SensorPinout, then continues to the USB. The blue line is ground. The other two black lines start from the pins Serial Clock Line (SCL) and the Serial Data Line (SDA) and continue the SCL and the SDA on the SensorPinOut. I2C uses only two bidirectional open collector or open drain lines for the SDA and the SCL pulled up with resistors. Typical voltages used are +5 V or +3.3 V.


# <a name="intro"/>Introduction
...
# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

### Input

### Output

## <a name="other"/>Other APIs
Expansion of the current API to MATLAB, Simulink and Python is currently in progress.

## <a name="simulink"/>Simulink API

# <a name="examples"/>Examples

## <a name="control"/>Feedback control

## <a name="ident"/>System identification
In progress.

# <a name="hardware"/>Detailed hardware description

## <a name="circuit"/>Circuit design

## <a name="parts"/>Parts
To make a BoBShield you will need the following parts or their similar equivalents:

| Part             | Name            | Type/Value/Note                                                         | PCS |
|------------------|-----------------|-------------------------------------------------------------------------|-----|
| (a)   	   | PCB             | 2-layer, FR4, 1.6mm thick (e.g. [JCLPCB](https://jlcpcb.com/))          | 1   |
| (e<sub>1-5</sub>)| 3D print        | 18g, Ø1.75mm PETG filament, bright green, at 240°C (90°C bed), 3h 20min | 1   |
| (o),POT1         | Trimmer         | 10kΩ, 250mW, single turn THT trimmer (e.g. [ACP CA14NV12,5-10KA2020](https://www.tme.eu/gb/details/ca14v-10k/single-turn-tht-trimmers/acp/ca14nv12-5-10ka2020/)) | 1   |
| (h)   	   | Screw           | DIN 7971C 2.2×6.5                                                       | 1   |
| (l),D1           | Diode           | Generic diode, DO214AC (e.g. [Vishay Semiconductor BYG20J](https://www.tme.eu/gb/details/byg20j-e3_tr/smd-universal-diodes/vishay/))                              | 1   |
| (g<sub>1</sub>)  | Screws          | M3×8 DIN963A                                                            | 4   |
| (g<sub>2</sub>)  | Nuts            | M3 DIN439B                                                              | 4   |
| (b<sub>3</sub>)  | Header          | 10×1, female, stackable, 0.1'' pitch  (e.g. [SparkFun 474-PRT-10007](https://eu.mouser.com/ProductDetail/SparkFun/PRT-10007?qs=WyAARYrbSnYGf8RckgedYg==))      | 1   |
| (p)	           | Pot shaft       | For ``POT1'', (e.g. [ACP CA9MA9005](https://www.tme.eu/gb/details/ca9ma5-b/knobs-for-trimmers/acp/ca9ma-9005-black/#))                                         | 1   |
| (b<sub>1</sub>)  | Header          | 8×1, female, stackable, 0.1'' pitch (e.g. [SparkFun 474-PRT-10007](https://eu.mouser.com/ProductDetail/SparkFun/PRT-10007?qs=WyAARYrbSnYGf8RckgedYg==))      | 2   |
| (b<sub>2</sub>)  | Header          | 6×1, female, stackable, 0.1'' pitch (e.g. [SparkFun 474-PRT-10007](https://eu.mouser.com/ProductDetail/SparkFun/PRT-10007?qs=WyAARYrbSnYGf8RckgedYg==))      | 1   |
| (c)              | Tube            | transparent, plexiglass (PMMA), L=100mm, 12/10mm                      | 1   |
| (m),J            | Sensor          | [VL53L1X breakout](https://www.st.com/en/evaluation-tools/vl53l1x-satel.html), time of flight ranging SNSR                         | 1   |
| (i),M            | Motor           | Metal geared 9g, 5V micro servo                                       | 1   |
| (k),C1           | Capacitor       | 100uF, 6.3V, 20, 0805, tantalum                                       | 1   |
| (d)              | Ball            | Steel ball, diameter 8mm                                              | 1   |
| (f)              | Bearing         | Miniature ball bearing, 3×6×2mm, (e.g. [MR-63-ZZ](https://www.nbr.eu/en/products/bearings/deep-groove-ball-bearing/mr-63-zz/))                                          | 1   |
| (n)              | Cable           | Ribbon cable, 4-7 wires, cca. 0.2m (e.g. EL-2468)                     | 1   |

The total cost of the above components and thus of the entire BoBShield is no more than $10 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_PCB.zip) and [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_Circuit.zip), respectively, while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_Gerber.zip).

![BOB_pcb](https://user-images.githubusercontent.com/18485913/101607872-5cbbef80-3a05-11eb-83c4-27b7500597f2.png)

# <a name="about-1"/>About
This shield was designed and created as a term project at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2019/2020.
# <a name="authors-1"/>Authors

* **Hardware and 3D model design:** Tibor Konkoly, Patrik Kvasný, Marko Michal, Marek Krippel 
* **Software design:** Lukáš Vadovič, Matúš Bíro, Samuel Mladý
* **Wiki documentation:** Martin Gulan, Rastislav Haška, Marek Krippel      





