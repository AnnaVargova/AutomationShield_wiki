# Introduction

FloatShield is a didactic tool based on Arduino platform. This project has been created by  students of control engineering as a part of AutomationShield projects and is aimed at students of control engineering.
The basic design of FloatShield consists of a potentiometer, a distance sensor, a reference item and a computer fan attached to our own-design PCB. A transparent tube and a distance sensor is mounted by 3D printed parts.
The function principle is based on controlling the distance of the reference item using the PID feedback controller. Manual control is operated by power regulation of the fan through a potentiometer, which is scaled to a pulse width modulated signal (PWM).


# Hardware insight 

The Float Shield is an open-source hardware product, dedicated to be widely spread among a control engineering community. Feel free to use any part of it for your own experiments. If you come up with improvements, please let us know so we can improve our design as well. A documentation posted below might help you while working on improvised experimental prototype on breadboard or perforated board. 

## Circuit design

![floatshield_scheme](https://user-images.githubusercontent.com/37963774/38776699-6c15ab8a-409b-11e8-91c4-a139d820b9f6.jpg)

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
|      | Transparent pipe           |        | 1   | O35.5mm, 0.4m              |
|      | Cork ball                  |        | 1   | O30mm                      |
|      | Cotton ball                |        | 1   | O30mm                      |
|      | Polystyrene ball           |        | 1   | O30mm                      |
|      | Plastic profile type U     |        | 1   | 8,0mm x 330mm, hide cable  |
|      | Set shaft on potentiometer |        | 1   | O5mm x 18.7mm, 14187-NE    |
|      | Header                     | 10 pin | 1   | long, stackable            |
|      | Header                     | 8 pin  | 2   | long, stackable            |
|      | Header                     | 6 pin  | 1   | long, stackable            |
````

## PCB layout



# Library functions