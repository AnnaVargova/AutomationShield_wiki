# Introduction

The OptoShield belongs to the family of control systems engineering education devices for Arduino from the [AutomationShield project](https://www.automationshield.com). This low-cost shield contains a simple circuitry implementing a light emitting diode (LED) as the actuator and a light-dependent resistor (LDR) as a sensor. The LED and LDR are enclosed in an opaque tube that blocks ambient light. The power of the LED can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating its apparent brightness. The LED and LDR thus creates a simple feedback loop that can be used in control engineering experiments.

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

The board contains a second, independent LDR in addition to the one used as a sensor. This allows the user to test the functionality of the sensor and perform simple experiments. The auxiliary LDR can be read by calling 
The output from the LDR is read by 
```
Opto.sensorAuxRead();
```
and similarly to `Opto.sensorRead()` the function returns the voltage on the sensor circuit as a floating point parameter.

# System Identification 

```
Opto.step();
```

## Examples

Lorem ipsum dolor sit ameta

# Detailed Hardware Description

The OptoShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB  by a PCB fabrication service.

## Circuit design


## Parts

To make an OptoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Name             | Part | pcs.  | Value | Note |
|------------------|------|-------|------|------|
| Bright white LED |      |       |      |      |
| Potentiometer    |      |1      | 10k  |      |
| Photoresistor    |      |2      | 10k  |      |
| Resistor         |      |2      | 10k  |      |


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