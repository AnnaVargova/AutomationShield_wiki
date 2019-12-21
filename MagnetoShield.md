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

# <a name="api"/>Application programming interface

## <a name="io"/>C/C++ API

### Input

### Output

## <a name="matlab"/>MATLAB API

## <a name="simulink"/>Simulink API

# <a name="examples"/>Examples

## <a name="control"/>Feedback control

## <a name="ident"/>System identification

# <a name="hardware"/>Detailed hardware description

## <a name="circuit"/>Circuit design

## <a name="parts"/>Parts

## <a name="pcb"/>PCB

# <a name="about"/>About
This shield was designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2017/2018. The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/Mihalik2018.pdf).

## <a name="authors"/>Authors
* Hardware design: Jakub Mihalík, Gergely Takács
* Software design: Jakub Mihalík
* Wiki: Martin Gulan, Gergely Takács


# Introduction

The MagnetoShield is a device designed for educational purposes of automatic control and is another product of an [AutomationShield](https://www.automationshield.com) project. This device is able to lift up and control position of a permanent magnet.   Thanks to these possibilities we are able to build up closed-loop regulation program (for instance PID regulator) and create levitation effect of the magnet. Device uses an electromagnet to generate a magnetic force which lifts up the permanent magnet. The position of the magnet is controlled by a Hall effect sensor. Because of a fast dynamic and complexity of the system device is great tool for learning and testing regulation algorithms.

[[/fig/Magneto/Magneto.png|Isometric photograph of the MagnetoShield.]]

# Library functions

In the next part below are defined functions which are part of the [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield) and are used for easier work with the MagnetoShield.

Initialization function defines which pin is used for data-transfer from hall sensor and initializes Wire library. Wire library is used for I2C communication between DA convertor and Arduino:

```
MagnetoShield.begin();
``` 

This function has as an argument float number in range 0.00 to 5.00 and sets exact output voltage on DA convertor: 

```
MagnetoShield.setVoltageV(float);
``` 

The function assigns voltage transformed into 10-bit value measured on hall sensor to the highest and the lowest possible position of the magnet. Thanks to this function we are able to use magnetic induction to determinate the position of the flying magnet. Values of the maximum and the minimum are in range 0 to 512. Chip on Arduino UNO has 10 bit resolution (0-1023) but because of using a bipolar Hall effect sensor values reach 512 maximally. This value represents measurement on Hall effect sensor without magnetic field:

```
MagnetoShield.calibration();
``` 

These functions return values discovered in calibration function as integer numbers:

```
MagnetoShield.getMin();
MagnetoShield.getMax();
``` 

The function returns current position of the magnet measured by hall sensor:

```
MagnetoShield.readHeight();
``` 

An argument of the function is desired position of levitation in percents:

```
MagnetoShield.setHeight(float);
``` 

The function sets output voltage from DA convertor. Difference between this function and setVoltageV is that previous function used as the argument float number 0.00-5.00 but this function uses 8-bit number in range 0 to 255. This version of controlling DA convertor is better to use by regulation programs:

```
MagnetoShield.setVoltage(float);
```

This function makes a difference between desired position of flying and current measured position. Returns back error which can be used in PID regulator for instance:

```
MagnetoShield.error();
``` 

# Example
## PID control of levitation
```
#include <AutomationShield.h>


float input, error, Setpoint, output;          //declaration of global vriables for .ino program
int Minimum;
int Maximum;

unsigned long Ts=688;                         //time of samplings in microseconds
/* 
 * 1) for flying without serial comunication = 688 microseconds
 * 2) flying with comunication = 1536 microseconds
 * 3) flying with comunication and converting to % = 1476 microseconds
 * there is possibility to use 1) sampling for all types but some data can be lost - doesn't have big influance 
 */
 
bool next=false;                              //variable which enable step() function

void setup() {
  Serial.begin(230400);                       //speed of serial comunication in bauds [bits/s]
  
   MagnetoShield.begin();
   MagnetoShield.calibration();
   Setpoint = MagnetoShield.setHeight(50.00); //returns hidden global value inside the function, so for flying without serial comunication, 
                                              //variable Setpoint not needed, function can be written like void function
   

   Minimum=MagnetoShield.getMin();            //getting borders for flying
   Maximum=MagnetoShield.getMax();
   
   Sampling.interruptInitialize(Ts);          //periaod of sampling
   Sampling.setInterruptCallback(stepEnable); //what happens when interrupt is called

   PIDAbs.setKp(12);                         //setting PID constants 
   PIDAbs.setTi(1.5);
   PIDAbs.setTd(0.02);

}

void loop() {                                 //execute program step() if next=true
   if(next){  
   step();
   next=false;
   }
}

void stepEnable(){                            //execute interrupt function -> enable step()
  next=true;
}

void step(){
// 1) Flying without Serial comunication - smoothiest regulation
    error = MagnetoShield.error();                           //diference between desired and real position of flying
    input=PIDAbs.compute(error,155,255,-65000,65000);        //PID regulation - values 155 and 255 depends on used MOSFET and his "permeability"
                                                             //possibility use 0 and 255 if these numbers are unknown
    MagnetoShield.setVoltage(input);                         //writes input into the system

// 2) Serial comunication shows inputs and outputs with theirs real values 
/*    output = MagnetoShield.readHeight();                    //output fom system - position of magnet  
    error = Setpoint-output;                                //diference between desired and real position of flying
    input=PIDAbs.compute(error,155,255,-65000,65000);       //create input into the system
    MagnetoShield.setVoltage(input);                        //writes input into the system
    Serial.print(output);                                   //shows inputs and outputs in Serial windows
    Serial.print(" ");
    Serial.println(input);
*/
// 3) Serial comunication shows inputs and outputs in %  
/*    output = MagnetoShield.readHeight();                                              //output fom system - position of magnet
    error = Setpoint-output;                                                          //diference between desired and real position of flying
    input=PIDAbs.compute(error,155,255,-65000,65000);                                 //create input into the system
    MagnetoShield.setVoltage(input);                                                  //writes input into the system
    float outputPer = AutomationShield.mapFloat(output,Minimum,Maximum,0.00,100.00);  //converts output into %
    float inputPer = AutomationShield.mapFloat(input,155.00,255.00,0.00,100.00);      //converts input into %
    Serial.print(outputPer);                                                          //shows inputs and outputs in Serial windows as %
    Serial.print(" ");
    Serial.println(inputPer);
*/    
}
```

# Hardware description

An area of levitation is surrounded by transparent tube enclosed on the top. These borders prevent the magnet from drawing up to electromagnet under the influence magnetic field of the permanent magnet and disable oscillations of the magnet in horizontal plane.  Parameters of the tube and the gate where is electromagnet located depend on used permanent magnet. In my case I used a neodymium disc with diameter 8 mm and 2 mm thick. By me used parameters you can see on the picture below.

[[/fig/Magneto/Branicka.png| Parameters of the transparent tube and the height of the electromagnet.]]

The MagnetoShield is a part of open-source project the AutoamtionShield. In the text below are circuit schematics, PCB layout and are described all components of the MagnetoShield. If you have ideas how to make hardware or software better please let us know, any other improvements are welcome.

## Circuit

The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the OptoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MotoShield_Circuit.zip). 

