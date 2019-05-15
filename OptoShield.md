#### Contents
[Introduction](#intro)<br/>
&nbsp;&nbsp;&nbsp;[Circuit design](#circuit)<br/>
&nbsp;&nbsp;&nbsp;[Parts](#parts)<br/>
&nbsp;&nbsp;&nbsp;[PCB](#pcb)<br/>
[About](#about)<br/>
&nbsp;&nbsp;[Authors](#authors)<br/>


# <a name="intro"/>Introduction

The OptoShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. This particular low-cost shield contains a simple circuitry implementing a light emitting diode (LED) as the actuator and a light-dependent resistor (LDR) as a sensor. The LED and LDR are enclosed in an opaque tube that blocks ambient light. The power of the LED can be varied by applying a pulse width modulated (PWM) signal to it, thus manipulating its apparent brightness. The LED and LDR thus creates a simple feedback loop that can be used in control engineering experiments.

![Opto_Iso](https://user-images.githubusercontent.com/18485913/57761786-7ea4a180-76fe-11e9-8a6c-01680d39310a.png)

# Library functions

All functions and examples associated to the OptoShield are included in the [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). The functions specific to this shield mostly perform input/output peripheral communication. [Certain library functions](https://github.com/gergelytakacs/AutomationShield/wiki/Common-functions) are common to all of our shields and are included in the `AutomationShield` class. 

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
OptoShield.actuatorWrite(u);
```
where `u` is a floating point number from the range of 0-100 %. This will send a PWM signal to both LEDs. The behavior of the visible LED is mirrored in the one located in the tube.

### Output
The output from the LDR is read by 
```
OptoShield.sensorRead();
```
where the function outputs the brightness in the range of 0-100 % as detected by the sensor circuit. This reading can be used as a feedback signal. The function returns a floating point value. 

Note that the sensor reading in percents is only available after the system has been calibrated. The sensor can be calibrated by calling
```
OptoShield.calibration();
```
in the `setup()` function, after which the `Opto.sensorRead()' will return the output readings in the correct percentual range.

A sensor reading can be requested instead of calibrated percents directly in units of voltage by calling
```
OptoShield.sensorReadVoltage();
```
which will return a floating point number. 

The board contains a second, independent LDR in addition to the one used as a sensor. This allows the user to test the functionality of the sensor and perform simple experiments.  The output from the auxiliary LDR is read by 
```
OptoShield.sensorAuxRead();
```
and similarly to `Opto.sensorReadVoltage()` the function returns the voltage on the sensor circuit as a floating point parameter. Note that this sensor is merely for testing and calibration, like in the case of the main sensor, is not possible.

### Reference

The user reference is read from the potentiometer by calling
```
OptoShield.referenceRead();
```
and the function returns the desired user reference setpoint in percents in the range of 0-100 \% as a floating point number.


## System Identification 

The functions listed below implement tests signals that can be used for system identification and modeling. The functions, when called without input parameters, run a specific identification experiment with pre-set parameters and length that we deemed suitable for identification. Upon evaluating the function, the Arduino board will start to list the results to the serial communication port. The formatting corresponds to a space-separated table format, where spaces separate columns and the line ending character begins a new line.

The results can be listed using the Arduino Serial Monitor, the Arduino IDE Serial Plotter or even logged by a number of [third party applications](http://freeware.the-meiers.org/), then exported to other software for visualization and post-processing.


# Examples

## Step Response
```
#include "AutomationShield.h"

float Setpoint = 70.00;

bool enable=false;
unsigned long int curTime=0;
unsigned long int prevTime=0;

unsigned long Ts = 0.1;

void setup() {
  
Serial.begin(115200);

Sampling.interruptInitialize(Ts * 1000);
Sampling.setInterruptCallback(stepEnable);
  
OptoShield.begin();
OptoShield.calibration();
delay(1000);
OptoShield.actuatorWrite(Setpoint);
}

void loop() {

  curTime=millis();
  if (enable) {
    step();
    enable=false;
    
  } // end of the if statement  
} // end of the loop


void stepEnable(){
  enable=true;
}

void step(){
  float Senzor = OptoShield.sensorRead();
  
  Serial.print(Setpoint);
  Serial.print(",");
  Serial.println(Senzor);

}
```
 

## PID Control

```
#include "AutomationShield.h"

unsigned long Ts = 1; // sampling time in milliseconds

bool enable=false; // flag for the sampling function

// variables for the PID

float r = 0.00;
float y = 0.00;
float u = 0.00;
float error = 0.00;


void setup() {
  
  Serial.begin(9600);
  
  OptoShield.begin(); // defining hardware pins
  OptoShield.calibration(); // calibration function for accurate measurements
  
  Sampling.interruptInitialize(Ts * 1000);  // initialize the sampling function, input is the sampling time in microseconds
  Sampling.setInterruptCallback(stepEnable); // setting the interrupts, the input is the ISR function

 // setting the PID constants
 PIDAbs.setKp(0.00173244161800246); // 0.000287897244979394 - identified value
 PIDAbs.setKi(10.0061553434575); // 5.75794489958787 - identified value
 PIDAbs.setKd(0); // 0 - identified value

}// end of the setup

void loop() {

  if (enable) {
    step();
    enable=false;
    
  }  

} // end of the loop


void stepEnable(){  // ISR
  enable=true;
}

void step(){ // we have to put our code here

r = OptoShield.referenceRead();  // reading the reference value of the potentiometer
y = OptoShield.sensorRead();    // reading the sensor value (LDR in the tube)

error = r - y; 

 u = PIDAbs.compute(error,0,100,0,100);

OptoShield.actuatorWrite(u);

Serial.print(r);
Serial.print(",");
Serial.print(u);
Serial.print(",");
Serial.println(y);
  

}
```

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