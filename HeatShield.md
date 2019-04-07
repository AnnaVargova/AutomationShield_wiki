#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
&nbsp;&nbsp;&nbsp;[MATLAB API](#matlab)<br/>
&nbsp;&nbsp;&nbsp;[Simulink API](#simulink)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[System identification](#ident)<br/>
&nbsp;&nbsp;&nbsp;[Control](#control)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#io)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#io)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#io)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction

The HeatShield belongs to the family of control engineering education devices for Arduino that form a part of the AutomationShield project. This particular low-cost shield demonstrates the thermal control of a 3D printer heating block implementing a resistive heating cartridge as the actuator and a negative temperature coefficient (NTC) resistor as the sensor, which creates a simple single-input single-output (SISO) feedback loop. In place of the usual extrusion nozzle supplying the melted plastic filament is a steel screw connecting the heating block to a thermal insulator that prevents heat damage to the printed circuit board (PCB). The maximal temperature of the heating block is limited at ~80°C by an adjustable linear voltage regulator. The HeatShield also features an optional transparent safety enclosure.

<!-- [Heat](https://user-images.githubusercontent.com/18485913/55647718-ae4aba80-57de-11e9-9ba4-b93ec63d62b5.png)-->
![Heat2](https://user-images.githubusercontent.com/18485913/55653082-afcfaf00-57ed-11e9-9187-0f1797fd71b2.png)

For a better visualization the entire assembly was 3D-modeled using the CAD software CATIA V5R20 (Student Edition) and can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1939152/HeatShieldAssembly.zip). Note that it features the model of Arduino Uno available from [here](https://grabcad.com/library/arduino-uno-r3-shield-in-description-1).

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the HeatShield is included in the `HeatShield.h` header, which contains the `HeatShieldClass` class that is constructed by default as the `HeatShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output (sensor): `HeatShield.sensorRead();` 
* Input  (actuator): `HeatShield.actuatorWrite();` 

![Heat_Functions](https://user-images.githubusercontent.com/18485913/55668007-c4e72500-5863-11e9-99f6-b117947494d9.png)

The following subsections describe the methods used to access the input and output of the HeatShield.
Note that before you begin an experiment you must initialize the hardware by calling

`HeatShield.begin();`

which determines the mode of the input and output pins.

### Input

Assuming the power supplied to the heating cartridge is stored in the variable `u` as a floating point number in the range of 0-100 (%), the actuator is supplied by the input signal after calling the method

`HeatShield.actuatorWrite(u);`

which will convert the percentage value to an 8-bit number driving the pulse-width modulation (PWM) output of the microcontroller.

### Output

The thermistor in the heating block is accessed by calling the

`y = HeatShield.sensorRead();`

method, which returns the block temperature in degrees Celsius to the variable `y` as a floating point number.
This function first calls the `getThermistorVoltage()` method, which returns the output potential at the voltage divider. Based on the known input reference voltage
<img src="http://latex.codecogs.com/gif.latex?V_{\mathrm{r}}" border="0"/>
, the known reference resistance
<img src="http://latex.codecogs.com/gif.latex?R_{\mathrm{r}}" border="0"/>
and the the output voltage
<img src="http://latex.codecogs.com/gif.latex?V_{\mathrm{o}}" border="0"/>
one may use Kirchhoff's current law to compute the unknown resistance
<img src="http://latex.codecogs.com/gif.latex?R" border="0"/>
according to

<img src="http://latex.codecogs.com/gif.latex?R=\frac{V_{\mathrm{o}}R_{\mathrm{r}}}{V_{\mathrm{r}}-V_{\mathrm{o}}};" border="0"/>

which is implemented in the device's API as the `getThermistorResistance()` method. Though searching for the unknown temperature is more precise according to tables associating the nonlinear relationship of resistance and temperature, a simplified form of the Steinhart-Hart equation is used. Given a known reference temperature
<img src="http://latex.codecogs.com/gif.latex?T_{0}\,(\mathrm{K})" border="0"/>
and corresponding nominal resistance
<img src="http://latex.codecogs.com/gif.latex?R_{0}\,(\omega)" border="0"/>
; and the properties of the thermocouple given by the parameter
<img src="http://latex.codecogs.com/gif.latex?\beta" border="0"/>
one may compute the unknown temperature
<img src="http://latex.codecogs.com/gif.latex?T\,(\mathrm{K})" border="0"/>
by

<img src="http://latex.codecogs.com/gif.latex?\frac{1}{T}=\frac{1}{T_0}+\frac{1}{\beta} \ln\left(\frac{R}{R_0} \right)," border="0"/>

which is what `sensorRead()` essentially does.

## <a name="matlab"/>MATLAB API

If you cannot program in C/C++ just yet, you may want to try out the MATLAB API for the HeatShield that enables to access the hardware through the [MATLAB](https://www.mathworks.com/downloads/) command line and scripts. It utilizes the [The MATLAB Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/47522-matlab-support-package-for-arduino-hardware) which enables communication between the Arduino prototyping platform and the development computer. Various commands accessing the hardware are executed directly in quasi real time without the need to compile code. This means that code is not deployed to the processor, and the Arduino merely acts as an external laboratory measurement card.

To prevent confusion between the C/C++ and the MATLAB API, the two interfaces are as similar as possible. The MATLAB API is written in object-oriented script and the user must first create an instance from the class:

`HeatShield=HeatShield;`

The shield is then initialized by calling

`HeatShield.begin();`

which, unless already done, uploads the server code to the prototyping board.

The temperature reading procedure is identical to that of C/C++ API. The voltage from the `getThermistorVoltage()` method is passed to the `getThermistorResistance()` and this will calculate the resistance of the thermistor based on the voltage divider. The sensor is then directly accessed by calling the

`y = HeatShield.sensorRead();`

method, where the variable `y` will hold the temperature in degrees Celsius. Finally, the heating cartridge power is set through

`HeatShield.actuatorWrite(u);`

which accepts the input power `u` in percents.

Note that the use of the high-level commands of MATLAB allows for a simple implementation of more advanced control engineering concepts that would be complex and time consuming to implement in C/C++.

## <a name="simulink"/>Simulink API

An even more intuitive to create control loops and perform live experiments is the Simulink API for the HeatShield. It utilizes the [Simulink Support Package for Arduino Hardware ](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware) which supplies algorithmic units in blocks that access the hardware functionality. In direct contrast with the way MATLAB handles Arduinos, the block scheme in [Simulink](https://www.mathworks.com/downloads/) is transcribed into C/C++, then compiled to machine code and uploaded to the microcontroller unit (MCU). In other words, code is run directly on the microcontroller. Simulink not only transcribes the block schemes for hardware, it also maintains the connection between the development computer and MCU. This way controllers can be fine-tuned in a live session, or data may be displayed and logged conveniently.

The Simulink API offers the following algorithmic blocks:
![HeatShield_Simulink_API](https://user-images.githubusercontent.com/18485913/55669618-e81ccf00-5879-11e9-9480-19b2cc51143f.png)

The 'Actuator Write' block accepts real numbers from 0-100\% and supplies power to the cartridge.

The 'Sensor Read' block reads the input from the thermistor. Users may choose between raw ADC levels, voltage, resistance and temperature. The parameters of the voltage divider and the thermistor can be also fine-tuned.

The 'HeatShield' block unites the input and output functionality into a single entity that can be conveniently used for identification and control experiments.

The remaining two blocks mathematically represented the dynamic process of heating the printer head by a black-box nonlinear ARX model and a first-order ordinary differential equation. The blocks can be used to simulate the response of HeatShield and thereby tuning controllers without the need to run the experiments on the live system.

# <a name="examples"/>Examples

## <a name="ident"/>System identification

Input-output experiments for data gathering can be launched, displayed and logged in C/C++ (Arduino IDE), MATLAB and Simulink as well. For example, one [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/HeatShield/HeatShield_Identification/HeatShield_Identification.ino) sends a series of step changes that can be logged in various ways through the serial line, while [another example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/HeatShield/HeatShield_Random/HeatShield_Random.ino) also adds a random component to the inputs to collect more information-rich signals. Similarly, a [worked MATLAB example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/HeatShield/HeatShield_Identification_Experiment.m) and a [worked Simulink example](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/examples/HeatShield/HeatShield_InputsOutputs.slx) perform a simple step change of input, displays the progress of the response live on screen and saves a data file.

![IDArduino](https://user-images.githubusercontent.com/18485913/55674551-7747d800-58b6-11e9-80f2-a8496d21bf76.png)

After you gather enough data sufficient for system identification, you may try to fit a model to the experimental response. By inspecting the response it is clear that the dynamics is of the first order, and one may assume that the identification procedure is straightforward. Nevertheless, the simple dynamics of the HeatShield holds surprises and may be used to illustrate several abstract principles by a real-life example.

The MATLAB API for the the proposed device contains a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/HeatShield/HeatShield_Identification_Blackbox.m) for black-box system identification using the MATLAB's [System Identification Toolbox](https://www.mathworks.com/products/sysid.html). This creates a simple but inadequate process model (∼20% fit) and a more complex and valid nonlinear ARX model (∼90% fit). An even more sophisticated assignment is to develop a first-principle model, then fitting the data to this using grey-box methods. A [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/HeatShield/HeatShield_Identification_Greybox.m) takes experimental data and searches for the unknown parameters of the [first-principle model](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/HeatShield/HeatShield_ODE.m) described by an ordinary differential equation (ODE). First, an initial guess of the model is created based on the assumed parameter values and model structure. The parameters are then found by a nonlinear grey box estimation procedure using an adaptive Gauss-Newton search method. The resulting model is an excellent match to the measured input-output data (∼92%). This model is also implemented in the Simulink API.

![SimulatedResponse](https://user-images.githubusercontent.com/18485913/55674702-64360780-58b8-11e9-8e41-359adf661315.png)

More details on the identification procedure can be found [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Takacs2019b.pdf).

## <a name="control"/>Control

For a start you may want to experiment with a closed-loop temperature control by the proportional–integral–derivative controller (PID) algorithm.

The implementation of PID control in C/C++ is demonstrated by a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/HeatShield/HeatShield_PID/HeatShield_PID.ino), which makes use of the interrupt-driven sampling subsystem of the AutomationShield library, and also its built-in input-saturated absolute-form PID methods with integral windup handling by clamping. The progress of the experiments can be followed in real time through the Serial Plotter of the Arduino IDE or logged in MATLAB. The results of the PID controlled temperature response of the printer head are shown below. The same experiment can be conveniently launched from the MATLAB API as well, see the [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/HeatShield/HeatShield_PID.m).

![HeatShield_PID_C](https://user-images.githubusercontent.com/18485913/55687584-dc5f0480-596e-11e9-815c-bf1d8eabe6bf.png)

The same feedback control loop can be built even easier using the Simulink API. Shown below is the full [block scheme](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/examples/HeatShield/HeatShield_PID_Simulate.slx) for discrete saturated PID control of the process. You need only to select the 'HeatShield' block from the API library to implement the input/output of the hardware. Other blocks, such as the 'Discrete PID Controller', can be readily selected from the Simulink's default library.

![SimulinkPID](https://user-images.githubusercontent.com/18485913/55687600-23e59080-596f-11e9-9b0f-59e6b01eea39.png)

After selecting the External running mode the block scheme is re-interpreted to C/C++ code, which is then compiled to AVR-specific machine code and downloaded to the MCU. Communication is possible between the block scheme and the hardware. You may use switches, sliders and knobs to select reference levels and inspect the response live using a Scope.

![ScopeShot](https://user-images.githubusercontent.com/18485913/55687610-3e1f6e80-596f-11e9-8fbf-2fc43436dc15.png)

Note that the MATLAB and Simulink APIs enable to exploit full high-level mathematic might of MATLAB in order to create and test more complex control algorithms. 

# <a name="hardware"/>Detailed hardware description

The HeatShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

For those who wish to use the board without the library, the components are connected to the following pins:

![Heat_PIN](https://user-images.githubusercontent.com/18485913/55667701-20afaf00-5860-11e9-8070-3cb1de7f9597.png)

## <a name="circuit"/>Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the HeatShield from [here](https://github.com/richardsalini/HeatShield/files/1968266/HeatShield_Circuit.zip).

![HeatScheme](https://user-images.githubusercontent.com/18485913/55666313-644ced80-584d-11e9-86c1-06162b22571e.png)

The power circuitry is driven by an N-channel metal-oxide semiconductor field-effect transistor (MOSFET) **Q1** connected to the PWM capable D3 pin of the Arduino. A series resistor **R5** protects the MCU **U1** in transients, while a resistor **R6** parallel to ground ensures that floating electrical states do not cause the heater to turn off accidentally.

Normal temperatures used to melt plastics in 3D printing are over 200°C and may reach as high as 320°C for polycarbonate filaments. To make the experiment safer for general classroom use, the maximal temperature of the heating block is limited at ~80°C by an adjustable linear voltage regulator **U2**, which takes the external 12V input from the Vin pin of the MCU and reduces it to a voltage configured by resistors **R3** and **R4** supplying its adjustable input.

Temperature feedback is based on a negative temperature coefficient (NTC) thermistor **R1** connected to the A0 analog input pin of the MCU in a voltage divider circuit paired with a resistor **R2**. 

The exact component specification and required quantities are given next. Note that only components with through-hole technology (THT) mounting are utilized in order to make assembly and servicing easy even if you are inexperienced with electronics.

## <a name="parts"/>Parts
To make an HeatShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|Part   | Name              | Type/Value/Note | PCS |
|-------|-------------------|-----------------|-----|
| Q1    | [Transistor](https://www.tme.eu/sk/details/buz11/tranzistory-s-kanalom-n-tht/on-semiconductor-fairchild/buz11-nr4941/) | BUZ11, N-channel power MOSFET, 50V, TO-220, THT | 1 |
| U2    | [Voltage regulator](https://www.tme.eu/sk/details/lm317t/stabilizatory-napatia-regulovane/st-microelectronics/) | LM317T, adjustable positive linear voltage regulator, 1.2-37V, TO-220, THT   | 1 |
| R1    | [Thermistor](https://www.na3d.sk/p/2482/termistor-pre-3d-tlaciaren-1-m-kabel) | NTC 3950, 100kΩ, rated 300°C with wiring | 1 |
| R2    | Resistor          | 100kΩ, 1/4W ,THT  | 1 |
| R3,R5 | Resistor          | 1kΩ, 1/4W, THT    | 2 |
| R4    | Resistor          | 240kΩ, 1/4W, THT  | 1 |
| -     | Heat sink         | for the voltage regulator, TO-220 package | 1 |
| -     | [Heating cartridge](https://www.na3d.sk/p/2634/vyhrevne-teleso-24v-30w) | 24V, 30W, 20x6mm     | 1 |
| -     | [Heating block](https://www.na3d.sk/p/2638/e3d-v6-hlinikova-kocka)      | aluminum, 20x16x12mm | 1 |
| -     | [Thermal insulator](http://www.conrad.sk/izolator-sestiuhelnik-m6-is20hh625-20-mm-25-mm.k887493)| hexagonal glass-filled polyester insulator, M6 IS20HH625, 25x20mm | 1 |
| -     | PCB               | FR4, 2-layer, 1.6mm thick | 1 |
| -     | Header            | 6x1, female, 2.54mm pitch | 1 |
| -     | Header            | 8x1, female, 2.54mm pitch | 3 |
| -     | Bolt              | M6x20mm, headless, block to insulator | 1 |
| -     | Bolt              | M6x10mm, rounded 3.3mm flat head, insulator to PCB | 1 |

Note that the total cost of the above components and thus of the entire HeatShield is no more than $5 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100x100mm limit of most board manufacturers. The DIPTrace PCB layout of the HeatShield can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1968264/HeatShield_PCB.zip), while the ready-to-manufacture Gerber files are available from [here](https://github.com/richardsalini/HeatShield/files/1968257/HeatShield_Gerber.zip).

![pcb](https://user-images.githubusercontent.com/38358320/39538111-fd3f2502-4e3b-11e8-8d28-1c011d404a38.png)
![pcb2](https://user-images.githubusercontent.com/38358320/39538175-3adf2240-4e3c-11e8-878c-773351e0a618.png)

# <a name="about"/>About
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

## <a name="authors"/>Authors
* **Hardware design:** Juraj Bavlna, Michal Kováč, Richard Köplinger, Sohaibullah Zarghoon, Richard Salíni
* **Software design:** Richard Köplinger
* **3D model design:** Michal Kováč
* **Wiki:** Martin Gulan, Gergely Takács