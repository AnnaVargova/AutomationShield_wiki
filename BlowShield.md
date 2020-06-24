#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[Feedback control](#control)<br/> 
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction
The BlowShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. The basic design of BlowShield consists of a vessel with integrated pressure sensor that is mapping pressure in the vessel and a pump. The goal is to obtain and keep the required value of pressure in the vessel despite its leakage. The value can be set by the user using the potenciometer. The power of the pump can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating airflow. The pump is an actuator and with the sensor it creates a simple single-input single-output (SISO) feedback loop that can be used in control engineering experiments.

<img width="500" alt="blow_photo" src="https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_photo.jpg">

<!--Before you proceed, you may also want to watch a short video tutorial_ [here](https://www.youtube.com/watch?v=RHkqxbUVUrw).-->

For a better visualization the entire assembly was 3D-modeled (see the illustration below) using the [DIPTrace](https://diptrace.com/) CAD software and can be downloaded from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_assembly.zip). Note that there are four parts to be 3D printed—a pump holder, a pump cap, a vessel and a backplate. One may also use a spiral fill structure and support parts for hollow objects. Feel free to download the [parts](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_3D_Parts.zip). The remaining, purchased assembly parts— [Arduino Uno](https://grabcad.com/library/arduino-uno-r3-4)—are downloadable from the GrabCAD database.

![blow_assembly](https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_assembly.png)

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API
The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the BlowShield is included in the `BlowShield.h` header, which contains the `BlowClass` class that is constructed by default as the `BlowShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions should get you started quickly:
* Output (sensor): `BlowShield.sensorRead();` 
* Input  (actuator): `BlowShield.actuatorWrite();`

The following subsections describe the methods used to access the input and output of the BlowShield. Note that before you begin an experiment you must initialize the hardware by calling

`BlowShield.begin();`

which launches the I2C interface and starting the pressure sensor itself.

Although the sensor provides pressure readings directly in pascals, the outputs should be scaled to the range of 0–
100% in order to use the Serial Plotter functionality, where all outputs are scaled to the same axis. The calibration
procedure is called by

`BlowShield.calibrate();`

finding the minimal pressure readings. The max pressure value is fixed to 106000Pa due to component protection. Then map these values to percentages. 

### Input
The actual pressure in the vessel is read by

`y=BlowShield.sensorRead();`

returning the scaled pressure inside the vessel as a floating point value from within the range of 0–100 (%). 

### Output
By supplying the input `u` in the range of 0–100 (%) to the

`BlowShield.actuatorWrite(u);`

method, the user can set the power sent to the pump through the power circuitry. This method checks for constraints to
avoid overflow, maps the input to 8-bit PWM integers, then sends it to the D11 pin of the Arduino.

Finally, user reference from the potentiometer is acquired by calling

`r=BlowShield.referenceRead();`

returning the position of the potentiometer runner as a floating point scaled to 0–100 (%).

# <a name="examples"/>Examples

## <a name="control"/>Feedback control
For a start you may want to experiment with a closed-loop control of the pressure in the vessel by the proportional–integral–derivative controller (PID) algorithm.

The implementation of PID control in C/C++ is demonstrated by a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/BlowShield/BlowShield_PID/BlowShield_PID.ino). 
which makes use of the interrupt-driven sampling subsystem of the AutomationShield library, and also its built-in input-saturated absolute-form PID method with integral windup handling by clamping. The progress of the experiments can be followed in real time through the Serial Plotter of the Arduino IDE. 

![blow_pid](https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_PID.png)

# <a name="hardware"/>Detailed hardware description
The BlowShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design
The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics of the FloatShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_Circuit.zip).

![float_pcb](https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_Schematics.bmp)

The pressure sensor is only represented by its connectors, which are connected to I2C communication pins of the Arduino and 3,3V power source and GND. 

The pump is powered by an N-channel MOSFET **Q1 **, and driven by the D11 PWM capable microcontroller pin through an 150 Ω current limiting resistor **R1 **. Floating states are handled by a 10 kΩ pull-down resistor **R2 **. A diode **D1 ** protects the microcontroller from reverse current caused by possible back electromotive force (EMF), while transient effects on the servo supply are filtered by a capacitor .

Finally, the potentiometer **POT1 ** runner is attached to the A0 ADC capable pin of the board, that allows the user to program this input for any purpose, such as providing reference to the feedback control loop.


## <a name="parts"/>Parts
To make a BlowShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Part | Name                                                                                                             | Type/Value/Note                               | PCS |
|------|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|-----|
| (b)  | PCB                                                                                                              | FR4, 2 layer, 1.6mm thick                     | 1   |
| (c)  | [Pump](https://www.gme.sk/vakuove-mikro-cerpadlo-20kpa#product-detail)                                           | Miniature vacuum pump: 3V, 160mA, 30kPa       | 1   |
| (d)  | [Pressure Sensor](https://www.gme.sk/i2c-senzor-tlaku-a-teploty-bmp280-3-3v)                                     | BMP280, I2C, pressure sensore module          | 1   |
| (e)  | [N-Mosfet](https://www.tme.eu/sk/details/irf3710pbf/tranzistory-s-kanalom-n-tht/infineon-irf/)                   | IRF 3710, TO-220, e.g. IRF3710PBF             | 1   |
| (f)  | [Capacitor](https://www.tme.eu/sk/details/t491a106m016at/tantalove-kondenzatory-smd/kemet/)                      | Tantalum, SMD, 10uF                           | 1   |
| (g)  | [Diode](https://www.tme.eu/sk/details/rs1d-e3_61t/univerzalne-diody-smd/vishay/)                                 | DO-214AC(SMA)                                 | 1   |
| (h)  | [Resistor](https://www.tme.eu/sk/en/details/0805s8j0103t5e/0805-smd-resistors/royal-ohm/)                        | SMD 10kΩ                                      | 1   |
| (i)  | [Resistor](https://www.tme.eu/sk/en/details/0805s8j0151t5e/0805-smd-resistors/royal-ohm/)                        | SMD 150kΩ                                     | 1   |
| (j)  | [Potentiometer](https://www.tme.eu/sk/details/ca9ma5-b/gombiky-pre-montazne-potenciometre/acp/ca9ma-9005-black/) |                                               | 1   |
| (k)  | [Potentiometer](https://www.tme.eu/sk/en/details/ca9mv-10k/single-turn-tht-trimmers/acp/ca9mv-10k/)              | 10kΩ                                          | 1   |
| (s)  | Header                                                                                                           | 10x1 pin, female, long, stackable, 0.1" pitch | 1   |
| (s)  | Header                                                                                                           | 8x1 pin, female, long, stackable, 0.1" pitch  | 2   |
| (s)  | Header                                                                                                           | 6x1 pin, female, long, stackable, 0.1" pitch  | 1   |
| (l)  | Vessel                                                                                                           | 3D printed                                    | 1   |
| (m)  | Backplate                                                                                                        | 3D printed                                    | 1   |
| (n)  | Pump holder                                                                                                      | 3D printed                                    | 1   |
| (o)  | Pump cap                                                                                                         | 3D printed                                    | 1   |
| (p)  | Screw                                                                                                            | M3x16                                         | 4   |
| (q)  | Nut                                                                                                              | M3                                            | 4   |
| (r)  | Self-tapping screw                                                                                               | M3x8                                          | 4   |
| (t)  | Tube                                                                                                             | 4x110mm                                       | 1   |
| (u)  | Cable                                                                                                            | 70mm                                          | 2   |
| (v)  | Gasket                                                                                                           |                                               | 1   |



Note that the total cost of the above components and thus of the entire FloatShield is no more than $30 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_PCB.zip) and [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_Circuit.zip), respectively, while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_Gerber.zip).

The assembled shield is fixed mechanically and electrically to the Arduino board through stackable header pins. The location of the each component is illustrated in the PCB layout below. The pump is mounted to the board on holder and covered by cap. The sensor is situated in the vessel. Backplate under the PCB board is also used for reinforcement the structure. Due to small space between the vessel and the pump, the suction pipe of the pump must be shortened (at least half of the original length). The connection of the tube with the vessel needs to be sealed up, one may use superglue or epoxy glue.

<img width="500" alt="pcbfront" src="https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_Board_Front.bmp">
<img width="500" alt="pcbback" src="https://github.com/gergelytakacs/AutomationShield/wiki/fig/Blow/Blow_Board_Bottom.bmp">

# <a name="about"/>About
This shield was designed and created as a term project at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2019/2020.

## <a name="authors"/>Authors
* 3D-model design: Tomáš Tužinský
* Hardware design: Martin Staroň, Martin Vríčan, Lukáš Kavoň
* Software design: Anna Vargová, Eva Vargová
* Wiki documentation: Vladimír Kmeť
* Postman: Matúš Ľeginus
* Support: Radoslav Gago