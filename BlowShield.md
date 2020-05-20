#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
&nbsp;&nbsp;&nbsp;[MATLAB API](#matlab)<br/>
&nbsp;&nbsp;&nbsp;[Simulink API](#simulink)<br/>
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
The BlowShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. <span style="color:red">The basic design of BlowShield consists of a vessel with integrated pressure sensor that is mapping pressure in the vessel and a pump. The goal is to obtain and keep the required value of pressure in the vessel despite its leakage. The value can be set by the user using the potenciometer. The power of the pump can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating airflow.The pump is an actuator and with the sensor it creates a simple single-input single-output (SISO) feedback loop that can be used in control engineering experiments.</span>

<span style="color:red">Before you proceed, you may also want to watch a short video tutorial</span> [here](https://www.youtube.com/watch?v=RHkqxbUVUrw).

![FloatShield](https://user-images.githubusercontent.com/18485913/71251627-26342300-2323-11ea-9496-1c5d9f2872e3.png)

<span style="color:red">For a better visualization the entire assembly was 3D-modeled (see the illustration below) using the CAD software CATIA V5R20 (Student Edition) and can be downloaded from</span> [here](https://github.com/gergelytakacs/AutomationShield/files/1942312/assembly.zip). Note that there are three parts to be 3D printed—a tube clamp, a tube lid and a sensor holder. One may also use a honeycomb tube insert to make the turbulent air flow relatively laminar. Feel free to download the ready-to-print [parts](https://github.com/gergelytakacs/AutomationShield/files/1977698/parts.zip). The remaining, purchased assembly parts—the [fan](https://grabcad.com/library/fan-40-x-40-x-28-1) and [Arduino Uno](https://grabcad.com/library/arduino-uno-r3-4)—are downloadable from the GrabCAD database.

![float_assembly](https://user-images.githubusercontent.com/18485913/71249052-a5722880-231c-11ea-9ffb-a4f2b7242047.png)

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API
The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the FloatShield is included in the `FloatShield.h` header, which contains the `FloatClass` class that is constructed by default as the `FloatShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output (sensor): `FloatShield.sensorRead();` 
* Input  (actuator): `FloatShield.actuatorWrite();`

![float_actsens](https://user-images.githubusercontent.com/18485913/71261365-78823d80-233d-11ea-8258-1c32df398e05.png)

The following subsections describe the methods used to access the input and output of the FloatShield. Note that before you begin an experiment you must initialize the hardware by calling

`FloatShield.begin();`

which launches the I2C interface and starting the time of flight (TOF) sensor itself.

Although the sensor provides distance readings directly in millimeters, the outputs should be scaled to the range of 0–
100% in order to use the Serial Plotter functionality, where all outputs are scaled to the same axis. The calibration
procedure is called by

`FloatShield.calibrate();`

finding the minimal and maximal distance readings and map these values to percentages. Later on, one may check
whether the calibration procedure was invoked or not by the `wasCalibrated()` method and access the minimal
and maximal distance of the ball from the sensor by executing `returnMinDistance()` and `returnMaxDistance()`.

### Input
The position of the ball is read by

`y=FloatShield.sensorRead();`

returning the scaled distance inside the tube as a floating point value from within the range of 0–100 (%). Alternatively, `sensorReadDistance()` reports the distance between the sensor and the ball in millimeters, while `sensorReadAltitude()` gives its altitude relative to ground.

### Output
By supplying the input power `u` in the range of 0–100 (%) to the

`FloatShield.actuatorWrite(u);`

method, the user can set the power sent to the fan through the power circuitry. This method checks for constraints to
avoid overflow, maps the input to 8-bit PWM integers, then sends it to the D3 pin of the Arduino.

Finally, user reference from the potentiometer is acquired by calling

`r=FloatShield.referenceRead();`

returning the position of the potentiometer runner as a floating point scaled to 0–100 (%).

## <a name="matlab"/>MATLAB API
If you cannot program in C/C++ just yet, you may want to try out the MATLAB API for the FloatShield that enables to access the hardware through the [MATLAB](https://www.mathworks.com/downloads/) command line and scripts. It utilizes the [The MATLAB Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/47522-matlab-support-package-for-arduino-hardware) which enables communication between the Arduino prototyping platform and the development computer.

To prevent confusion between the C/C++ and the MATLAB API, the two interfaces are as similar as possible. The MATLAB API is written in object-oriented script and the user must first create an instance from the class:

`FloatShield=FloatShield;`

Using MATLAB with Arduino does not compile m-script to hardware, thus invoking

`HeatShield.begin();`

will simply load a server code to the microcontroller, unless it is not already present. This means that the closed-loop
control by the API in MATLAB is not quasi real-time, since commands are transferred through the serial link between the board and the computer and may be affected by transfer speed or operating system behavior. However, being able to use the high-level MATLAB script allows you to run live experiments under this already familiar software platform and, most importantly, to create and test more advanced control frameworks with minimal
effort.

The operation of the MATLAB API is otherwise completely identical to the aforementioned Arduino version. Therefore methods such as `calibrate()`, `sensorRead()`, `actuatorWrite()` and `referenceRead()` are all implemented for the FloatShield in MATLAB.

## <a name="simulink"/>Simulink API
An even more intuitive to create control loops and perform live experiments is the Simulink API for the HeatShield. It utilizes the [Simulink Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware) which supplies algorithmic units in blocks that access the hardware functionality.

The collection of the algorithmic blocks will be then available for use through the [Simulink Library](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/FloatLibrary.slx) (see figure below) and may be combined with all the other available blocks to create feedback control applications. The Simulink API retains the naming convention and features for individual input and output blocks ('Actuator Write', 'Sensor Read' and 'Reference Read') and a comprehensive block representing the entire device is also available ('FloatShield').

![float_blocks](https://user-images.githubusercontent.com/18485913/71268008-3c0a0e00-234c-11ea-9bea-8acd2670d89a.png)

In direct contrast with the way MATLAB handles Arduinos, the block scheme in [Simulink](https://www.mathworks.com/downloads/) is transcribed into C/C++, then compiled to machine code and uploaded to the microcontroller unit (MCU). In other words, code is run directly on the microcontroller. Simulink not only transcribes the block schemes for hardware, it also maintains the connection between the development computer and MCU. This way controllers can be fine-tuned in a live session, or data may be displayed and logged conveniently.

# <a name="examples"/>Examples

## <a name="control"/>Feedback control
For a start you may want to experiment with a closed-loop control of the levitating ball's position by the proportional–integral–derivative controller (PID) algorithm.

The implementation of PID control in C/C++ is demonstrated by a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/FloatShield/FloatShield_PID/FloatShield_PID.ino), which makes use of the interrupt-driven sampling subsystem of the AutomationShield library, and also its built-in input-saturated absolute-form PID method with integral windup handling by clamping. The progress of the experiments can be followed in real time through the Serial Plotter of the Arduino IDE or logged in MATLAB. Note that reference tracking is fairly accurate for the complex dynamics, while comparing inputs to the outputs exposes the nonlinearity of the system.

![float_pid](https://user-images.githubusercontent.com/18485913/71278045-cce5e700-2356-11ea-9525-90e0965ba885.png)

The same experiment can be conveniently launched from the MATLAB API as well, see the [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/FloatShield/FloatShield_PID.m). Sampling and computing the control decision is performed by the PC, while the Arduino only acts as an I/O interface. The response shown in the figure below demonstrates that a consistent closed-loop behavior is expected even when resorting to MATLAB script.

![float_pid2](https://user-images.githubusercontent.com/18485913/71279257-0d466480-2359-11ea-815a-5cce8adc5786.png)

The same feedback control loop can be built even easier using the Simulink API. Shown below is the full [block scheme](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/examples/FloatShield/FloatShield_PID.slx) for discrete saturated PID control of the process. You need only to select the 'FloatShield' block from the API library to implement the input/output of the hardware. Other blocks, such as the 'Discrete PID Controller', can be readily selected from the Simulink's default library.

![float_scheme](https://user-images.githubusercontent.com/18485913/71279700-b8efb480-2359-11ea-9d8b-cdec066660e6.png)

After selecting the External mode the block scheme is transcribed into C/C++ code, which is then compiled to AVR-specific machine code and downloaded to the MCU. The application runs stand-alone on the MCU while providing basic interaction with the host PC. You may use switches, sliders and knobs to select reference levels and inspect the response live using a 'Scope'.

![float_scope](https://user-images.githubusercontent.com/18485913/71280128-ab86fa00-235a-11ea-8f4a-45a9198b9a3a.png)

Note that the provided MATLAB and Simulink APIs enable to exploit full high-level mathematic power of MATLAB in order to create and test more complex estimation and control algorithms. 

## <a name="ident"/>System identification
Input-output experiments for data gathering can be easily launched, displayed and logged using the Arduino IDE. For example, one [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/FloatShield/FloatShield_Identification/FloatShield_Identification.ino) initializes the sampling and PID control subsystems from the AutomationShield library and allows user to select whether to use PRBS (PseudoRandom Binary Sequence) or APRBS (Amplitude-modulated PRBS) signal for making small changes in input value. The example stabilizes the ball at selected position using PID control and then uses selected signal to induce small changes in the stabilized input, with the goal of monitoring system's response while avoiding saturated positions of the ball.

![float_io](https://user-images.githubusercontent.com/18485913/71281001-1cc7ac80-235d-11ea-984a-b82790777312.png)

After you gather enough data sufficient for system identification, you may try to fit a model to the experimental response. The MATLAB API for the proposed device contains a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/FloatShield/FloatShield_Ident_Greybox.m) for grey-box system identification. The example offers various first-principle model formulations—a nonlinear state-space model (based on an [ordinary differential equation (ODE)](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/FloatShield/FloatShield_ODE.m)), a linear(ized) state-space model and a transfer function.<br/>Once the model is specified, it takes the experimental data and searches for the unknown parameters of the specified model. First, an initial guess of the system model is created based on assumed parameter values and model structure. The unknown parameters are then found by a grey-box estimation procedure using an appropriate search method implemented in MATLAB's [System Identification Toolbox](https://www.mathworks.com/products/sysid.html). The resulting models provide a very good match to the measured input-output data (from 77% to 85%) as indicated in the figure below.

![float_ident](https://user-images.githubusercontent.com/18485913/71270338-9eb1d880-2351-11ea-8645-bdbc60a11197.png)

More details on the identification procedure can be found [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Takacs2020a.pdf).

# <a name="hardware"/>Detailed hardware description
The BlowShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design
The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics of the FloatShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_Circuit.zip).

![float_pcb](https://user-images.githubusercontent.com/18485913/71244839-b4081200-2313-11ea-8cb3-41aff3b5493e.png)

<span style="color:red">The fan and the TOF sensor are only represented by their connectors, while we assume that 12 V external power is drawn through the VIN pin of the Arduino (a).

The fan is powered by an N-channel MOSFET **C1** (l), and driven by the D3 PWM capable microcontroller pin through an 1 kΩ current limiting resistor **R1** (m). Floating states are handled by a 10 kΩ pull-down resistor **R2** (n), while a diode **D1** (o) ensures back electromotive-force (EMF) protection. A connector (p) finally leads to the fan terminals. Since the fan requires 12 V and more current than the USB-powered Arduino can handle, a separate wall adapter power supply is required to operate the device.

The TOF sensor is integrated to a convenient open-source breakout board, thus it can be effortlessly connected (q) to the I2C bus of the Arduino (SDA,SCL).

Finally, the potentiometer **POT1** (r) runner is attached to the A0 ADC capable pin of the board.</span>

## <a name="parts"/>Parts
To make a BlowShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Part    	| Name               	| Type/Value/Note                                                     	| PCS 	|
|-----------	|--------------------	|-----------------------------------------------------------------	|-------|
| (b)       	| PCB                	| FR4, 2 layer, 1.6mm thick                                      	| 1    	|
| (c)       	| [Pump](https://www.gme.sk/vakuove-mikro-cerpadlo-20kpa#product-detail)                	| 3 V       	| 1    	|
| (d)       	| [Pressure Sensor](https://www.gme.sk/i2c-senzor-tlaku-a-teploty-bmp280-3-3v)         	|                 	| 1    	|
| (e)       	| [N-Mosfet](https://www.tme.eu/sk/details/irf3710pbf/tranzistory-s-kanalom-n-tht/infineon-irf/)               	| 	        | 1 	|
| (f)       	| [Capacitor](https://www.tme.eu/sk/details/t491a106m016at/tantalove-kondenzatory-smd/kemet/)               	|           | 1    	|
| (g)       	| [Diode](https://www.tme.eu/sk/details/rs1d-e3_61t/univerzalne-diody-smd/vishay/)        	|                | 1    	|
| (h)       	| [Resistor](https://www.tme.eu/sk/en/details/0805s8j0103t5e/0805-smd-resistors/royal-ohm/)      	|   	| 1    	|
| (i)       	| [Resistor](https://www.tme.eu/sk/en/details/0805s8j0151t5e/0805-smd-resistors/royal-ohm/)             	|   	| 1    	|
| (j)       	| [Potentiometer](https://www.tme.eu/sk/details/ca9ma5-b/gombiky-pre-montazne-potenciometre/acp/ca9ma-9005-black/)               	|    	| 1    	|
| (k)       	| [Potentiometer](https://www.tme.eu/sk/en/details/ca9mv-10k/single-turn-tht-trimmers/acp/ca9mv-10k/)       	| 10kΩ                     	| 1    	|

Note that the total cost of the above components and thus of the entire FloatShield is no more than $30 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_PCB.zip) and [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Blow/BlowShield_Circuit.zip), respectively, <span style="color:red">while the ready-to-manufacture Gerber files with the NC drilling instructions are available from</span> [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Float/FloatShield_Gerber.zip).

The assembled shield is fixed mechanically and electrically to the Arduino board through stackable header pins. The location of the components is also illustrated in the PCB layout below. <span style="color:red">The axial fan is mounted to the board on standoffs and a hole cut at the manufacturing stage of the PCB increases the air intake capacity.</span>

<img width="500" alt="pcbfront" src="https://user-images.githubusercontent.com/37963774/39986438-ef45542e-5761-11e8-8869-8ff14c35a95a.png">
<img width="500" alt="pcbback" src="https://user-images.githubusercontent.com/37963774/39986439-ef812314-5761-11e8-88a9-eab1ce675225.png">

# <a name="about"/>About
This shield was designed and created as a term project at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2019/2020.

## <a name="authors"/>Authors
* 3D-model design: Marcel Vdoleček
* Hardware design: Peter Šálka, Miloš Podbielančík, Dávid Šroba, Martin Lučan
* Software design: Gábor Penzinger, Jakub Kulhánek
* Wiki documentation: Martin Gulan, Gergely Takács