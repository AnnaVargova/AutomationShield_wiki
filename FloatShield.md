#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[C/C++ API](#io)<br/>
&nbsp;&nbsp;&nbsp;[MATLAB API](#matlab)<br/>
&nbsp;&nbsp;&nbsp;[Simulink API](#simulink)<br/>
[Examples](#examples)<br/>
&nbsp;&nbsp;&nbsp;[System identification](#ident)<br/>
&nbsp;&nbsp;&nbsp;[Feedback control](#control)<br/>
[Detailed hardware description](#hardware)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

### Input

### Output

## <a name="matlab"/>MATLAB API

## <a name="simulink"/>Simulink API

# <a name="examples"/>Examples

## <a name="ident"/>System identification

## <a name="control"/>Feedback control

# <a name="hardware"/>Detailed hardware description
The FloatShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design
The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics of the FloatShield from [here](https://github.com/gergelytakacs/AutomationShield/files/1934584/Float_scheme.zip).

![float_pcb](https://user-images.githubusercontent.com/18485913/71244839-b4081200-2313-11ea-8cb3-41aff3b5493e.png)

The electronic schematics of the FloatShield are shown in Fig. X, while some parts are visible on the photograph in Fig. X as well.

The TOF sensor and the fan are only represented by their connectors, while we assume that 12 V external power is drawn through the VIN pin of the Arduino. The fan is powered by an N-channel MOSFET (l), and driven by the D3 PWM capable microcontroller pin through an 1 kΩ current limiting resistor (m). Floating states are handled by 10 kΩ pull-down resistor (n), while a diode (o) ensures back electromotive-force (EMF) protection. A connector (p) finally leads to the fan terminals. Since the fan requires 12 V and more current than the USB powered Arduino can handle, a separate wall adapter power supply is required to operate the device. Since the TOF sensor is integrated to a convenient open-source breakout board, it can be effortlessly connected (q) to the I2C bus of the Arduino (SDA,SCL). Finally, the potentiometer (r) runner is attached to the A0 ADC capable pin of the board.

## <a name="parts"/>Parts
To make an FloatShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Part    	| Name               	| Type/Value/Note                                                     	| PCS 	|
|-----------	|--------------------	|-----------------------------------------------------------------	|-------|
| (b)       	| PCB                	| FR4, 2 layer, 1.6mm thick                                      	| 1    	|
| (c)       	| Fan                	| Axial, 12 V, 40X40 mm, 24.0 CFM; e.g. Sunon PMD1204PQB1       	| 1    	|
| (d)       	| Tube clamp         	| 3D printed, 16g filament, print time 2:24h                      	| 1    	|
| (e)       	| Tube               	| Clear, Ø35.5 mm, wall approx. 0.6mm, 0.4m; e.g. no. 113816 	        | 0.4m 	|
| (f)       	| Ball               	| Cork, Ø30 mm; e.g. no. 108269                                 	| 1    	|
| (g)       	| Tube flange        	| 3D printed, 5.8g filament, print time 57min                    	| 1    	|
| (h)       	| Sensor holder      	| 3D printed, 4g filament, print time 43min                      	| 1    	|
| (i)       	| Sensor             	| ST Microelectronics VL5310X TOF sensor on a breakout board      	| 1    	|
| (j)       	| Wire               	| 1m, 4 lead, 0.15mm2, multi-conductor ribbon; e.g. VFL 4x0,14    	| 1    	|
| (k)       	| Cable shaft        	| U-shape, 8x330mm, ASA polymer; e.g. 11796                     	| 1    	|
| (l), C1    	| MOSFET             	| IRF520, TO-220AB, e.g. IRF520NPBF                               	| 1    	|
| (m), R1   	| Resistor           	| 1kΩ, 2.5x6.8mm, THT                                             	| 1    	|
| (n), R2   	| Resistor           	| 10kΩ, 2.5x6.8mm, THT                                            	| 1    	|
| (o), D1   	| Diode              	| 1N4001, e.g 1N4001-DCO                                          	| 1    	|
| (p)       	| Connector (fan)    	| 2x1 pin, 0.1" pitch; e.g. TE Connectivity 280358               	| 1    	|
| (p), J2   	| Jumper (fan)       	| 2x1 pin, 0.1" pitch; e.g. TE Connectivity 280370-2               	| 1    	|
| (q)       	| Connector (sensor) 	| 6x1 pin, 0.1" pitch; e.g. TE Connectivity 280360                 	| 1    	|
| (q), J1   	| Jumper (sensor)    	| 6x1 pin, 0.1" pitch; e.g. TE Connectivity 280372-2              	| 1    	|
| (q),(p)   	| Connector pins     	| e.g. TE Connectivity 182206-2                                 	| 14   	|
| (r)       	| Turning knob       	| 5x18.7mm; e.g. ACP 14187-NE                                     	| 1    	|
| (r), POT1 	| Potentiometer      	| 10kΩ                                                            	| 1    	|
| (s)       	| Header             	| 10x1 pin, female, long, stackable, 0.1" pitch                  	| 1    	|
| (s)       	| Header             	| 8x1 pin, female, long, stackable, 0.1" pitch                   	| 2    	|
| (s)       	| Header             	| 6x1 pin, female, long, stackable, 0.1" pitch                   	| 1    	|
| -         	| Bolts              	| DIN 912 M3x40                                                 	| 4    	|
| -         	| Bolts              	| DIN 912 M3x16                                                 	| 2    	|
| -         	| Nuts               	| DIN 934 M3                                                      	| 6    	|
| -         	| Screws             	| DIN 7981F 2.9x9.5                                             	| 2    	|
| -         	| Washers            	| DIN125 A 3.2x7x0.5 Polyamide washers                          	| 4    	|
| -         	| Standoffs          	| TFM-M3/10                                                       	| 4    	|

Note that the total cost of the above components and thus of the entire FloatShield is no more than $30 excluding labor and postage.

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/files/1916198/FloatShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/files/1916200/FloatShield_Gerber.zip).

The assembled shield is fixed mechanically and electrically to the Arduino board through stackable header pins. The location of the components is also illustrated in the PCB layout below. The axial fan is mounted to the board on standoffs and a hole cut at the manufacturing stage of the PCB increases the air intake capacity.

<img width="500" alt="pcbfront" src="https://user-images.githubusercontent.com/37963774/39986438-ef45542e-5761-11e8-8869-8ff14c35a95a.png">
<img width="500" alt="pcbback" src="https://user-images.githubusercontent.com/37963774/39986439-ef812314-5761-11e8-88a9-eab1ce675225.png">

# <a name="about"/>About
This shield was designed and created as a term project at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2017/2018.

## <a name="authors"/>Authors
* 3D-model design: Marcel Vdoleček
* Hardware design: Peter Šálka, Miloš Podbielančík, Dávid Šroba, Martin Lučan
* Software design: Gábor Penzinger, Jakub Kulhánek
* Wiki documentation: Martin Gulan, Gergely Takács


# Introduction

FloatShield is a didactic tool based on the Arduino platform. This project has been created as a part of the [AutomationShield](https://www.automationshield.com) project and is aimed at teaching and learning of control engineering concepts. The FloatShield consists of a translucent vertical tunnel containing a ball that is floating inside via an air current generated by a fan. The goal is to control the position of the ball within this column by the means of a feedback controller.	

The basic design of FloatShield consists of a potentiometer, a distance sensor, a translucent vertical tunnel containing a ball (reference item) that is floating inside and a computer fan attached to our own-design PCB. A transparent tube and a distance sensor is mounted by 3D printed parts.
The function principle is based on controlling the distance of the reference item using the PID feedback controller. Manual control is operated by power regulation of the fan through a potentiometer, which is scaled to a pulse width modulated signal (PWM).

<img width="500" alt="pcbback" src="https://user-images.githubusercontent.com/37963774/39859491-5725bf50-543a-11e8-86ae-32686be9a22b.JPG">


# Library functions

This function needs to be called during setup. It will initiate the sensor.

` void initialize(); `

This function has to be called during setup before initialize. It will print out debug data for the sensor if necessary.

` void debug(); `


This function will return current potentiometer position in percent.

` int referencePercent(); ` 


This function will return current position of the ball as read by the sensor in percent.

` int positionPercent();  ` 


This function will drive the ventilator. Argument of this function is in percent from 0 to 100 - full power.

` void ventInPercent(int value); ` 


Calling this function will switch floatshield into manual control mode. By adjusting the potentiometer ventilator power will adjust accordingly. 

` float manualControl(); `  


This function will return current distance between the sensor and the ball in millimeters.

` int  positionMillimeter(); ` 


This function is recommended to be called during setup. It will run an algorithm, which will adjust minimum and maximum distatnce between the sensor and the ball, so that it corrensponds to 0 and 100% when read.

` void calibrate();`


# Example

## Open-loop control
```
#include <AutomationShield.h>
#include <FloatShield.h>

int dist;


void setup() {
  // FloatShield.debug();     will print out in monitor sensor debug data
  Serial.begin(115200);  //start serial communication
  FloatShield.initialize(); //FloatShield initialization
  FloatShield.calibrate(); //FloatShield calibration for more accurate measurements
}

void loop() {
  FloatShield.manualControl(); //Calling this function will switch floatshield into manual
                               //control mode. By adjusting the potentiometer ventilator power
                               //will adjust accordingly.
  dist = FloatShield.positionMillimeter(); // read sensor 
  Serial.print("Distance (mm): ");
  Serial.println(dist);
}

```
## PID control
```
#include <AutomationShield.h>
#include <FloatShield.h>

int dist;
unsigned long int Ts = 100; //sampling time in ms
bool enable = false; //flag 
int y;  //output
float u; //input
int r; //reference
float Ti,Td,Kp; // PID constants
float error; //error
void setup() {
  Serial.begin(115200); //start serial communication
  FloatShield.initialize(); //FloatShield initialization
  FloatShield.calibrate(); //FloatShield calibration for more accurate measurements
  Sampling.interruptInitialize(Ts*1000); //Sampling initialization in microseconds
  Sampling.setInterruptCallback(StepEnable); // seting the interrupt functon
  //Setting the PID constants
  PIDAbs.setKp(0.86);
  PIDAbs.setKi(1.729);
  PIDAbs.setKd(0.1081);
  //Getting constants needed for calculating PID in absolute form
  Ti = PIDAbs.getTi(); // integral time constant
  Td = PIDAbs.getTd(); // derivative time constant
  Kp = PIDAbs.getKp(); 
}

void loop() 
{
  if(enable)
  {
    Step();
    enable = false; //enable flag becomes false after the execution of the Step() function
  }
}

void StepEnable(void) // interrupt function
{
  enable = true; // when the interrupt function is called, the enable is true
}

void Step (void)
{
  y = FloatShield.positionPercent(); //reading the  sensor value
  r = FloatShield.referencePercent(); //reading the reference value

  error = r - y; 
  u = PIDAbs.compute(error,25,55,0,100); // absolute PID function with anti-wind up and saturation
  FloatShield.ventInPercent(u);
  Serial.print(r);
  Serial.print(", ");
  Serial.print(u);
  Serial.print(", ");
  Serial.println(y);
}

```
 
## 3D assembly

Firstly, the whole [assembly](https://github.com/gergelytakacs/AutomationShield/files/1942312/assembly.zip) was designed in CAD software and forwarded to 3D print service afterwards. Overall, there are three parts to be printed, a tube clamp, a tube lid and a sensor holder. One might use a honeycomb tube insert to modify turbulent air flow to be way more laminar. Feel free to download ready-to-print [parts](https://github.com/gergelytakacs/AutomationShield/files/1977698/parts.zip). Other assembly parts, a [fan](https://grabcad.com/library/fan-40-x-40-x-28-1) and [Arduino microcontroller](https://grabcad.com/library/arduino-uno-r3-4) are downloadable from GrabCAD database.

<img width="600" alt="FloatShield_parts" src="https://user-images.githubusercontent.com/37963774/39174444-154dc80a-47a8-11e8-9d1e-c78b2c2862db.jpg">





