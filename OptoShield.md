#### Contents
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
&nbsp;&nbsp;&nbsp;[Simulink API](#simulink)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[System identification](#ident)<br/>
&nbsp;&nbsp;&nbsp;[PID control](#control)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction

The OptoShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. This particular low-cost shield contains a simple circuitry implementing a light emitting diode (LED) as the actuator and a light-dependent resistor (LDR) as a sensor. The LED and LDR are enclosed in an opaque tube that blocks ambient light. The power of the LED can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating its apparent brightness. The LED and LDR thus creates a simple feedback loop that can be used in control engineering experiments.

![Opto_Iso](https://user-images.githubusercontent.com/18485913/57761885-bd3a5c00-76fe-11e9-8b29-d3ccd2b5b196.png)

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. The OptoShield API is located in the `OptoShield.h` header and a corresponding source file; these implement the `OptoClass` class which is then declared as default `OptoShield` instance. The functions specific to this shield mostly perform input/output peripheral communication. [Certain library functions](https://github.com/gergelytakacs/AutomationShield/wiki/Common-functions) are common to all of our shields and are included in the `AutomationShield` class. 

The summary of functions and the illustration below should get you started quickly:
* Output (sensor): `Opto.sensorRead();` 
* Input  (actuator): `Opto.actuatorWrite();` 
* Reference (setpoint): `Opto.referenceRead();` 

[[/fig/Opto_Functions.gif|A quick-start guide for the Opto functions.]]

## Inputs and outputs

The following sections describe the methods used to access the input and output of the OptoShield.
Note that before you begin an experiment you must initialize the hardware by calling

`OptoShield.begin();`

which determines the mode of the input and output pins.

This must be followed by

`OptoShield.calibrate();`

to re-scale the input and output values.
Because the LDR cannot measure physically valid units, the signal to the LED and from the LDR will be given in percents of the full scale. The method `calibrated()` thus finds the minimal and maximal analog-to-digital converter (ADC) levels measured at the sensor. The `returnCalibrated()` method returns a flag informing on the calibration state, while the `returnMinVal()` and `returnMaxVal()` method return the sensor calibration levels.

### Input

The input to both actuator LED an auxiliary LED is written by calling the method

`OptoShield.actuatorWrite(u);`

which accepts input `u` as a floating-point number in the range of 0-100 %. This will send a PWM signal to both LEDs. The behavior of the visible LED is mirrored in the one located in the tube.

### Output

The output from the LDR is read by calling

`y = OptoShield.sensorRead();`

which returns a floating-point number in the calibrated range of 0-100 % representing process output `y`. The voltage at the main sensor is accessible through the `sensorReadVoltage()` method as well, while the auxiliary sensor can be accessed via `sensorAuxRead()`.

### Reference

The onboard potentiometer can be used in any role, but the most straightforward one is to read a user-defined setpoint `r`  using

`r = OptoShield.referenceRead();`

which returns a floating-point number between 0-100\%.

## <a name="simulink"/>Simulink API

If you cannot program in C/C++ just yet, you may want to try out the Simulink API for the OptoShield that enables to create control loops and perform live experiments in the [Simulink](https://www.mathworks.com/products/simulink.html) environment. It utilizes the [Simulink Support Package for Arduino Hardware ](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware) which supplies algorithmic units in blocks that access the hardware functionality. The block scheme in [Simulink](https://www.mathworks.com/downloads/) is transcribed into C/C++, then compiled to machine code and uploaded to the microcontroller unit (MCU). In other words, code is run directly on the microcontroller. Simulink not only transcribes the block schemes for hardware, it also maintains the connection between the development computer and MCU. This way controllers can be fine-tuned in a live session, or data may be displayed and logged conveniently.

The Simulink API offers the following algorithmic blocks:
![OptoSimulink](https://user-images.githubusercontent.com/18485913/57798916-2e553000-774e-11e9-8749-4d8769ff7176.png)

The 'Actuator Write' block accepts real numbers from 0-100% and supplies power to the onboard LEDs.

The 'Sensor Read' and 'Sensor Aux. Read' blocks read the input from the LDR and its auxiliary twin, respectively. User may select the desired type of outputs such as voltage, ADC levels or a manually calibrated signal in percentages. The onboard potentiometer may be accessed in a similar fashion using the 'Reference Read' block.

The 'OptoShield' block unites the input and output functionality into a single entity that can be conveniently used for identification and control experiments. The block automatically calibrates the sensor to the available range, then accepts a saturated input signal in the range of 0-100 % and reads the calibrated brightness signal from the main LDR. One may also use the 'OptoShield TF' block to model the process dynamics by a continuous linear transfer function for simulation-only exercises. 

# <a name="examples"/>Examples

## <a name="ident"/>System identification

Input-output experiments for data gathering can be launched, displayed and logged in C/C++ (Arduino IDE) and Simulink as well. The AutomationShield library for OptoShield incorporates several worked examples. For example, one [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/OptoShield/OptoShield_StepResponse/OptoShield_StepResponse.ino) illustrates the input-output step response of the onboard optical system. A more [detailed example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/OptoShield/OptoShield_Identification/OptoShield_Identification.ino) runs through a pre-set array of open-loop inputs, incorporates an interrupt-based sampling system and lists the results to the serial communication line. The dynamic response of the system can be followed through the built-in Serial Plotter of the Arduino IDE. This enables one to display the process dynamics within the development environment itself.

![ide](https://user-images.githubusercontent.com/18485913/57801853-061cff80-7755-11e9-8055-8543ae303ecc.png)

The results can be also listed using the Arduino Serial Monitor or even logged by a number of [third party applications](http://freeware.the-meiers.org/), then exported to other software for visualization and post-processing.

Apart from this simple graphical representation, the library includes a [function](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/OptoShield/plotOptoShield_Step.m) to read the results to the [MATLAB](https://www.mathworks.com/products/matlab.html) workspace. This way you can perform system identification procedures on the collected data. The figure below shows the input and output data gathered through the latter example.

![identification](https://user-images.githubusercontent.com/18485913/57801799-db32ab80-7754-11e9-81ab-3bd4efecf683.png)

One may, for example, identify a continuous-time first-order process model using the MATLAB's [System Identification Toolbox](https://www.mathworks.com/products/sysid.html) and compare the model to the measurement results. As a result, the first-order transfer function

<img src="http://latex.codecogs.com/gif.latex?G(s)=\frac{3.35}{1+0.0041s} ," border="0"/>

with a 
<img src="http://latex.codecogs.com/gif.latex?K" border="0"/>
=3.35 (-) DC gain and
<img src="http://latex.codecogs.com/gif.latex?T=0.0024 \mathrm{s}" border="0"/>
time constant produces a ∼96.5% match with measurement data, as long as we compare a unit step from zero level up to the expected working range of the optical tunnel.




## <a name="control"/>PID control

![pidoutput](https://user-images.githubusercontent.com/18485913/57802153-a7a45100-7755-11e9-8edf-619efc03122a.png)
![PIDSimulink](https://user-images.githubusercontent.com/18485913/57802158-aa06ab00-7755-11e9-81db-1e2478f02064.png)
![scope](https://user-images.githubusercontent.com/18485913/57802256-d91d1c80-7755-11e9-895e-168906ccd453.png)

# <a name="hardware"/>Detailed hardware description

The OptoShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB  by a PCB fabrication service.

For those who wish to use the board without the library, the components are connected to the following pins:

[[/fig/Opto_PIN.gif|OptoShield PIN assignments.]]


## <a name="circuit"/>Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the OptoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Circuit.zip). 

![scheme](https://user-images.githubusercontent.com/18485913/57755559-814cca00-76f1-11e9-8bfa-f4fed6276e9b.png)

The current to both the main LED **D1** and its external duplicate **D2** is limited by series resistors **R3** and **R4**. The value of these resistors has been chosen so as to create a maximum brightness that is within the sensing range of the LDR. Both LED are connected to digital output **D3** of the MCU, therefore the brightness of the external LED copies that of the main one.

The main LDR acting as a sensor **LDR1** is connected to the **A1** analog input of the MCU through a voltage divider configuration using resistor **R1**. Its value has been chosen to increase the resolution at the output measurement when combined with the brightness of the LED. This is repeated for the external sensor with **LDR2** and **R2** connected to the input **A2**. Finally, the potentiometer is connected to the **A0** input of the MCU, while the power LED with its series resistor is omitted from the scheme.

## <a name="parts"/>Parts

To make an OptoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|Part       | Name             | Type/Value/Note                | PCS |
|-----------|------------------|--------------------------------|-----|
| LDR1,LDR2 | [Photoresistor](https://www.tme.eu/sk/details/pgm5516-mp/fotorezistory/token/) |5-10kΩ, 5mm | 2 |
| D1,D2     | LED              | 5mm, clear                     | 2 |
| POT1      | [Potentiometer](https://www.tme.eu/sk/Document/a8800d4bf548c3723171950d7cc2898f/ACP_CA14-CE14.pdf) |                             10kΩ, pin size 12.5x10 | 1 |
| R1,R2     | Resistor         | 2.4kΩ                          | 2 |
| R3,R4     | Resistor         | 4.7kΩ                          | 2 |
| -         | Header           | 10x1, female, long, stackable  | 1 |
| -         | Header           | 8x1, female, long, stackable   | 2 |
| -         | Header           | 6x1, female, long, stackable   | 1 |
| -         | Tube             | 20mm, Ø5mm, opaque             | 1 |

Note that the total cost of the above components, including the PCB, and thus of the entire OptoShield is less than $3 excluding labor and postage.

## <a name="pcb"/>PCB

The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100x100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/OptoShield_Gerber.zip).

![PCBtop](https://user-images.githubusercontent.com/18485913/57757206-3cc32d80-76f5-11e9-98f9-5817f5af3eb1.png)
![PCBbottom](https://user-images.githubusercontent.com/18485913/57757207-3df45a80-76f5-11e9-8446-6a6815e3f356.png)

# <a name="about"/>About
This shield was designed and created within the framework of a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics in 2017/2018. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava. 

## <a name="authors"/>Authors
* Hardware design: Tibor Konkoly, Martin Gulan, Gergely Takács
* Software design: Tibor Konkoly, Gergely Takács
* Wiki: Tibor Konkoly, Gergely Takács