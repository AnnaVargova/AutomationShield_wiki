#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
&nbsp;&nbsp;&nbsp;[MATLAB API](#matlab)<br/>
&nbsp;&nbsp;&nbsp;[Simulink API](#simulink)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[Feedback control](#control)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction
The AeroShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. The basic design of the AeroShield consists of a small motor, mounted to the end of a pendulum, which has a Hall-effect rotational encoder with a magnet connected to it. The pendulum and the rotational encoder are supported by 3D printed parts. The goal is to control the angle of inclination of the pendulum by changing the lift created by the motor. The user may regulate motor thrust manually—using a potentiometer, or automatically by switching to pre-programmed trajectory. 

<img src="https://user-images.githubusercontent.com/92367957/171638147-279e70b0-1642-45a5-920b-348eecb4beaa.png" width="700"> 

Note, that in the assembly, four parts are 3D printed as shown in the picture below (from left to right): main body, pendulum connectors, motor holder and a magnet holder. At the moment new model of the pendulum body is being designed. Feel free to download the ready-to-print [parts](https://github.com/gergelytakacs/AutomationShield/files/8824121/AeroShield_readyToPrint.zip).

<img src="https://user-images.githubusercontent.com/92367957/171638286-62c036c3-7ee2-4045-b979-a0b4a6c38545.png" width="170"> 
<img src="https://user-images.githubusercontent.com/92367957/171638581-6f7e9fb3-7095-4fd1-824f-13d74b21afbb.png" width="210">
<img src="https://user-images.githubusercontent.com/92367957/184544749-fa6d400b-73bf-4445-9b48-608adc8e57b2.png" width="250"> 
<img src="https://user-images.githubusercontent.com/92367957/169500558-a874d23e-f622-4491-a1b3-aaa4bd5b91af.png" width="200"> 


 
# <a name="api"/>Application programming interface
## <a name="io"/>C/C++ API
The application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the AeroShield is included in the `AeroShield.h` header, which contains the `AeroClass` class that is constructed by default as the `AeroShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output - (°)(Hall effect sensor): `AeroShield.getRawAngle();`
* Output - (A)(Current monitor): `AeroShield.currentMeasure();` 
* Input -  (%)(Actuator): `AeroShield.actuatorWrite();`

<img src="ttps://user-images.githubusercontent.com/92367957/171638842-b0134654-f1c3-43b4-a338-6470c7ce974f.png" width="600"> 

The following subsections describe the methods used to access the input and outputs of the AeroShield. Note, that before you begin an experiment you must initialize the hardware by calling `AeroShield.begin();`, which launches the I2C interface and check if rotational encoder detects magnet needed for angle reading.

The rotational encoder provides angle readings in the form of a 12-bit unsigned integer, but for the ease of interpretation the output should be scaled to the range of 0–360°. Nevertheless, in some cases such as displaying runs on the Serial Plotter, scaling to the range of 0-100% is better for uniform scaling across all variables. 

After initializing the shield, the calibration procedure is called by `AeroShield.calibration();`, which reads the minimal and maximal angle and maps these values correspondingly to percentage [0-100%] / or angle [0-360°]. After calibration, the minimal angle value is stored in the form of global variable `startAngle` for later use.

### Output
The angle of the pendulum is read by `y=AeroShield.getRawAngle();` returning the raw angle, which is then converted into an angle reading in the range of 0-360 [°] by a mapping function. 

The current drawn by the motor is read by `y=AeroShield.currentMeasure();` which provides an averaged current reading in the form of a floating point number.  

### Input
By choosing the method of supplying the input power `u` in the range of 0–100 [%] to the `AeroShield.actuatorWrite(u);` method, the user can control the motor speed. This method also constrains the output value to avoid overflow and maps the input to a 8-bit PWM integer. The output of this method is then sent to the Arduino pin `D5`, which can deliver PWM signal to the PCB circuitry.

The reference set by the user is acquired from the potentiometer by calling `r=AeroShield.referenceRead();` returning the position of the potentiometer runner as a floating point scaled to 0–100 [%].

## <a name="matlab"/>MATLAB API
If you cannot program in C/C++ just yet, you may want to try out the MATLAB API for the AeroShield that enables to access the hardware through the [MATLAB](https://www.mathworks.com/downloads/) command line and scripts. It utilizes the [MATLAB Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/47522-matlab-support-package-for-arduino-hardware) which enables communication between the Arduino prototyping platform and the development computer.

To prevent confusion between the C/C++ and the MATLAB API, the two interfaces are as similar as possible. The MATLAB API is written in object-oriented script and the user must first create an instance from the class: 

`AeroShield=AeroShield;` 

The AeroShield device is then initialized by calling 

`AeroShield.begin();` 

which will simply load a server code to the microcontroller, unless it is not already present. This means that the closed-loop control by the API in MATLAB is not strictly real-time, since commands are transferred through the serial link between the board and the computer and may be affected by transfer speed or operating system behavior. However, being able to use the high-level MATLAB script allows you to run live experiments under this already familiar software platform and, most importantly, to create and test more advanced control frameworks with minimal effort.

The operation of the MATLAB API is otherwise completely identical to the Arduino version, mentioned before. Therefore, methods such as `calibrate()`, `actuatorWrite()` and `referenceRead()` are all implemented for the AeroShield in MATLAB library.

## <a name="simulink"/>Simulink API
An even more intuitive tool for creating control loops and perform live experiments is the Simulink API for the AeroShield. It utilizes the [Simulink Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware) which supplies algorithmic units in blocks that access the core hardware functionality.

All necessary functions of the AeroShield are built into separate blocks available for use through the [Simulink Library](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/AeroLibrary.slx) (see figure below) and may be combined with all the other available blocks to create feedback control applications.

The Simulink API custom library retains the naming convention and features for individual input and output blocks ('Reference Read', 'Actuator Write' and 'Angle Read') and a comprehensive block representing the entire device is also available ('AeroShield').

<img src="https://user-images.githubusercontent.com/92367957/171639001-bba37b57-6afa-4d0d-bcef-b3ea8200cc1c.png" width="700"> 

In direct contrast with the way MATLAB handles Arduinos, the block scheme in [Simulink](https://www.mathworks.com/downloads/) is transcribed into C/C++, then compiled to machine code and uploaded to the microcontroller unit (MCU). In other words, code is run directly on the microcontroller. Simulink not only transcribes the block schemes for hardware, it also maintains the connection between the development computer and MCU. This way controllers can be fine-tuned in a live session, or data may be displayed and logged conveniently.

# <a name="examples"/>Examples

## <a name="control"/>Feedback control
As a start, you may want to experiment with a closed-loop control of the pendulum position by the proportional–integral–derivative controller (PID) algorithm.

The implementation of PID control in C/C++ is demonstrated by an [example](https://github.com/gergelytakacs/AutomationShield/tree/master/examples/AeroShield/AeroShield_PID), which makes use of the interrupt-driven sampling subsystem of the AutomationShield library, and also its built-in input-saturated absolute-form PID method with integral windup handling by clamping. The progress of the experiments can be followed in real time through the Serial Plotter of the Arduino IDE or logged in MATLAB.

<img src="https://user-images.githubusercontent.com/92367957/171639090-781734d4-438f-466d-b430-7e3adf701234.png" width="880">

The same experiment can be conveniently launched from the MATLAB API as well, see the [worked example](https://github.com/gergelytakacs/AutomationShield/tree/master/matlab/examples/AeroShield/AeroShield_PID). Sampling and computing the control decision is performed by the PC, while the Arduino only acts as an I/O interface. The response shown in the figure below demonstrates that a consistent closed-loop behavior is expected even when resorting to MATLAB script.

<img src="https://user-images.githubusercontent.com/92367957/171639180-b17c29bc-edc4-48cf-8228-c28b855f9c2e.png" width="650">

The same feedback control loop can be built even easier using the Simulink API. Shown below is the full [block scheme](https://github.com/gergelytakacs/AutomationShield/tree/master/simulink/examples/AeroShield/AeroShield_PID) for discrete saturated PID control of the process. You only need to select the 'AeroShield' block from the API library to implement the input/output of the hardware and the 'Reference read' block to choose between manual or automatc trajectory. Other blocks, such as the 'Discrete PID Controller', can be readily selected from the Simulink's default library.

<img src="https://user-images.githubusercontent.com/92367957/171639290-d30a36b3-cd82-4ed0-8f8b-772fb00eef3c.png" width="700">

After selecting the External mode, the block scheme is transcribed into C/C++ code, which is then compiled to AVR-specific machine code and downloaded to the MCU. The application runs stand-alone on the MCU while providing basic interaction with the host PC. You may use switches, sliders and knobs to select reference levels and inspect the response live using a 'Scope'.

<img src="https://user-images.githubusercontent.com/92367957/171639370-39933777-311f-4904-90c6-7de18f0c332f.png" width="700">

Note that the provided MATLAB and Simulink APIs enable to exploit full high-level mathematic power of MATLAB in order to create and test more complex estimation and control algorithms. 

# <a name="hardware"/>Detailed hardware description
The AeroShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design
The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics of the AeroShield from [here](https://github.com/cenculis/Bakalarka/files/8740608/Scheme_AeroShield.zip).

<img src="https://user-images.githubusercontent.com/92367957/171639482-76d44b0d-d292-423b-b7ea-9a3373c14964.png" width="1000">
<img src="https://user-images.githubusercontent.com/92367957/171639491-3f31cbec-a0a8-47e6-ab4e-c3e83c8130a2.png" width="500">

The motor **M1** is powered by an N-channel MOSFET **Q1**, and driven by the D5 PWM capable microcontroller pin through an 100Ω current limiting resistor **R8**. Floating states are handled by a 10 kΩ pull-down resistor **R9**, while a diode **D1** ensures back electromotive-force (EMF) protection. A motor is connected to the connector **M1**. Since the motor requires 12 V and more current than the USB-powered Arduino can handle, a separate wall adapter power supply is required to operate the device. 

The operating voltage of the motor is 4,7 V, so a buck-down converter **U1** steps-down the voltage, using custom made resistors **R1/2/3**, capacitors **C1/2** and coil **L1** layout. 

Microchip **U2** is responsible for meassuring the current drawn by the motor. It is done so, by comparing the voltage drop across the shunt resistor **R4**. On the output of the current monitor there is a 10 kΩ pull-down resistor **R5**. On both V+ and VIN+ there are a filter capacitors **C3/4**.

The angle encoder is integrated to custom made breakout board, with two capacitors **C1/2**, and is connected to the I2C bus connectors of the Arduino (SDA,SCL) with a connector **J1/2**. Angle is read by hall sensor AS5600 **U3** which also needs two 4.7 kΩ pull-up resistors **R6/7** on the I2C connections. 

Last but not least, the potentiometer **POT1** runner is attached to the A3 ADC capable pin of the board.

## <a name="parts"/>Parts
To make an AeroShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| part      | name                | Type/Value/Note          | PCS   |
|---------- |-------------------- |--------------------------|-------|
| C1-4 +2   | Capacitor           | SMD, sot23               | 6     |
| D1        | Diod                | 1N400IG                  | 1     | 
| J1        | FFC connector       | FFC 4pin                 | 2     | 
| L1        | Coil                | IND1210                  | 1     | 
| M1        | Motor connector     | JST-XH 2,54              | 1     |  
| M1        | Motor               | Howellp 7x20mm Motor     | 1     | 
| POT1      | Potentiometer       | CA14                     | 1     | 
| Q1        | Mosfet              | pmv45en2                 | 1     | 
| R1-9      | Resistor            | SMD, sot23               | 9     |  
| U1        | Buck converter      | TPS56339                 | 1     |  
| U2        | Shunt monitor       | INA169/NA                | 1     | 
| U3        | Hall sensor         | AS5600                   | 1     |
| -         | 3D Printed parts    | Body, connectors         | 4     |
| -         | Ball bearings       | BB-694-B180-30-ES IGUS   | 2     |
| -         | FFC cables          | 4 pin, min. length 15cm  | 1     |
| -         | Motor cables        | min. length 35cm         | 1     | 
| -         | Bolts               | 4x M3x40 4x M4x15        | 8     | 
| -         | Carbone tubes       | 2x10cm Øint: 4mm         | 2     | 
| -         | PCB shield          | JLCPCB                   | 1     |   
| -         | PCB brakout         | JLCPCB                   | 1     | 
| -         | Nuts                | M4                       | 4     |

Note that the total cost of the above components and thus of the entire AeroShield is no more than $25 excluding labor, postage and soldering material.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/files/8824259/Scheme_AeroShield.zip), respectively, while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/files/8824254/PCB_AeroShield_gerber.zip).


The assembled shield is fixed mechanically and electrically to the Arduino board through stackable header pins. The location of the components is also illustrated in the PCB layout below. The pendulum body is attached through 4 pre-drilled holes made in the manufacturing process.  

<img width="400" alt="pcbFront" src="https://user-images.githubusercontent.com/92367957/171640017-257f9730-7933-43dc-b859-c6a584ab54bb.png">
<img width="400" alt="pcbBack" src="https://user-images.githubusercontent.com/92367957/171640015-b1f4ba0d-4261-4057-9ce1-0551d9962055.png">
<img width="250" alt="pcbBreakoutFront" src="https://user-images.githubusercontent.com/92367957/171640020-cb860d15-e6b9-4ddf-b16b-c4c352bc1ddb.png">
<img width="250" alt="pcbBreakoutBack" src="https://user-images.githubusercontent.com/92367957/171640018-47f52e6f-9d3e-4d1f-ba76-3981ca17e30b.png">

# <a name="about"/>About
This shield was designed and created as a bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2021/2022.

## <a name="authors"/>Authors
* 3D-model design: Peter Tibenský
* Hardware design: Ján Boldocký, Erik Mikuláš, Denis Skokan, Peter Tibenský, Anna Vargová, Dávid Vereš, Tadeas Vojtko and Gergely Takács
* Software design: Peter Tibenský, Anna Vargová and Gergely Takács
* Wiki documentation: Peter Tibenský, Erik Mikuláš and Gergely Takács