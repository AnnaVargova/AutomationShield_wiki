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
| (d),M1   	   | Servo           | digital, high-speed metal gear micro-servo, e.g. Savox SH-0257MG | 1   |
| (v),U2           | Accelerometer   | Analog Devices ADXL345                          | 1   |
| (h),J1,J2        | Connector       | 0.5mm pitch, 4-lead FFC/FPC, e.g. 52745-0497    | 2   |
| (i),POT1         | Potentiometer   | 250 mW, e.g. ACP CA14NV12,5-10KA2020            | 1   |
| R1,R2            | Resistor        | 0805, 10kΩ, e.g. [ROYAL OHM 0805S8J0103T5E](https://www.tme.eu/sk/details/smd0805-10k/rezistory-smd-0805/royal-ohm/0805s8j0103t5e/)       | 2   |
| (f),C1           | Capacitor       | 0805, tantalum, 4.7µF, e.g. AVX TAJP475K016RNJV | 1   |
| (u),C2           | Capacitor       | 1206, tantalum, 10µF, e.g. T491A106M016AT       | 1   |
| (t),C3           | Capacitor       | 0805, ceramic, 100nF, e.g. C0805C104M5RACTU     | 1   |

# <a name="about"/>About
This shield is currently designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI). The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018.
<!-- The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/xxx.pdf). -->

## <a name="authors"/>Authors
* Hardware design: Martin Vríčan, Erik Mikuláš 
* Software design: Gergely Takács
* Wiki: Martin Gulan, Gergely Takács