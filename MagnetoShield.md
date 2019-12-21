#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[Initialization and calibration](#init)<br/>
&nbsp;&nbsp;&nbsp;[Input](#input)<br/>
&nbsp;&nbsp;&nbsp;[Output](#output)<br/>
&nbsp;&nbsp;&nbsp;[Auxiliary functions](#aux)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[Feedback control](#control)<br/>
&nbsp;&nbsp;&nbsp;[System identification](#ident)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>

# <a name="intro"/>Introduction

# <a name="api"/>Application programming interface

## <a name="init"/>Initialization and calibration

## <a name="input"/>Input

## <a name="output"/>Output

## <a name="aux"/>Auxiliary functions

# <a name="examples"/>Examples

## <a name="control"/>Feedback control

## <a name="ident"/>System identification

# <a name="hardware"/>Detailed hardware description

## <a name="circuit"/>Circuit design
The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the MagnetoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MotoShield_Circuit.zip). 

![magneto_circuit](https://user-images.githubusercontent.com/18485913/71308765-c14efa80-2400-11ea-88d3-fbe9317e9040.png)

Electromagnet is supplied by 12 V from pin Vin on the Arduino board. Schematic sign of electromagnet is not in the circuit schematics. Electromagnet is connected to the pins + and - in the right bottom corner of the schema. Supply voltage of the electromagnet is regulated by MOSFET type IRF520. Gate of the MOSFET is connected to the DA convertor PCF8591. Advantage of the DA convertor over a PWM signal is that DA convertor creates a real analog value of the voltage in range 0 to 5 V. The DA convertor is supplied by 5 V from the Arduino board and is controlled through I2C communication protocol.  For the I2C communication are used pins SDA and SCL. On these pins are connected two pull-up resistors too. Values of resistors are 10 k?. On the DA convertor supply pin and the output pin are connected two LED diodes too. There are also two resistors. The first one with value 270 ? is before LED diode connected to supply voltage. The second one is before LED diode connected to the output from the DA convertor and has value 1.2 k?. These diodes signalize that device is working. There is also bipolar hall sensor for controlling the position of the magnet.

## <a name="parts"/>Parts
To make a MagnetoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:


| Part             | Name            | Type/Value/Note                                                       | PCS |
|------------------|-----------------|-----------------------------------------------------------------------|-----|
| —   	           | 3D Print        | 5.7g Ø1.75mm PETG filament, bright green, at 240&deg;C (90&deg;C bed) | 1   |
| C1,C3            | Capacitor       | 0805, ceramic, 0.1µF                                                  | 2   |
| (h),C2           | Capacitor       | 0805, tantalum, 10µF                                                  | 1   |
| —   	           | Enclosure top   | clear acrylic; e.g. h=2 mm, stamped to the outer diameter of the tube | 1   |
| (m),U4           | Current sensor  | INA149                                                                | 1   |
| (b),U3           | [DAC](http://sk.farnell.com/nxp/pcf8591t-2-518/adc-single-8bit-11-1ksps-soic/dp/2400442RL?st=PCF8591)             | PCF8591T                                                              | 1   |
| (i),D1           | Diode           | DO214AC                                                               | 1   |
| (j),U2           | [Hall sensor](https://uk.rs-online.com/web/p/hall-effect-sensor-ics/6807119/)     | A1302KUA                                                              | 1   |
| —   	           | Header          | 6x1, female, 2.54mm pitch                                             | 1   |
| —   	           | Header          | 8x1, female, 2.54 mm pitch                                            | 2   |
| —   	           | Header          | 10x1, female, 2.54mm pitch                                            | 1   |
| (q),D2           | LED             | 0805, red                                                             | 1   |
| —   	           | [Magnet](https://www.ebay.com/itm/322722704471)          | NdFeB, disc, Ø8mm, h=2mm, N38                                         | 1   |
| (e),Q2           | MOSFET          | IRF520                                                                | 1   |
| —   	           | PCB             | 2 layer, FR4, 1.6mm thick                                             | 1   |
| (o),POT1         | Potentiometer   | 10kΩ                                                                  | 1   |
| (c),(f),R1,R2,R4 | Resistor        | 10kΩ, 0805                                                            | 3   |
| (n),(p),R6,R9    | Resistor        | 3kΩ, 0805, 0.1%                                                       | 2   |
| (p,q),R7         | Resistor        | 1kΩ, 0805, 0.1%                                                       | 1   |
| (f),R3           | Resistor        | 220Ω, 0805                                                            | 1   |
| —   	           | O-Ring          | rubber, M12, h=1mm, e.g. Ø18mm (outer)                                | 1   |
| —   	           | Screws          | polyamid, M3x8                                                        | 2   |
| —   	           | Shaft           | ACP CA9MA9005                                                         | 1   |
| (l),R8           | Shunt           | 10Ω, 0805, 0.1%                                                       | 1   |
| (g),L1           | Solenoid        | 220Ω, 0805                                                            | 1   |
| —   	           | Enclosure tube  | clear, Plexiglas XT, h=8mm, φ10mm (inner), φ12mm (outer)              | 1   |
| (k),D3–D5        | Zener diode     | 3.3V, SOD323                                                          | 3   |

Note that the total cost of the above components and thus of the entire MagnetoShield is no more than $9 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_gerber.zip).

![magneto_pcb](https://user-images.githubusercontent.com/18485913/71308119-abd5d280-23f8-11ea-9ded-0aa222dbafd0.png)

# <a name="about"/>About
This shield was designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI). The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018. The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Mihalik2018.pdf).

## <a name="authors"/>Authors
* Hardware design: Jakub Mihalík, Gergely Takács
* Software design: Jakub Mihalík
* Wiki: Martin Gulan, Gergely Takács


# Introduction

The MagnetoShield is a device designed for educational purposes of automatic control and is another product of an [AutomationShield](https://www.automationshield.com) project. This device is able to lift up and control position of a permanent magnet.   Thanks to these possibilities we are able to build up closed-loop regulation program (for instance PID regulator) and create levitation effect of the magnet. Device uses an electromagnet to generate a magnetic force which lifts up the permanent magnet. The position of the magnet is controlled by a Hall effect sensor. Because of a fast dynamic and complexity of the system device is great tool for learning and testing regulation algorithms.

[[/fig/Magneto/Magneto.png|Isometric photograph of the MagnetoShield.]]

# Library functions

In the next part below are defined functions which are part of the [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield) and are used for easier work with the MagnetoShield.

Initialization function defines which pin is used for data-transfer from hall sensor and initializes Wire library. Wire library is used for I2C communication between DA convertor and Arduino:

```
MagnetoShield.begin();
``` 

This function has as an argument float number in range 0.00 to 5.00 and sets exact output voltage on DA convertor: 

```
MagnetoShield.setVoltageV(float);
``` 

The function assigns voltage transformed into 10-bit value measured on hall sensor to the highest and the lowest possible position of the magnet. Thanks to this function we are able to use magnetic induction to determinate the position of the flying magnet. Values of the maximum and the minimum are in range 0 to 512. Chip on Arduino UNO has 10 bit resolution (0-1023) but because of using a bipolar Hall effect sensor values reach 512 maximally. This value represents measurement on Hall effect sensor without magnetic field:

```
MagnetoShield.calibration();
``` 

These functions return values discovered in calibration function as integer numbers:

```
MagnetoShield.getMin();
MagnetoShield.getMax();
``` 

The function returns current position of the magnet measured by hall sensor:

```
MagnetoShield.readHeight();
``` 

An argument of the function is desired position of levitation in percents:

```
MagnetoShield.setHeight(float);
``` 

The function sets output voltage from DA convertor. Difference between this function and setVoltageV is that previous function used as the argument float number 0.00-5.00 but this function uses 8-bit number in range 0 to 255. This version of controlling DA convertor is better to use by regulation programs:

```
MagnetoShield.setVoltage(float);
```

This function makes a difference between desired position of flying and current measured position. Returns back error which can be used in PID regulator for instance:

```
MagnetoShield.error();
``` 
# Hardware description

An area of levitation is surrounded by transparent tube enclosed on the top. These borders prevent the magnet from drawing up to electromagnet under the influence magnetic field of the permanent magnet and disable oscillations of the magnet in horizontal plane.  Parameters of the tube and the gate where is electromagnet located depend on used permanent magnet. In my case I used a neodymium disc with diameter 8 mm and 2 mm thick. By me used parameters you can see on the picture below.

[[/fig/Magneto/Branicka.png| Parameters of the transparent tube and the height of the electromagnet.]]

The MagnetoShield is a part of open-source project the AutoamtionShield. In the text below are circuit schematics, PCB layout and are described all components of the MagnetoShield. If you have ideas how to make hardware or software better please let us know, any other improvements are welcome.

