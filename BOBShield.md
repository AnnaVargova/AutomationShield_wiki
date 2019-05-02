**Content:**

#### [Introduction](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield#introduction-1)
#### [Arduino library](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield#arduino-library-1)
* servo motor and sensor
* PID control
#### [3D sketch](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield#3d-sketch)
#### [Circuit design](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#circuit-design)
#### [Components](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#components)
#### [PCB layout](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#pcb-layout)
#### [Gallery](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#gallery)
#### [About](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#about) 
#### [Authors](https://github.com/gergelytakacs/AutomationShield/wiki/BOBShield/_edit#authors)

# Introduction

BOBShield or Ball on beam shield is device which consist from micro servo motor, plastic transparent tube, ball, Time of Flight distance sensor,  PID controller which can be or is control by Arduino Zero or Uno or Due, because it can work on 3.3 or 5V . The project BOBShield is didactical device for education feedback control principles. Main goal of the device is to stabilize the ball in the center of the tube. The length of tube is 100mm and diameter is 10mm. The ball diameter is 8mm and can be made from various material, such as glass, plastic, wood, cork or metal. The tube is placed in the center of tube holder, which is connect to micro servo motor, which can tilt the tube in clockwise and counter-clockwise directions. Other end of tube holder is palced in a ball bearing, which is inserted in plastic holder. The micro servo motor is also inserted in plastic holder and fixed with steel screws. The holder are fixed with steel screws to printed circuit board. TOF distance sensor VL6180X marking the position of the ball, the sensor contains a very tiny laser source, and a matching sensor. He can handle about 5mm to 100mm of range distance. Sensor is placed in holder on the end of  the tube, on other side of the tube is simple closure. PID controller control the position of the tube and is placed on printed circuit board.                                                                                                         This device is inexpensive and mechanically simple for construction.

# Arduino library

**C/C++ API**

The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers

# 3D sketch



# Circuit design



# Components



# PCB Layout

The printed circuit board (PCB) has been designed in the CAD software DIPTrace, Freeware version.  The PCB has two layers and fits within the customary 100x100mm limit of most board manufacturers. The DIPTrace PCB layout of the BOBShield can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/files/3126564/BoBShield_R1_Final.zip) and the BOBShield Production files can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/files/3126563/BoBShield_Production_R1.zip).

![The upper part of the PCB](https://user-images.githubusercontent.com/37699408/56760632-e7ea6200-679b-11e9-869d-21d8d7e0bdf1.png)

On the picture of the upper layout can be seen black line is starting from digital pin 9 and going to the pin where micro servo motor has connector, it is signal for micro servo motor. Power for black line come from 3.3V pin and go to pin AREF. Analogue REFerence it allows to feed the Arduino a reference voltage from an external power supply. It would be feed with 3.3V into the AREF pin – perhaps from a voltage regulator IC. From 5V pin exit red line which is  power for resistor and diode and continuous to the ServoPinout. Blue line is ground for SensorPinout diode resistor and ServoPinout.

![The bottom part of the PCB](https://user-images.githubusercontent.com/37699408/56760628-e6b93500-679b-11e9-85ba-842c4c616c45.png)

On the bottom layout can be seen black line starting from power of 3.3V going to + of SensorPinout then continue to USB. Blue line is ground. The other two black lines starting from pins Serial Clock Line (SCL) and Serial Data Line (SDA) and going to SCL and SDA on SensorPinOut. I2C uses only two bidirectional open collector or open drain lines for SDA and SCL pulled up with resistors. Typical voltages used are +5 V or +3.3 V.
# Gallery



# About
The shield was developed as a semester project for subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics in 2018/2019. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

# Authors

* **Hardware and 3D model design:** Tibor Konkoly, Patrik Kvasný, Marko Michal, Marek Krippel 
* **Software design:** Lukáš Vadovič, Matúš Bíro, Samuel Mladý
* **Wiki documentation:** Marek Krippel, Rastislav Haška     





