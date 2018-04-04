# Introduction

The OptoShield belongs to the family of control systems engineering education devices for Arduino from the [AutomationShield project](https://www.automationshield.com). This low-cost shield contains a simple circuitry implementing a light emitting diode (LED) as the actuator and a light-dependent resistor (LDR) as a sensor. The LED and LDR are enclosed in an opaque tube that blocks ambient light. The power of the LED can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating its apparent brightness. The LED and LDR thus creates a simple feedback loop that can be used in control engineering experiments.
[[/fig/Opto_Front.jpg|A photograph of the OptoShield from the front.]]
[[/fig/Opto_Iso.jpg|Isometric photograph of the OptoShield.]]
# Library functions

All functions and examples belonging to the OptoShield are included in the [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). 

## Inputs and outputs

The input to the actuator LED (along with the auxiliary LED) is written by 
```
Opto.actuatorWrite(u);
```
where `u` is a floating point number from the range of 0-100 %. This will send a PWM signal to both LEDs. The behavior of the visible LED is mirrored in the one located in the tube.

The output from the LDR is read by 
```
Opto.sensorRead();
```
where the function outputs the voltage detected on the sensor circuit that can be used as a feedback signal. The function returns a floating point value. 

The user reference is read from the potentiometer by calling
```
Opto.referenceRead();
```
and the function returns the desired user reference setpoint in percents in the range of 0-100 \% as a floating point number.

The board contains a second, independent LDR in addition to the one used as a sensor. This allows the user to test the functionality of the sensor and perform simple experiments. The auxiliary LDR can be read by calling 
The output from the LDR is read by 
```
Opto.sensorAuxRead();
```
and similarly to `Opto.sensorRead()` the function returns the voltage on the sensor circuit as a floating point parameter.

## System Identification 

```
Opto.step();
```

## Examples

Lorem ipsum dolor sit

# Detailed Hardware Description

The OptoShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB  by a PCB fabrication service.

## Circuit design

The circuit schematics has been designed in  in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the OptoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Circuit.zip).

[[/fig/Opto_Circuit.png|OptoShield Circuit Schematics.]]


## Parts

To make an OptoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|Part              | Name             | Value | PCS  | Note                       |
|------------------|------------------|-------|------|----------------------------|
| LDR1             | Photoresistor    | 10 k  | 1    | 5 mm                       |
| LDR2             | Photoresistor    | 10 k  | 1    | 5 mm                       |
| D1               | LED              |       | 1    | 5 mm, bright white         |
| D2               | LED              |       | 1    | 5 mm, bright white         |
| POT1             | Potentiometer    | 10 k  | 1    | Pin size 12.5x10, [URL](https://www.tme.eu/sk/Document/a8800d4bf548c3723171950d7cc2898f/ACP_CA14-CE14.pdf)|
| R1               | Resistor         | 10 k  | 1    |                            |
| R2               | Resistor         | 10 k  | 1    |                            |
| R3               | Resistor         | 220   | 1    |                            |
| R4               | Resistor         | 220   | 1    |                            |
|                  | Header           | 10 pin| 1    | long, stackable            |
|                  | Header           | 8 pin | 2    | long, stackable            |
|                  | Header           | 6 pin | 1    | long, stackable            |
|                  | Tube             | 20 mm | 1    | opaque, 5 mm inner dia.    |

## PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Gerber.zip).


The PCB from the front:
[[/fig/Opto_PCB_Front.png|OptoShield PCB from the front.]]

..and from the back:
[[/fig/Opto_PCB_Back.png|OptoShield PCB from the back.]]

# About

The board was developed within the framework of a bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics of the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2017/2018. 

## Authors

* Hardware design: Tibor Konkoly, Martin Gulan, Gergely Takács
* Software design: Tibor Konkoly, Gergely Takács