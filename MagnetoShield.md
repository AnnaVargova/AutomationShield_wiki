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

#### Legacy documentation
[Release 1: (MagnetoShield R1)](https://github.com/gergelytakacs/AutomationShield/wiki/MagnetoShield/993435cb0fb3cee840e43e1a2bc32eabbdb5bf99)

# <a name="intro"/>Introduction
The MagnetoShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project and presents a low-cost miniature magnetic levitation experiment. The device uses a solenoid electromagnet to generate a magnetic force which lifts up the permanent magnet. The goal is to control the position of the levitating magnet indirectly measured by a Hall effect sensor, which creates a simple single-input single-output (SISO) feedback loop. Due to its complexity and fast dynamics, MagnetoShield is great tool for learning and implementation of feedback control algorithms. The hardware is low-cost and small, making it ideal for take-home experiments or long-term student projects.

![MagnetoShield](https://user-images.githubusercontent.com/18485913/71313088-2112c900-2434-11ea-90dc-818339cc13a1.png)

# <a name="api"/>Application programming interface
The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the MagnetoShield is included in the `MagnetoShield.h` header, which contains the `MagnetoClass` class that is constructed by default as the `MagnetoShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output (sensor): `MagnetoShield.sensorRead();`
* Output (auxiliary sensor): `MagnetoShield.auxReadCurrent();`
* Input (actuator): `MagnetoShield.actuatorWrite();`

![magneto_actsens](https://user-images.githubusercontent.com/18485913/71313099-60d9b080-2434-11ea-8a43-2326924940c9.png)

## <a name="init"/>Initialization and calibration
The following subsections describe the methods used to access the input and output of the MagnetoShield. Note that before you begin an experiment you must initialize the hardware by calling the

`MagnetoShield.begin();`

method, which sets the analog reference to external. This way the 3.3V rail connected to the reference pin acts as the
maximum for the 10-bit built-in ADC ensuring compatibility with non-AVR boards. In addition to this, the method calls
the Wire library for the I<sup>2</sup>C bus functionality required for the DAC chip.

It is recommended to start the applications by selfcalibrating the device, by calling the

`MagnetoShield.calibration();`

method, which measures the output of the Hall effect sensor with the magnet off and on full power. The calibrated ADC
levels can be accessed by the `getMinCalibrated()` and `getMaxCalibrated()` methods. Direct distance measurement is not available on this device and it is not even needed for simple feedback control, but first-principle models require
a distance signal. The distance of the permanent magnet from the solenoid is known at the two extremes; e.g. when it rests
on the surface of the PCB directly over the sensor and when it hits the roof of the enclosure. Let us assume that there is a power relationship between the magnetic flux
<img src="http://latex.codecogs.com/gif.latex?B(k)" border="0"/>
measured on the Hall sensor and the distance
<img src="http://latex.codecogs.com/gif.latex?h(k)" border="0"/>
at time
<img src="http://latex.codecogs.com/gif.latex?t=kT_\mathrm{s}" border="0"/>
, so
that

<img src="http://latex.codecogs.com/gif.latex?h(k) = p_1B(k)^{p_2}." border="0"/>

The calibration routine thus performs a two-point calibration searching for constants
<img src="http://latex.codecogs.com/gif.latex?p_1" border="0"/>
and
<img src="http://latex.codecogs.com/gif.latex?p_2" border="0"/>
based on this idea. In case you don't run the calibration routine, default values are given as well.

## <a name="input"/>Input
For general feedback control experiments, it does not actually matter what units we used to power the solenoid: it may be percentage of full power, voltage, current, etc. However, if we wish to use first-principle models or model-based control, the input to the solenoid shall be

`MagnetoShield.actuatorWrite(u);`

which applies the input voltage
<img src="http://latex.codecogs.com/gif.latex?u(k)" border="0"/>
ranging from 0 to 12V at the given sample.<br/>The method uses the results of an offline multi-point calibration procedure described in [this paper](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Takacs2020b.pdf). Alternatively, input to the solenoid can be also sent by employing the `actuatorWritePercents()` method.

## <a name="output"/>Output
The distance of the permanent magnet from the solenoid can be determined by calling

`y=MagnetoShield.sensorRead()`

method that returns the distance
<img src="http://latex.codecogs.com/gif.latex?y(k)=h(k)" border="0"/>
in millimeters as a floating point number. The method reads the ADC levels from the Hall sensor and calls `adcToGauss()`, converting the levels to voltage related to the 3.3V reference, removing the 2.5V bias for zero magnetic flux and multiplying by the manufacturer given sensitivity of 1.3mV/G. The magnetic flux is then recalculated by the above-mentioned two-point calibration implemented in the `gaussToDistance()` method. Sensor readings can be acquired in percents, by employing the `sensorReadPercents()` methods as well.

## <a name="aux"/>Auxiliary functions
In addition, there are some non-essential auxiliary functions provided in the device API. First of these is the method reading the current sensor that is useful when one considers a first-principle model of the process. The current sensor is accessed by

`i=MagnetoShield.auxReadCurrent();`

which returns the current reading
<img src="http://latex.codecogs.com/gif.latex?y(k)=i(k)" border="0"/>
in milliamperes. The method polls for ADC levels, which are converted to voltage and this in turn to current by a proportional gain defined by the current sensor chip.

The external reference potentiometer is read by

`r=MagnetoShield.referenceRead();`

which returns a floating point number in the range of 0???100 (%).<br/>Finally, calling

`vin=MagnetoShield.auxReadVoltage();`

will return the supply voltage of the 12V rail, being mainly useful for self-diagnostics.

# <a name="examples"/>Examples

## <a name="control"/>Feedback control
For a start you may want to experiment with a closed-loop control of the levitating ball's position by the proportional???integral???derivative controller (PID) algorithm.

The implementation of PID control in C/C++ is demonstrated by a [worked example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/MagnetoShield/MagnetoShield_PID/MagnetoShield_PID.ino), which makes use of the interrupt-driven sampling subsystem of the AutomationShield library, and also its built-in input-saturated absolute-form PID method with integral windup handling by clamping. You may select wheter the reference is given by the potentiometer or you want to test a predetermined reference trajectory. The progress of the experiments can be followed in real time through the Serial Plotter of the Arduino IDE or logged in MATLAB.

![magneto_pid](https://user-images.githubusercontent.com/18485913/71310428-5b6c6e00-2414-11ea-8a73-8d1cd230f326.png)

The above figure shows a typical and robustly repeatable closed-loop experiment. At the beginning of the experiment, the permanent magnet laying at rest is quickly pulled up close to the level of the solenoid, then settles on the first reference level r=14mm with some overshoot (upper graph). Other reference levels are followed closely. The levitation including the reference changes are obvious and visually engaging. The lower graph shows the corresponding input signal. The nonlinear nature of its closed-loop dynamics can be hypothetically further improved by better???possibly natively nonlinear???control methods.

## <a name="ident"/>System identification

Input-output experiments for data gathering can be easily launched, displayed and logged using the Arduino IDE. The system is open-loop unstable, therefore experimental data have to be obtained in a closed loop using a feedback controller. A [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/MagnetoShield/MagnetoShield_Identification/MagnetoShield_Identification.ino) initializes the sampling and PID control subsystems from the AutomationShield library. A pre-tuned PID controller is used to follow a reference output sequence, each setpoint for a thousand samples. In order to create a rich probe signal, the inputs computed by the controller are superimposed by a 1.5V pseudorandom signal generated in the device. The resulting input and corresponding output are logged.

After you gather enough data sufficient for system identification, you may try to fit a model to the experimental response. A comprehensive background on mathematical modeling of the magnetic levitation experiment based on mechanical and electrical first principles can be found in [our paper](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Takacs2020b.pdf).<br/>A [worked MATLAB example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/MagnetoShield/MagnetoShield_ID_GreyBox_TF.m) takes the experimental data and searches for the unknown parameters of the system's transfer function. First, an initial guess of the model is built based on laboratory measurements. This initial model is then fitted to the experimental data using the MATLAB???s [System Identification Toolbox](https://www.mathworks.com/products/sysid.html). After several iterations the identification procedure converges reliably to a solution. As shown below, the resulting transfer function provides a decent 83.3% match to the measured input-output data.

![magneto_ident](https://user-images.githubusercontent.com/18485913/71310643-aa1b0780-2416-11ea-981a-23a7d3a929e4.png)

Accuracy of the identified model can be verified in a closed-loop simulation of the model response in the time domain. As it is evident, the linear model over-values the random noise in the probe signal, which in reality is much less stated.

![magneto_ident2](https://user-images.githubusercontent.com/18485913/71310644-ab4c3480-2416-11ea-870e-d2198fb51ffd.png)

Another [worked MATLAB example](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/MagnetoShield/MagnetoShield_ID_GreyBox_SS.m) in turn assumes a linearized state-space model parametrized with five
unknown parameters, initialized same as in the previous example. The only difference here is, that the current sensor is used to gather dynamic current measurements at each sample, as current represents one of three dynamic states of the system.<br/>The following figure shows the comparison of the model outputs with experimental measurements in the frequency-domain. The model fit to measurement is 82% for the position measurement and 87% for the current signal, suggesting an adequate model that is suited for simulation, estimator and observer design, and model-based control.

![magneto_freq](https://user-images.githubusercontent.com/18485913/71310468-cddd4e00-2414-11ea-882f-96d9a90e1c4d.png)

More details on the identification procedure can be found [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Takacs2020b.pdf).

# <a name="hardware"/>Detailed hardware description
The MagnetoShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

The schematic representation of the MagnetoShield is shown in the figure below. The position of a disc-shaped levitating permanent magnet (a) is sensed indirectly by a Hall effect sensor (b). The analog voltage signal from the sensor is passed through an analog-to-digital converter (ADC) (c) to the microcontroller (d). Inputs computed in the microcontroller are turned to an analog voltage signal through a digital-to-analog converter (DAC) (e) then passed to an amplification circuit (f) to the solenoid (g). A current sensor (h) is employed to gain extra insight to dynamic processes, mainly for system identification purposes.


![magneto_scheme](https://user-images.githubusercontent.com/18485913/71310211-b2bd0f00-2411-11ea-9b1a-097a5ad89420.png)

## <a name="circuit"/>Circuit design
The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the MagnetoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_Circuit.zip). 

![magneto_circuit](https://user-images.githubusercontent.com/18485913/71308765-c14efa80-2400-11ea-88d3-fbe9317e9040.png)

Let us start with the input signal path. The microcontroller ((a),**U1**)???for example an atMega 328P in case of an Arduino Uno???is connected to an NXP Semiconductors PCF8591T external 8-bit DAC chip ((b),**U3**) via the I<sup>2</sup>C bus, pulled up to 5V ((c),**R1**,**R2**). Although some actuators, such as motors, can be conveniently supplied by a pulse width modulated (PWM) signal, tests have shown that the fast dynamic response of the levitation process is affected by this type of input significantly. The 12V supply voltage (d) to the electromagnet is pulled from the common VIN pin of the Arduino layout. This means, that the barrel jack connector on the Arduino can be used to power the electromagnet by a wall-plug adapter. The input voltage from the DAC chip is then fed to an IRF520 N-channel MOSFET ((e),**Q2**) in a low-side configuration. The analog out of the pin is protected in transients by a small-value series resistor, while open floating states are prevented with a parallel resistor ((f),**R3**,**R4**). The solenoid ((g),**L1**) goes to the high-side of this configuration, thus when there is gate-source voltage on the MOSFET, the channel starts to conduct and the electromagnet is energized. Transient effects are filtered by a capacitor ((h),**C2**), while the MOSFET and voltage source is protected from back-EMF by a diode ((i),**D1**).

Let us continue with the output path. MagnetoShield utilizes indirect distance sensing by an Allegro MicroSystems A1302KUA linear Hall effect sensor ((j),**U2**). Since the magnetic levitation experiment is designed to be compatible with ARM Cortex M-series boards as well, a 3.3V Zener diode is used to protect analog inputs from overvoltage. The hall sensor is bipolar and is supplied from the 5V rail, therefore an incidental swap of the magnet polarities could result in an overvoltage on these devices. Other sensing circuits use the same type of Zener clippers ((k),**D3**???**D5**) as well. The output of the Hall sensor is connected directly to the Arduino, which contains built-in ADC peripheries.<br/>Although a non-essential functionality, sensing the current passing through the electromagnet may aid mathematical modeling of the system, providing data for estimation and system identification tasks. The electromagnet is powered through a precise shunt resistor with a known resistance ((l),**R8**). This differential input signal is then amplified using a Texas Instruments INA169 high-side measurement current shunt monitor ((m),**U4**) with a gain configured by a precise resistor ((n),**R9**). The output of this amplifier is then fed to the ADC of the Arduino, which is also protected by the aforementioned Zener clamp.

Note that there are also simple auxiliary circuits extending the functionality of the MagnetoShield. A potentiometer ((o),**POT1**) can be programmed to perform a number of roles, most notably it may supply a manually adjusted reference trajectory for position. There is a voltage divider circuit ((p),**R6**,**R7**) that monitors supply voltage, and finally, a LED signals power to the board ((q),**D6**).

## <a name="parts"/>Parts
To make a MagnetoShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:


| Part             | Name            | Type/Value/Note                                                       | PCS |
|------------------|-----------------|-----------------------------------------------------------------------|-----|
| ???   	           | 3D Print        | 5.7g ??1.75mm PETG filament, bright green, at 240&deg;C (90&deg;C bed) | 1   |
| C1,C3            | Capacitor       | 0805, ceramic, 0.1??F                                                  | 2   |
| (h),C2           | Capacitor       | 0805, tantalum, 10??F                                                  | 1   |
| ???   	           | Enclosure top   | clear acrylic; e.g. h=2 mm, stamped to the outer diameter of the tube | 1   |
| (m),U4           | Current sensor  | INA149                                                                | 1   |
| (b),U3           | [DAC](http://sk.farnell.com/nxp/pcf8591t-2-518/adc-single-8bit-11-1ksps-soic/dp/2400442RL?st=PCF8591)             | PCF8591T                                                              | 1   |
| (i),D1           | Diode           | DO214AC                                                               | 1   |
| (j),U2           | [Hall sensor](https://uk.rs-online.com/web/p/hall-effect-sensor-ics/6807119/)     | A1302KUA                                                              | 1   |
| ???   	           | Header          | 6??1, female, 2.54mm pitch                                             | 1   |
| ???   	           | Header          | 8??1, female, 2.54 mm pitch                                            | 2   |
| ???   	           | Header          | 10??1, female, 2.54mm pitch                                            | 1   |
| (q),D2           | LED             | 0805, red                                                             | 1   |
| ???   	           | Magnet          | NdFeB, disc, ??8mm, h=2mm, N38                                         | 1   |
| (e),Q2           | MOSFET          | IRF520                                                                | 1   |
| ???   	           | PCB             | 2 layer, FR4, 1.6mm thick                                             | 1   |
| (o),POT1         | Potentiometer   | 10k???                                                                  | 1   |
| (c),(f),R1,R2,R4 | Resistor        | 10k???, 0805                                                            | 3   |
| (n),(p),R6,R9    | Resistor        | 3k???, 0805, 0.1%                                                       | 2   |
| (p),R7           | Resistor        | 1k???, 0805, 0.1%                                                       | 1   |
| (f),R3           | Resistor        | 220???, 0805                                                            | 1   |
| ???   	           | O-Ring          | rubber, M12, h=1mm, e.g. ??18mm (outer)                                | 1   |
| ???   	           | Screws          | polyamid, M3??8                                                        | 2   |
| ???   	           | Shaft           | ACP CA9MA9005                                                         | 1   |
| (l),R8           | Shunt           | 10???, 0805, 0.1%                                                       | 1   |
| (g),L1           | [Solenoid](https://www.ebay.com/itm/322722704471)        | 220???, 0805                                                            | 1   |
| ???   	           | Enclosure tube  | clear, Plexiglas XT, h=8mm, ??10mm (inner), ??12mm (outer)              | 1   |
| (k),D3???D5        | Zener diode     | 3.3V, SOD323                                                          | 3   |

Note that the total cost of the above components and thus of the entire MagnetoShield is no more than $9 excluding labor and postage.

The assembled MagnetoShield is shown in the figure below. The circuit board contains long stacking headers (I), which connect the device to a compatible Arduino. The magnet is held in place by a 3D printed superstructure (II). The 12V rail for the magnet is powered through the barrel connector of the development board underneath, while the USB programming port of an Arduino Uno is also shown here. The equivalents of the circuit components described above for the schematics are marked on the assembled device with the same letters.

![magneto_assembly](https://user-images.githubusercontent.com/18485913/71309268-4c7ebf00-2406-11ea-8256-d6bbc3e7d2ec.png)

The structure for holding the solenoid was designed in Autodesk Fusion 360 and printed by a Prusa I2 MK3/S 3D printer using PETG filament in 80 minutes time. 

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout and circuit schematics can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_PCB.zip) and [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_Circuit.zip), respectively, while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_Gerber.zip).

![magneto_pcb](https://user-images.githubusercontent.com/18485913/71308119-abd5d280-23f8-11ea-9ded-0aa222dbafd0.png)

# <a name="about"/>About
This shield was designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI) in 2017/2018. The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava. The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Mihalik2018.pdf).

## <a name="authors"/>Authors
* Hardware design: Gergely Tak??cs, Jakub Mihal??k
* Software design: Gergely Tak??cs, Jakub Mihal??k
* Wiki: Martin Gulan, Erik Mikul???? and Gergely Tak??cs