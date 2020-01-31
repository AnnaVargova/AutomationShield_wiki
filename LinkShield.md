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
| (d),M1   	   | Servo           | digital, high-speed metal gear micro-servo | 1   |
| (v),U2           | Accelerometer   | Analog Devices ADXL345                     | 1   |
| (h),C2           | Capacitor       | 0805, tantalum, 10µF                                                  | 1   |
| —   	           | Enclosure top   | clear acrylic; e.g. h=2 mm, stamped to the outer diameter of the tube | 1   |
| (m),U4           | Current sensor  | INA149                                                                | 1   |
| (b),U3           | [DAC](http://sk.farnell.com/nxp/pcf8591t-2-518/adc-single-8bit-11-1ksps-soic/dp/2400442RL?st=PCF8591)             | PCF8591T                                                              | 1   |

# <a name="about"/>About
This shield is currently designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI). The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018.
<!-- The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/xxx.pdf). -->

## <a name="authors"/>Authors
* Hardware design: Martin Vríčan, Erik Mikuláš 
* Software design: Gergely Takács
* Wiki: Martin Gulan, Gergely Takács