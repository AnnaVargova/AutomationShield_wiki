# This site is currently under maintenance. Please hold on.

#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
&nbsp;&nbsp;&nbsp;[Initialization and calibration](#init)<br/>
&nbsp;&nbsp;&nbsp;[Input](#input)<br/>
&nbsp;&nbsp;&nbsp;[Output](#output)<br/>
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
## <a name="init"/>Initialization and calibration
## <a name="input"/>Input
## <a name="output"/>Output

# <a name="examples"/>Examples
## <a name="control"/>Feedback control
## <a name="ident"/>System identification

# <a name="hardware"/>Detailed hardware description
## <a name="circuit"/>Circuit design
## <a name="parts"/>Parts
To make a LinkShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Part             | Name            | Type/Value/Note                                                       | PCS |
|------------------|-----------------|-----------------------------------------------------------------------|-----|
| (d),M1   	   | Servo           | digital, high-speed metal gear micro-servo,<br/>e.g. [Savox SH-0257MG](https://www.savox-servo.com/Servos/Brushed-Motor/Savox-Servo-SH-0257MG-Digital-DC-Motor-Metal-Gear/) | 1   |
| (v),U2           | Accelerometer   | [Analog Devices ADXL345](https://www.analog.com/en/products/adxl345.html#product-overview)                          | 1   |
| (h),J1,J2        | Connector       | 0.5mm pitch, 4-lead FFC/FPC, e.g. [Molex 52745-0497](https://www.tme.eu/sk/details/mx-52745-0497/konektory-ffc-fpc-raster-0-5mm/molex/52745-0497/)    | 2   |
| (i),POT1         | Potentiometer   | 250 mW, e.g. [ACP CA14NV12,5-10KA2020](https://www.tme.eu/sk/en/details/ca14v-10k/single-turn-tht-trimmers/acp/ca14nv12-5-10ka2020/)            | 1   |
| R1,R2            | Resistor        | 0805, 10kΩ, e.g. [ROYAL OHM 0805S8J0103T5E](https://www.tme.eu/sk/en/details/smd0805-10k/0805-smd-resistors/royal-ohm/0805s8j0103t5e/)       | 2   |
| (f),C1           | Capacitor       | 0805, tantalum, 4.7µF, e.g. [AVX TAJP475K016RNJ](https://www.tme.eu/en/details/tajp475k016rnj/smd-tantalum-capacitors/avx/) | 1   |
| (u),C2           | Capacitor       | 1206, tantalum, 10µF, e.g. [KEMET T491A106M016AT](https://www.tme.eu/en/details/t491a106m016at/smd-tantalum-capacitors/kemet/)       | 1   |
| (t),C3           | Capacitor       | 0805, ceramic, 100nF, e.g. [KEMET C0805C104M5RACTU](https://www.tme.eu/en/details/c0805c104m5rac/0805-mlcc-smd-capacitors/kemet/c0805c104m5ractu/)     | 1   |
| (e),D1           | Diode           | DO214AC, e.g. [Vishay BYG20J](https://www.tme.eu/sk/en/details/byg20j-e3_tr/smd-universal-diodes/vishay/), 1.5A, 600V)                       |  1   |
| (r)              | Cable           | 0.5mm pitch, 4-lead FFC                         |  1   |
| (a)              | PCB (shield)    | 2 layer, FR4, 1.6mm thick, green mask           |  1   |
| (s)              | PCB (breakout)  | 1 layer, FR4, 0.6mm thick (or less), green mask |  1   |
| (o)              | Screw           | M2×8, steel                                     |  1   |
| (p)              | Nut             | M2, steel                                       |  1   |
| (k)              | Spacer          | hexagonal, polyamide, M2, 10mm                  |  2   |
| (l)              | Screw           | M2×5, Phillips, polyamide                       |  2   |
| (m)              | Nut             | M2, polyamide                                   |  2   |
| (j)              | Shaft           | Potentiometer shaft, e.g. [ACP CA9MA9005](https://www.tme.eu/sk/en/details/ca9ma5-b/knobs-for-trimmers/acp/ca9ma-9005-black/#)  |  1   |
| (c)              | Header          | 6×1, female, 2.54mm pitch                       |  1   |
| (c)              | Header          | 8×1, female, 2.54mm pitch                       |  2   |
| (c)              | Header          | 10×1, female, 2.54mm pitch                      |  1   |
| (n)              | Hub             | 1.1g green PETG filament, 21m to print, 0.07kWh electricity   |  1   |
| —                | Magnets         | Ø9×2mm, N50, ∼13N, e.g. [Omo Magnets N50D00960020](http://www.omomagnets.com/index.php?controller=omoproduct&id_product=1193)            |  3   |
| (q)              | Beam            | 85×10×0.3mm, Ø2mm hole, 5mm from edge, AISI 301 |  1   |

Note that the total cost of the above components and thus of the entireLinkShield is no more than 22€ excluding labor and postage.

The assembled LinkShield is shown in the figure below. The servo is inserted into a prefabricated slot on the PCB and raised by 10mm using a pair of spacers (k), fixed with polyamide screws (l) from the top and nuts (m) from the bottom. The slotted cylindrical hub (n), which connects the servo shaft with the beam, was designed in Autodesk Fusion 360 and printed by a Prusa I2 MK3/S 3D printer using PETG filament in 21 minutes. The hub is held in place by a machine screw connecting the servo shaft that comes with most metal-geared servos
as standard, while the slot holding the beam is tightened and fixed by a M2×8 machine screw (o) and corresponding nut (p). The flexible cantilever beam (q) measuring 85×10×0.3mm and with a φ2mm mounting hole placed 5mm from the edge is laser-cut from stainless steel. The tip of the beam is equipped by the accelerometer unit (v), which is connected to the base board by a
4-lead flexible flat cable with a 0.5 mm pitch (r). This cable transfers power and communication to the accelerometer by a connector identical to the one located on the base board.

The accelerometer unit is based on a single layer 0.6mm thick printed circuit board (s). The PCB is glued firmly to the tip by Suxun B-7000 adhesive. Power to the accelerometer chip is filtered by a pair of 10µF (t) and 100nF (u) capacitors. The breakout PCB contains the Analog Devices ADXL345 3-axis configurable gain digital accelerometer unit (v). 

The equivalents of the circuit components described above for the schematics are marked on the assembled device with the same letters.

![PCB2](https://user-images.githubusercontent.com/18485913/73637578-f473e300-4668-11ea-8645-fb1704566928.png)

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Link/LinkShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Link/LinkShield_gerber.zip).

![PCB](https://user-images.githubusercontent.com/18485913/73541789-9c07d000-4433-11ea-9ced-fe0cbd83b94b.png)

# <a name="about"/>About
This shield is currently designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI). The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018.
<!-- The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/xxx.pdf). -->

## <a name="authors"/>Authors
* Hardware design: Martin Vríčan, Erik Mikuláš 
* Software design: Gergely Takács
* Wiki: Martin Gulan, Gergely Takács