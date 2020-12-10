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

# <a name="intro"/>Introduction
...
# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

### Input

### Output

## <a name="other"/>Other APIs
Expansion of the current API to MATLAB, Simulink and Python is currently in progress.

# <a name="examples"/>Examples

Currently, there are four examples offered for the BOBShield Arduino API.

The file [`BOBShield_SelfTest.ino`](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/BOBShield/BOBShield_SelfTest/BOBShield_SelfTest.ino) implements a basic self-test routine, aiding the verification of the hardware functionality. First, the ToF sensor is initialized and, if the MCU cannot connect to the sensor chip, will report the failure. Then, the servo turns to each of its extreme operation points and the routine compares the distance readings with expected values.

The project file [`BOBShield_Identification.ino`](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/BOBShield/BOBShield_Identification/BOBShield_Identification.ino) performs an open-loop test by supplying a range of actuator settings, while [`BOBShield_Identification_aprbs.ino`](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/BOBShield/BOBShield_Identification_aprbs/BOBShield_Identification_aprbs.ino) does the same but applies an amplitude-modulated pseudo-random signal.

## <a name="control"/>Feedback control

Finally, [`BOBShield_PID.ino`](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/BOBShield/BOBShield_PID/BOBShield_PID.ino) demonstrates the proportional-integral-derivative (PID) position control of the steel ball.

Besides the engaging visual action of the hardware, results are also printed to the serial line. The figure below demonstrates a step change of the reference, where the process variables <em>r</em> (blue), <em>y</em> (red) and <em>u</em> (green) may be followed in the oscilloscope-like rolling window of the Arduino IDE Serial Plotter. Similar to the open-loop case, any other terminal program is suitable for monitoring and data logging.

![BOB_screenshot](https://user-images.githubusercontent.com/18485913/101794273-00d98f80-3b07-11eb-8f66-8b5ef6b9419a.png)

The PID demonstration example is sampled with a <em>T</em>=10 ms period, and a pre-set reference vector of [30 50 8 65 15 40]<sup>T</sup> mm is requested for 1000 samples (10 seconds) providing sufficient time for transients. The PID controller is manually tuned to the <em>K</em><sub>P</sub>=0.3 (mm/°), <em>T</em><sub>I</sub>=600 and <em>T</em><sub>D</sub>=0.22 constants. An integral windup clipping strategy is set to ±10 (°), while the input to the servo motor is saturated at <em>u</em>=30 (°).

The results of this experiment, processed in [MATLAB](https://www.mathworks.com/downloads/), are illustrated in the figure below, where reference position <em>r</em> is compared to achieved output <em>y</em> on the top and the corresponding servo input <em>u</em> is presented at the bottom.

![BOB_PID](https://user-images.githubusercontent.com/18485913/101798625-94ad5a80-3b0b-11eb-8fc9-ce43df9aea97.png)

The ball follows the reference position trajectory faithfully across a wide span of the working range. The tracking is worse when the ball is close to the sensor. We believe that this is caused by sensor noise originating from unwanted reflections or other physical effects. Note that the response may be further improved by more advanced control and/or appropriate signal processing.

## <a name="ident"/>System identification
In progress.

# <a name="hardware"/>Detailed hardware description

The BOBShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics of the FloatShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_Circuit.rar).

![BOB_scheme](https://user-images.githubusercontent.com/18485913/101789913-23b57500-3b02-11eb-9f3b-96080eb25e4a.png)

The inclination of the tube and, ultimately, the position of the ball, is manipulated by a standard micro RC Servo motor **M** (i) with analog feedback. The total rotation range of the motor utilized in this application is only ±30°. The motor is connected to the D9 pin of the standard Arduino R3 layout, since this equivalent MCU pin handles interrupts and timing on the Uno as well as other prototyping devices. The current consumption of the servo motor is low enough to be directly powered from the 5V supply of the Arduino board without the need of an external wall-plug adapter. We added a capacitor **C1** (k) parallel to the motor to smooth out possible transients affecting the board supply and a diode **D1** (l) to prevent possible back-EMF damaging the circuitry.

The absolute distance of the ball from the reference located at one end of the tube is measured by the ST Microelectronics VL6180X ToF sensor **J** (m). We have decided to utilize a widely available breakout board implementing the VL6180X for hobbyist use. This breakout may be purchased as an end-product and implements the sensor on a small-outline PCB with standard 0.1'' pitch. The sensor communicates through the I2C protocol and we have utilized the standard A4/SDA and A5/SCL pins of the layout. The breakout connects to the base board via a flexible 4-wire ribbon cable (n). A flat flexible cable (FFC) would have been a better choice, unfortunately the sensor module would need to be custom-made for this.

Finally, there is a potentiometer **POT1** (o) with a small plastic shaft (p) allowing to manually input the desired reference signal. The potentiometer is fed by the 3.3V source for its maximal setting, so that the wiper connected to the A0 analog port does not damage other microcontroller boards, such as the Due or Zero, operating with CMOS logic levels. To maximize analog resolution, we have also connected the 3.3V source to the AREF pin and compensate for this change in code. The sensor board is also powered by the 3.3V source.

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

The total cost of the above components and thus of the entire BOBShield is no more than $10 excluding labor and postage.

The assembled BOBShield is shown below. The equivalents of the circuit components described above for the schematics are marked on the assembled device with the same letters.

![BOB_assembled](https://user-images.githubusercontent.com/18485913/101788033-fff12f80-3aff-11eb-81f0-d3c6711bc2eb.png)

One end of the tube is closed by a 3D printed cap (e<sub>1</sub>), while the other end is terminated by another 3D printed part (e<sub>2</sub>) designed to hold the distance measurement unit. The tube is fixed at its middle by a 3D printed circular clamp (e<sub>3</sub>) that permits the inclination of the tube. The clamp is held in place at both sides by L-shaped 3D printed brackets, one (e<sub>4</sub>) containing a miniature ball bearing (f) to facilitate smooth movement and the other (e<sub>5</sub>) fixed to the clamp through the shaft of the actuator. The figure below shows all of the 3D printed components (e<sub>1-5</sub>), for which we provide downloadable [CAD files](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_3D_printed_parts.rar). They were printed on an original Prusa i3 MK3/S 3D printer using a total of 18g filament under 3.5h time.

![BOB_CAD](https://user-images.githubusercontent.com/18485913/101789752-f2d54000-3b01-11eb-98f8-c32053e2fbf7.png)

A 3D CAD sketch of the entire assembly may also be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_final_assembly.rar).

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100×100mm limit of most board manufacturers. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_PCB_R1_Final.zip) and [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BOBShield_Circuit.rar), respectively, while the ready-to-manufacture Gerber files can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/BoBShield_Gerber_Production_R1.zip).

![BOB_pcb](https://user-images.githubusercontent.com/18485913/101608410-f6839c80-3a05-11eb-9cbf-c662d25bd580.png)

# <a name="about"/>About

This shield was designed and created as a term project at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2019/2020.

# <a name="authors"/>Authors

* **Hardware and 3D model design:** Tibor Konkoly, Patrik Šíma, Marko Michal, Marek Krippel 
* **Software design:** Lukáš Vadovič, Matúš Bíro, Samuel Mladý
* **Wiki documentation:** Martin Gulan, Gergely Takács