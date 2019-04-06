##### Contents 
[Introduction](#intro)  
[Library functions](#api)  
[MATLAB API](#matlab)
[Simulink API](#simulink)
[Examples](#examples)
[Detailed Hardware Description](#hardware)
[About](#about)


# <a name="intro"/></a>Introduction

The HeatShield belongs to the family of control engineering education devices for Arduino that form a part of the AutomationShield project. This particular low-cost shield demonstrates the thermal control of a 3D printer heating block implementing a resistive heating cartridge as the actuator and a negative temperature coefficient (NTC) resistor as the sensor, which creates a simple single-input single-output (SISO) feedback loop. In place of the usual extrusion nozzle supplying the melted plastic filament is a steel screw connecting the heating block to a thermal insulator that prevents heat damage to the printed circuit board (PCB). The maximal temperature of the heating block is limited at ~80°C by an adjustable linear voltage regulator. The HeatShield also features an optional transparent safety enclosure.

<!-- [Heat](https://user-images.githubusercontent.com/18485913/55647718-ae4aba80-57de-11e9-9ba4-b93ec63d62b5.png)-->
![Heat2](https://user-images.githubusercontent.com/18485913/55653082-afcfaf00-57ed-11e9-9187-0f1797fd71b2.png)

For a better visualization the entire assembly was 3D-modeled using the CAD software CATIA V5R20 (Student Edition) and can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1939152/HeatShieldAssembly.zip). Note that it features the model of Arduino Uno available from [here](https://grabcad.com/library/arduino-uno-r3-shield-in-description-1).




# <a name="api"/></a>Library functions

The basic aplication programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the HeatShield is included in the `HeatShield.h` header, which contains the `HeatShieldClass` class that is constructed by default as the `HeatShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output (sensor): `HeatShield.sensorRead();` 
* Input  (actuator): `HeatShield.actuatorWrite();` 

![Heat_Functions](https://user-images.githubusercontent.com/18485913/55668007-c4e72500-5863-11e9-99f6-b117947494d9.png)

## <a name="io"/>Inputs and outputs

The following sections describe the methods used to access the input and output of the HeatShield.
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

If you cannot program in C/C++ just yet, you may want to try out the MATLAB API for the HeatShield that enables to access the hardware through the [MATLAB](https://www.mathworks.com/downloads/) command line and scripts. It requires the [The MATLAB Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/47522-matlab-support-package-for-arduino-hardware) which enables communication between the Arduino prototyping platform and the development computer. Various commands accessing the hardware are executed directly in quasi real time without the need to compile code. This means that code is not deployed to the processor, and the Arduino merely acts as an external laboratory measurement card.

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

[Simulink Support Package for Arduino Hardware ](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware)

The Simulink API offers the following algorithmic blocks:
![HeatShield_Simulink_API](https://user-images.githubusercontent.com/18485913/55669618-e81ccf00-5879-11e9-9480-19b2cc51143f.png)



# <a name="examples"/>Examples

Example for PID control temperature from 28°C to 40°C.

```
#include "AutomationShield.h"

float r;
float y;
float u=0.0;
float e;

bool enable = false;

void setup(void) {

  Serial.begin(9600);          
  HeatShield.begin(); 
  Sampling.interruptInitialize(1000000); //period in us
  Sampling.setInterruptCallback(stepEnable); 
  PIDAbs.setKp(10.3022952672455);
  PIDAbs.setTi(300);
  PIDAbs.setTd(0);
}

void loop(void) {

  if (enable) {
    
    step();
    enable=false;
  }
}

void stepEnable() {

  enable=true;
}

void step(){
  
  HeatShield.actuatorWrite(u);
  r=40.0;
  y=HeatShield.sensorReadTemperature();
  e=r-y;
  u=PIDAbs.compute(e,0.0,100.0,-100.0,100.0);
    
  Serial.print(y);
  Serial.print(" ");
  Serial.print(u);
  Serial.print(" ");
  Serial.println(r);
}
```
![bez nazvu](https://user-images.githubusercontent.com/23738757/40050933-3f97931a-5839-11e8-940c-04a633e233a8.png)

# <a name="hardware"/>Detailed Hardware Description

The HeatShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

For those who wish to use the board without the library, the components are connected to the following pins:

![Heat_PIN](https://user-images.githubusercontent.com/18485913/55667701-20afaf00-5860-11e9-8070-3cb1de7f9597.png)

## Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the HeatShield from [here](https://github.com/richardsalini/HeatShield/files/1968266/HeatShield_Circuit.zip).

![HeatScheme](https://user-images.githubusercontent.com/18485913/55666313-644ced80-584d-11e9-86c1-06162b22571e.png)

The power circuitry is driven by an N-channel metal-oxide semiconductor field-effect transistor (MOSFET) **Q1** connected to the PWM capable D3 pin of the Arduino. A series resistor **R5** protects the microcontroller unit (MCU) **U1** in transients, while a resistor **R6** parallel to ground ensures that floating electrical states do not cause the heater to turn off accidentally.

Normal temperatures used to melt plastics in 3D printing are over 200°C and may reach as high as 320°C for polycarbonate filaments. To make the experiment safer for general classroom use, the maximal temperature of the heating block is limited at ~80°C by an adjustable linear voltage regulator **U2**, which takes the external 12V input from the Vin pin of the MCU and reduces it to a voltage configured by resistors **R3** and **R4** supplying its adjustable input.

Temperature feedback is based on a negative temperature coefficient (NTC) thermistor **R1** connected to the A0 analog input pin of the MCU in a voltage divider circuit paired with a resistor **R2**. 

The exact component specification and required quantities are given next. Note that only components with through-hole technology (THT) mounting are utilized in order to make assembly and servicing easy even if you are inexperienced with electronics.

## Parts
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

## PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100x100mm limit of most board manufacturers. The DIPTrace PCB layout of the HeatShield can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1968264/HeatShield_PCB.zip), while the ready-to-manufacture Gerber files are available from [here](https://github.com/richardsalini/HeatShield/files/1968257/HeatShield_Gerber.zip).

![pcb](https://user-images.githubusercontent.com/38358320/39538111-fd3f2502-4e3b-11e8-8d28-1c011d404a38.png)
![pcb2](https://user-images.githubusercontent.com/38358320/39538175-3adf2240-4e3c-11e8-878c-773351e0a618.png)

# <a name="about"/>About
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

## Authors
* **3D model design:** Michal Kováč
* **Hardware design:** Juraj Bavlna, Michal Kováč, Richard Köplinger, Sohaibullah Zarghoon, Richard Salíni
* **Software design:** Richard Köplinger
* **Wiki:** Martin Gulan, Gergely Takács