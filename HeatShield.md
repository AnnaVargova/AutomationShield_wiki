# INTRODUCTION
The aim of this project was to create an instructive device called a HeatShield. This is a simple and affordable project in which emphasis are placed on timing and feedback control.
The project was inspired by 3D printing, from which an aluminum block (thermal block) was used and allowed for the simple installation of a temperature sensor (NTC thermistor) and a heating patron. The maximum working temperature of the Heatshield was set at 80°C. Due to safety features, a specific safety box had to be designed to eliminate the risk of injury. HeatShield is part of an AutomationShield project group for Arduino.

# 3D VISUALIZATION
3D model was designed in software CATIA V5R20 (student freeware version).
![3d](https://user-images.githubusercontent.com/38358320/38812667-413d8e94-418d-11e8-9dc3-1854140b5a3a.jpg)


# DETAILED HARDWARE DESCRIPTION
The following figure shows the HeatShield electrical circuit diagram. It is a simple electrical circuit. The meaning of the individual parts of the circuit is given below. The scheme was designed through DIPTrace software (freeware version).
![schema](https://user-images.githubusercontent.com/38358320/38783464-a8084960-4102-11e8-892e-114c7f417ea8.jpg)

## Electrical circuit (Measuring temperature circuit part)
Dependance of the NTC thermistor at the temperature defines Stein-Hart formula.Due to the fact that only the Beta factor is found in the relevant NTC thermistor documentation. In order to use the NTC thermistor as a temperature sensor it was necessary to use a voltage divider.

Since the materials used in 3D printing have a melting temperature of about 240-260°C, the maximum working temperature of the HeatShield has to be reduced.The maximum working temperature of HeatShield was set at 80°C for safety purposes. The lowering of the temperature was regulated by reducing the voltage using the adjustable LM317T stabilizer.
![heating](https://user-images.githubusercontent.com/38358320/38783658-7f64f136-4105-11e8-9820-9eea8925aa7f.png)
Output voltage setting is secured by two resistors. The first resistor having a value of 240Ω being recommended by the manufacturer. The value of the second resistor is determined by the desired output voltage.In this case, the temperature of 80°C was reached at a voltage of 6.45V, the resistor R2 used had a value of 1kΩ. A resistor with a 1kΩ resistance value serves to protect the Arduin pins. The 10kΩ resistor provides a transistor closure. The used transistor is a MOSFET marked as BUZ11, a type of transistor that is only controlled by voltage.

# PCB
A PCB (printed circuit design) was designed and created through DIPTrace software (freeware version). The folowing diagram shows both sides of the PCB.
![pcb1](https://user-images.githubusercontent.com/38358320/38783725-5b73f08c-4106-11e8-8614-4f86847e732a.png)
![pcb2](https://user-images.githubusercontent.com/38358320/38783727-5f893fe2-4106-11e8-8fd7-8cb86c64c6af.png)

# COMPONENTS
Here is a list of components used in the project.
![components](https://user-images.githubusercontent.com/38358320/38783763-18e9a792-4107-11e8-8b77-0259dcd5be54.png)

# ABOUT
This shield was designed and created for the subject of Microcomputers and Microprocessor Technology at the Institute of Automation, Measurement and Applied Informatics. The Institute belongs to the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava.
