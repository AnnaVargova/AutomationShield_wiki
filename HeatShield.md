# INTRODUCTION
The aim of this project was to create an instructive device called a HeatShield. This is a simple and affordable project in which emphasis are placed on timing and feedback control.
The project was inspired by 3D printing, from which an aluminum block (thermal block) was used and allowed for the simple installation of a temperature sensor (NTC thermistor) and a heating patron. The maximum working temperature of the Heatshield was set at 80°C. Due to safety features, a specific safety box had to be designed to eliminate the risk of injury. HeatShield is part of an [AutomationShield](https://www.automationshield.com) project group for Arduino.

# 3D VISUALIZATION
3D model was designed in software CATIA V5R20 (student freeware version). Arduino UNO 3D model was used and downloaded from [here](https://grabcad.com/library/arduino-uno-r3-shield-in-description-1).

![3d](https://user-images.githubusercontent.com/38358320/38812667-413d8e94-418d-11e8-9dc3-1854140b5a3a.jpg)


# DETAILED HARDWARE DESCRIPTION


## Circuit schematics

The following figure shows the HeatShield electrical circuit diagram. It is a simple electrical circuit. The meaning of the individual parts of the circuit is given below. The scheme was designed in [DIPTrace](https://diptrace.com/) software (freeware version). You can download circuit schematics layout [here](https://github.com/richardsalini/HeatShield/files/1935400/HeatShield_Circuit.zip).


![scheme](https://user-images.githubusercontent.com/38358320/39093157-25df132e-461b-11e8-9a21-0880fe9cd45f.png)

Dependance of the NTC thermistor at the temperature defines Stein-Hart formula.Due to the fact that only the Beta factor is found in the relevant NTC thermistor documentation. In order to use the NTC thermistor as a temperature sensor it was necessary to use a voltage divider.

Since the materials used in 3D printing have a melting temperature of about 240-260°C, the maximum working temperature of the HeatShield has to be reduced.The maximum working temperature of HeatShield was set at 80°C for safety purposes. The lowering of the temperature was regulated by reducing the voltage using the adjustable LM317T voltage regulator.

Output voltage setting is secured by two resistors. The first resistor having a value of 240Ω being recommended by the manufacturer. The value of the second resistor is determined by the desired output voltage.In this case, the temperature of 80°C was reached at a voltage of 6.45V, the resistor R2 used had a value of 1kΩ. A resistor with a 1kΩ resistance value serves to protect the Arduin pins. The 10kΩ resistor provides a transistor closure. The used transistor is a MOSFET marked as BUZ11, a type of transistor that is only controlled by voltage.

# PCB
A PCB (printed circuit design) was designed and created in [DIPTrace](https://diptrace.com/) software (freeware version). The folowing diagram shows both sides of the PCB. The PCB layout may be downloaded [here](https://github.com/richardsalini/HeatShield/files/1935396/HeatShield_PCB.zip).

![pcb1](https://user-images.githubusercontent.com/38358320/39092967-e3861f84-4617-11e8-9536-c7d2b333961c.png)
![pcb2](https://user-images.githubusercontent.com/38358320/39092994-4b031b6c-4618-11e8-90be-81771ea70638.png)

# COMPONENTS
Here is a list of components used in the project.

| Name              | Type/Value   | Number of pieces |
|-------------------|--------------|------------------|
| Transistor        | MOSFET-BUZ11 | 1                |
| Voltage regulator | LM317T       | 1                |
| Resistor          | 240Ω         | 1                |
| Resistor          | 1kΩ          | 2                |
| Resistor          | 10kΩ         | 1                |
| Resistor          | 100kΩ        | 1                |
| Aluminum cube     |              | 1                |
| Heating patron    |              | 1                |
| Thermistor NTC    | NTC3950      | 1                |
| Temperature isolator |           | 1                |
| Bolt              | M6x10        | 3                |
| Bolt              | M6x20        | 3                |
| Bolt              | M3x8         | 3                |
| Nut               | M3           | 3                |

# ABOUT
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.

## Authors
* 3D Model:
* Hardware design:
* Software design: 
* Wiki: 