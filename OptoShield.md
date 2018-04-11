# Introduction

The OptoShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. This particular low-cost shield contains a simple circuitry implementing a light emitting diode (LED) as the actuator and a light-dependent resistor (LDR) as a sensor. The LED and LDR are enclosed in an opaque tube that blocks ambient light. The power of the LED can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating its apparent brightness. The LED and LDR thus creates a simple feedback loop that can be used in control engineering experiments.

[[/fig/Opto_Iso.jpg|Isometric photograph of the OptoShield.]]
[[/fig/Opto_Front.jpg|A photograph of the OptoShield from the front.]]

# Library functions

All functions and examples associated to the OptoShield are included in the [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). The functions specific to this shield are included in the `Opto` class of functions, these mostly perform input/output peripheral communication. [Certain library functions](https://github.com/gergelytakacs/AutomationShield/wiki/Common-functions) are common to all of our shields and are included in the `AutomationShield` class. 

The summary of functions and the illustration below should get you started quickly:
* Output (sensor): `Opto.sensorRead();` 
* Input  (actuator): `Opto.actuatorWrite();` 
* Reference (setpoint): `Opto.referenceRead();` 

[[/fig/Opto_Functions.gif|A quick-start guide for the Opto functions.]]

## Inputs and outputs

The following sections describe the methods used to access the input and output of the OptoShield.

### Input
The input to the actuator LED (along with the auxiliary LED) is written by 
```
Opto.actuatorWrite(u);
```
where `u` is a floating point number from the range of 0-100 %. This will send a PWM signal to both LEDs. The behavior of the visible LED is mirrored in the one located in the tube.

### Output
The output from the LDR is read by 
```
Opto.sensorRead();
```
where the function outputs the brightness in the range of 0-100 % as detected by the sensor circuit. This reading can be used as a feedback signal. The function returns a floating point value. 

Note that the sensor reading in percents is only available after the system has been calibrated. The sensor can be calibrated by calling
```
Opto.calibrate();
```
in the `setup()` function, after which the `Opto.sensorRead()' will return the output readings in the correct percentual range.

A sensor reading can be requested instead of calibrated percents directly in units of voltage by calling
```
Opto.sensorReadVoltage();
```
which will return a floating point number. 

The board contains a second, independent LDR in addition to the one used as a sensor. This allows the user to test the functionality of the sensor and perform simple experiments.  The output from the auxiliary LDR is read by 
```
Opto.sensorAuxRead();
```
and similarly to `Opto.sensorReadVoltage()` the function returns the voltage on the sensor circuit as a floating point parameter. Note that this sensor is merely for testing and calibration, like in the case of the main sensor, is not possible.

### Reference

The user reference is read from the potentiometer by calling
```
Opto.referenceRead();
```
and the function returns the desired user reference setpoint in percents in the range of 0-100 \% as a floating point number.


## System Identification 

The functions listed below implement tests signals that can be used for system identification and modeling. The functions, when called without input parameters, run a specific identification experiment with pre-set parameters and length that we deemed suitable for identification. Upon evaluating the function, the Arduino board will start to list the results to the serial communication port. The formatting corresponds to a space-separated table format, where spaces separate columns and the line ending character begins a new line.

The results can be listed using the Arduino Serial Monitor, the Arduino IDE Serial Plotter or even logged by a number of [third party applications](http://freeware.the-meiers.org/), then exported to other software for visualization and post-processing.

### Step response
```
Opto.step();
```

### Impulse response
```
Opto.impulse();
```

# Examples

Lorem ipsum dolor sit

## Step Response

Lorem ipsum dolor sit 

## PID Control

Lorem ipsum dolor sit

# Detailed Hardware Description

The OptoShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB  by a PCB fabrication service.

For those who wish to use the board without the library, the components are connected to the following pins:

[[/fig/Opto_PIN.gif|OptoShield PIN assignments.]]


## Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the OptoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Circuit.zip).

[[/fig/Opto_Schematics.png|OptoShield Circuit Schematics.]] 


## Parts

To make an OptoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|Part              | Name             | Value | PCS  | Note                       |
|------------------|------------------|-------|------|----------------------------|
| LDR1             | Photoresistor    | 10 k  | 1    | 5.5 mm                     |
| LDR2             | Photoresistor    | 10 k  | 1    | 5.5 mm                     |
| D1               | LED              |       | 1    | 5 mm, bright white         |
| D2               | LED              |       | 1    | 5 mm, bright white         |
| D3               | LED              |       | 1    | 1.8 mm, 2.2-2.5V / 10mA    |
| POT1             | Potentiometer    | 10 k  | 1    | Pin size 12.5x10, [URL](https://www.tme.eu/sk/Document/a8800d4bf548c3723171950d7cc2898f/ACP_CA14-CE14.pdf)|
| R1               | Resistor         | 10 k  | 1    |                            |
| R2               | Resistor         | 10 k  | 1    |                            |
| R3               | Resistor         | 270   | 1    |                            |
| R4               | Resistor         | 270   | 1    |                            |
| R5               | Resistor         | 270   | 1    |                            |
|                  | Header           | 10 pin| 1    | long, stackable            |
|                  | Header           | 8 pin | 2    | long, stackable            |
|                  | Header           | 6 pin | 1    | long, stackable            |
|                  | Tube             | 20 mm | 1    | opaque, 5 mm inner dia.    |

## PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Gerber.zip).

[[/fig/Opto_PCB_Front.png|OptoShield PCB from the front.]]
[[/fig/Opto_PCB_Back.png|OptoShield PCB from the back.]]

# About

The board was developed within the framework of a bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics of the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2017/2018. 

## Authors

* Hardware design: Tibor Konkoly, Martin Gulan, Gergely Takács
* Software design: Tibor Konkoly, Gergely Takács
* Wiki: Gergely Takács