[[/fig/Magneto/Magneto_schema.jpg|MagnetoShield circuit.]]

Electromagnet is supplied by 12 V from pin Vin on the Arduino board. Schematic sign of electromagnet is not in the circuit schematics. Electromagnet is connected to the pins + and - in the right bottom corner of the schema. Supply voltage of the electromagnet is regulated by MOSFET type IRF520. Gate of the MOSFET is connected to the DA convertor PCF8591. Advantage of the DA convertor over a PWM signal is that DA convertor creates a real analog value of the voltage in range 0 to 5 V. The DA convertor is supplied by 5 V from the Arduino board and is controlled through I2C communication protocol.  For the I2C communication are used pins SDA and SCL. On these pins are connected two pull-up resistors too. Values of resistors are 10 k?. On the DA convertor supply pin and the output pin are connected two LED diodes too. There are also two resistors. The first one with value 270 ? is before LED diode connected to supply voltage. The second one is before LED diode connected to the output from the DA convertor and has value 1.2 k?. These diodes signalize that device is working. There is also bipolar hall sensor for controlling the position of the magnet. 


## Components

|Part              | Name             | Value | PCS  | Note                       |
|------------------|------------------|-------|------|----------------------------|
| Electromagnet      | P20/15   | -    | 1    |  [URL](https://www.ebay.com/itm/322722704471)|
| Hall effect sensor            | A1302ELHLT-T            |  -    |  1   |  [URL](https://uk.rs-online.com/web/p/hall-effect-sensor-ics/6807119/)|
| Mosfet            | IRF520  |  -    |  1   |      -                     |
| DA convertor             | PCF8591T    |  -   | 1    |  [URL](http://sk.farnell.com/nxp/pcf8591t-2-518/adc-single-8bit-11-1ksps-soic/dp/2400442RL?st=PCF8591)|
| R1                | Resistor         | 10 k?    |   1   |   -   |
| R2               | Resistor         | 10 k?  | 1    |      SMD  0805          |
| R5               | Resistor         | 270 ?  | 1    |      SMD  0805          |
| R6               | Resistor         | 1.2 k?  | 1    |      SMD  0805          |
| C1               | Capacitor         | 0.1 ?F  | 1    |      SMD  0805          |
| D1               | LED         |   |  1  |      SMD 0805 color-red         |
| D2               | LED         |   |  1  |      SMD 0805 color-green          |
|                  | Header           | 10 pin| 1    | long, stackable            |
|                  | Header           | 8 pin | 2    | long, stackable            |
|                  | Header           | 6 pin | 1    | long, stackable            |
|                  | Aluminium plate  | 50x20x2 mm  |   1   |     -        |
|                  | Spacer bolt  | 35 mm  |   4   |  thread M5         |
|                  | Plastic screw  |  -  |   4   |  thread M5       |
|                  | rubber ?O? ring  |  inside diameter 12 mm  |   1   |   -   |
|                  | transparent tube  |  height 9 mm; 10x12 mm  |   1   |   -   |
|                  |                  |       |      |                            |



## PCB layout

The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Magneto/MagnetoShield_gerber.zip).

[[/fig/Magneto/PCB1.jpg|MagnetoShield PCB from the front.]]
[[/fig/Magneto/PCB2.jpg|MagnetoShield PCB from the back.]]
