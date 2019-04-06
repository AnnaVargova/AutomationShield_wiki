# Introduction
The HeatShield belongs to the family of control engineering education devices for Arduino that form a part of the AutomationShield project. This particular low-cost shield demonstrates the thermal control of a 3D printer heating block implementing a resistive heating cartridge as the actuator and a negative temperature coefficient (NTC) resistor as the sensor, which creates a simple feedback loop. The maximal temperature of the heating block is maximized at ~80°C. The HeatShield also features a transparent safety enclosure.

<!-- [Heat](https://user-images.githubusercontent.com/18485913/55647718-ae4aba80-57de-11e9-9ba4-b93ec63d62b5.png)-->
![Heat2](https://user-images.githubusercontent.com/18485913/55653082-afcfaf00-57ed-11e9-9187-0f1797fd71b2.png)

For a better visualization the entire assembly was 3D-modeled using the CAD software CATIA V5R20 (Student Edition) and can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1939152/HeatShieldAssembly.zip). Note that it features the model of Arduino Uno available from [here](https://grabcad.com/library/arduino-uno-r3-shield-in-description-1).

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


## Circuit design

The circuit schematics has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the HeatShield from [here](https://github.com/richardsalini/HeatShield/files/1968266/HeatShield_Circuit.zip).

![HeatScheme](https://user-images.githubusercontent.com/18485913/55666313-644ced80-584d-11e9-86c1-06162b22571e.png)

Dependance of the NTC thermistor at the temperature defines Stein-Hart formula.Due to the fact that only the Beta factor is found in the relevant NTC thermistor documentation. In order to use the NTC thermistor as a temperature sensor it was necessary to use a voltage divider.

Since the materials used in 3D printing have a melting temperature of about 240-260°C, the maximum working temperature of the HeatShield has to be reduced.The maximum working temperature of HeatShield was set at 80°C for safety purposes. The lowering of the temperature was regulated by reducing the voltage using the adjustable LM317T voltage regulator.

Output voltage setting is secured by two resistors. The first resistor having a value of 240Ω being recommended by the manufacturer. The value of the second resistor is determined by the desired output voltage.In this case, the temperature of 80°C was reached at a voltage of 6.45V, the resistor R2 used had a value of 1kΩ. A resistor with a 1kΩ resistance value serves to protect the Arduin pins. The 10kΩ resistor provides a transistor closure. The used transistor is a MOSFET marked as BUZ11, a type of transistor that is only controlled by voltage.

## Parts
To make an HeatShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

|Part   | Name              | Type/Value/Note | PCS |
|-------|-------------------|-----------------|-----|
| Q1    | [Transistor](https://www.tme.eu/sk/details/buz11/tranzistory-s-kanalom-n-tht/on-semiconductor-fairchild/buz11-nr4941/) | BUZ11, N-channel power MOSFET, 50V, TO-220, through-hole technology (THT) mounting | 1 |
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
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded from [here](https://github.com/richardsalini/HeatShield/files/1968264/HeatShield_PCB.zip), while the ready-to-manufacture Gerber files are available from [here](https://github.com/richardsalini/HeatShield/files/1968257/HeatShield_Gerber.zip).

![pcb](https://user-images.githubusercontent.com/38358320/39538111-fd3f2502-4e3b-11e8-8d28-1c011d404a38.png)
![pcb2](https://user-images.githubusercontent.com/38358320/39538175-3adf2240-4e3c-11e8-878c-773351e0a618.png)

# About
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

## Authors
* **3D model design:** Michal Kováč
* **Hardware design:** Juraj Bavlna, Michal Kováč, Richard Köplinger, Sohaibullah Zarghoon, Richard Salíni
* **Software design:** Richard Köplinger
* **Wiki:** Martin Gulan, Richard Salíni, Juraj Bavlna, Sohaibullah Zarghoon