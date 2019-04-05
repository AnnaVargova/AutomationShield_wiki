# Introduction
The HeatShield belongs to the family of control engineering education devices for Arduino that form a part of the AutomationShield project. This particular low-cost shield demonstrates the thermal control of a 3D printer heating block implementing a resistive heating cartridge as the actuator and a negative temperature coefficient (NTC) resistor as the sensor, which creates a simple feedback loop. The maximal temperature of the heating block is maximized at ~80°C. The HeatShield also features a transparent safety enclosure.

![isoHeatShield](https://user-images.githubusercontent.com/18485913/55643295-18f5f900-57d3-11e9-9b5b-0ecdd72ef6e4.jpg)
![cageHeatShield](https://user-images.githubusercontent.com/18485913/55643507-9a4d8b80-57d3-11e9-8a1b-41d3a76e7c84.jpg)


# 3D Visualization
3D model was designed in software CATIA V5R20 (student freeware version). Arduino UNO 3D model was used and downloaded from [here](https://grabcad.com/library/arduino-uno-r3-shield-in-description-1). The entire assembly of the HeatShield 3D model is available [here](https://github.com/richardsalini/HeatShield/files/1939152/HeatShieldAssembly.zip).

![3d1](https://user-images.githubusercontent.com/38358320/40002314-a9aa6042-5790-11e8-8b4d-d476e85f654b.png)


# Library Functions
`void begin()`

This method initialize pins of the Heatshield.

`float sensorReadTemperature()`

This method returns temperature in °C, which is detect by sensor.

`void actuatorWrite(float percent)`

This method accepts as parameter action in percent and writes adequate value to drive the heating body.


# Example

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

# Detailed Hardware Description


## Circuit Schematics

The following figure shows the HeatShield electrical circuit diagram. It is a simple electrical circuit. The meaning of the individual parts of the circuit is given below. The scheme was designed in [DIPTrace](https://diptrace.com/) software (freeware version). You can download circuit schematics layout [here](https://github.com/richardsalini/HeatShield/files/1968266/HeatShield_Circuit.zip).


![schema](https://user-images.githubusercontent.com/38358320/39537574-534a42c6-4e3a-11e8-8927-0fbf338a4aca.png)

Dependance of the NTC thermistor at the temperature defines Stein-Hart formula.Due to the fact that only the Beta factor is found in the relevant NTC thermistor documentation. In order to use the NTC thermistor as a temperature sensor it was necessary to use a voltage divider.

Since the materials used in 3D printing have a melting temperature of about 240-260°C, the maximum working temperature of the HeatShield has to be reduced.The maximum working temperature of HeatShield was set at 80°C for safety purposes. The lowering of the temperature was regulated by reducing the voltage using the adjustable LM317T voltage regulator.

Output voltage setting is secured by two resistors. The first resistor having a value of 240Ω being recommended by the manufacturer. The value of the second resistor is determined by the desired output voltage.In this case, the temperature of 80°C was reached at a voltage of 6.45V, the resistor R2 used had a value of 1kΩ. A resistor with a 1kΩ resistance value serves to protect the Arduin pins. The 10kΩ resistor provides a transistor closure. The used transistor is a MOSFET marked as BUZ11, a type of transistor that is only controlled by voltage.

# PCB
A PCB (printed circuit board) was designed and created in [DIPTrace](https://diptrace.com/) software (freeware version). The folowing diagram shows both sides of the PCB. The PCB layout may be downloaded [here](https://github.com/richardsalini/HeatShield/files/1968264/HeatShield_PCB.zip).
Gerber files are available to download [here](https://github.com/richardsalini/HeatShield/files/1968257/HeatShield_Gerber.zip).

![pcb](https://user-images.githubusercontent.com/38358320/39538111-fd3f2502-4e3b-11e8-8d28-1c011d404a38.png)
![pcb2](https://user-images.githubusercontent.com/38358320/39538175-3adf2240-4e3c-11e8-878c-773351e0a618.png)

# Components
Here is a list of components used in the project.

| Name              | Type/Value   | Number of pieces |
|-------------------|--------------|------------------|
| Transistor        | MOSFET-BUZ11 | 1                |
| Voltage regulator | LM317T       | 1                |
| Resistor          | 240Ω         | 1                |
| Resistor          | 1kΩ          | 2                |
| Resistor          | 10kΩ         | 1                |
| Resistor          | 100kΩ        | 1                |
| Aluminum cube     | [Link](https://www.na3d.sk/p/2638/e3d-v6-hlinikova-kocka)             | 1                |
| Heating patron    | [Link](https://www.na3d.sk/p/2634/vyhrevne-teleso-24v-30w)             | 1                |
| Thermistor NTC    | NTC3950, [Link](https://www.na3d.sk/p/2482/termistor-pre-3d-tlaciaren-1-m-kabel)      | 1                |
| Temperature isolator |[Link](http://www.conrad.sk/izolator-sestiuhelnik-m6-is20hh625-20-mm-25-mm.k887493) |       1                
| Bolt              | M6x10        | 3                |
| Bolt              | M6x20        | 3                |
| Bolt              | M3x8         | 3                |
| Nut               | M3           | 3                |

# About
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

## Authors
* **3D model design:** Michal Kováč
* **Hardware design:** Juraj Bavlna, Michal Kováč, Richard Köplinger, Sohaibullah Zarghoon, Richard Salíni
* **Software design:** Richard Köplinger
* **Wiki:** Richard Salíni, Juraj Bavlna, Sohaibullah Zarghoon