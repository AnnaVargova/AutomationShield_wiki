# Introduction

FloatShield is a didactic tool based on the Arduino platform. This project has been created as a part of the [AutomationShield](https://www.automationshield.com) project and is aimed at teaching and learning of control engineering concepts.

The basic design of FloatShield consists of a potentiometer, a distance sensor, a translucent vertical tunnel containing a ball (reference item) that is floating inside and a computer fan attached to our own-design PCB. A transparent tube and a distance sensor is mounted by 3D printed parts.
The function principle is based on controlling the distance of the reference item using the PID feedback controller. Manual control is operated by power regulation of the fan through a potentiometer, which is scaled to a pulse width modulated signal (PWM).

photo shield

photo shield



# Library functions

# Hardware insight 

The Float Shield is an open-source hardware product, dedicated to be widely spread among a control engineering community. Feel free to use any part of it for your own experiments. If you come up with improvements, please let us know so we can improve our design as well. A documentation posted below might help you while working on improvised experimental prototype on breadboard or perforated board. 

## 3D assembly

Firsty, the whole assembly was designed in CAD software and forwarded to 3D print service afterwards. Overall, there are three parts to be printed, a tube clamp, a tube lid and a sensor holder. Feel free to download ready-to-print [parts](https://github.com/gergelytakacs/AutomationShield/files/1925550/FloatShield.zip).

<img width="800" alt="FloatShield_parts" src="https://user-images.githubusercontent.com/37963774/39097690-fb94c628-465f-11e8-8892-587151f1e415.png">

## Circuit design

The circuit schematics has been designed in the Freeware version of the DIPTrace CAD software. You may download the circuit schematics of the FloatShield from [here](https://github.com/gergelytakacs/AutomationShield/files/1934584/Float_scheme.zip).

<img width="1800" alt="floatshield_schemepng" src="https://user-images.githubusercontent.com/37963774/38952961-91a10444-434d-11e8-9ab2-7707e9070782.png">

## Components


| Part | Name                       | Value  | PCS | Note                       |
|------|----------------------------|--------|-----|----------------------------|
| POT1 | Potentiometer              | 10 k   | 1   | Pin Size 12.5x10           |
| R1   | Resistor                   | 1 k    | 1   | O2.5x6.8mm                 |
| R2   | Resistor                   | 10 k   | 1   | O2.5x6.8mm                 |
| D1   | Diode                      |        | 1   | 1N4001-DCO                 |
| C1   | Mosfet                     |        | 1   | IRF520N                    |
| J1   | Jumper on sensor           |        | 1   | 2.54mm, 6-pin, 280372-2    |
| J2   | Jumper on fan              |        | 1   | 2.54mm, 2-pin, 280370-2    |
|      | Connector to sensor        |        | 1   | 2.54mm, 6-pin, 280360      |
|      | Connector to fan           |        | 1   | 2.54mm, 2-pin, 280358      |
|      | Conntact to connector      |        | 14  | 2.54mm, 182206-2           |
|      | Fan                        |        | 1   | Sunon PMD1204PQB1          |
|      | Sensor Time of flight      |        | 1   | VL5310X                    |
|      | Wire                       |        | 1   | 1m, VFL 4x0,14             |
|      | Transparent pipe           |        | 1   | diam. 35.5mm, 0.4m         |
|      | Cork ball                  |        | 1   | diam. 30mm                 |
|      | Cotton ball                |        | 1   | diam. 30mm                 |
|      | Polystyrene ball           |        | 1   | diam. 30mm                 |
|      | Plastic profile type U     |        | 1   | 8,0mm x 330mm, hide cable  |
|      | Set shaft on potentiometer |        | 1   | diam. 5mm x 18.7mm, 14187-NE |
|      | Header                     | 10 pin | 1   | long, stackable            |
|      | Header                     | 8 pin  | 2   | long, stackable            |
|      | Header                     | 6 pin  | 1   | long, stackable            |


## PCB layout

The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/files/1916198/FloatShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/files/1916200/FloatShield_Gerber.zip).

<img width="500" alt="pcbfront" src="https://user-images.githubusercontent.com/37963774/38953392-e529b61e-434e-11e8-946b-e3f4d14f5c98.png">
<img width="500" alt="pcbback" src="https://user-images.githubusercontent.com/37963774/38953332-b74ee67e-434e-11e8-8e5b-12c815c9a0a9.png">



# About

The board was developed within the framework of semester project at the Institute of Automation, Measurement and Applied Informatics of the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018. 

## Authors

* 3D-model design: Marcel Vdoleček
* Hardware design: Peter Šálka, Miloš Podbielančík, Dávid Šroba 
* Software design: Gábor Penzinger, Jakub Kulhánek
* Wiki documentation: Martin Lučan, Dávid Šroba, Miloš Podbielančík