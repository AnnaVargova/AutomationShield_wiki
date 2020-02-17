#### Contents 
[Introduction](#intro)<br/>
[Application programming interface](#api)<br/>
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
The LinkShield belongs to the family of control engineering education devices for Arduino that form a part of the [AutomationShield](https://www.automationshield.com) project. The basic design of LinkShield consists of a vertically placed servo motor connected to a horizontal flexible cantilever beam to simulate the vibration response of dynamic systems in motion—such as high-speed slender flexible robotic manipulator arms. The goal is to attenuate the vibrations of the beam measured by an accelerometer unit attached to its free end, thus creating a simple single-input single-output (SISO) feedback loop. The user may also set the reference angle of the servo manually—using a potentiometer.

![LinkShield](https://user-images.githubusercontent.com/18485913/73826463-1ce11600-47fe-11ea-9179-85e396f9c581.png)

# <a name="api"/>Application programming interface
The basic application programming interface (API) serving the device is written in C/C++ and is integrated into the open-source [AutomationShield Arduino library](https://github.com/gergelytakacs/AutomationShield). This library contains hardware drivers and sample exercises for control systems engineering education. All functionality associated with the LinkShield is included in the `LinkShield.h` header, which contains the `LinkClass` class that is constructed by default as the `LinkShield` object. The functions specific to this shield mostly perform input/output peripheral communication.

The summary of basic functions and the illustration below should get you started quickly:
* Output (sensor): `LinkShield.sensorRead();`
* Input (actuator): `LinkShield.actuatorWrite();`

![link_actsens](https://user-images.githubusercontent.com/18485913/73727688-4896ca00-4732-11ea-8a58-4d80a5b1a463.png)

Before you begin an experiment you must initialize the hardware by calling

`LinkShield.begin();`

which initializes the default Arduino servo motor library and attaches the motor to the D9 pin. To maintain both 3.3V system compatibility and analog resolution, the reference is set to external. Finally, the ADXL345 accelerometer is initialized with a 8G range and 3200Hz data rate, producing a 1600Hz bandwidth. The sensor can be optionally calibrated by running the `calibrate()` method to remove sensor bias in the direction of interest.

The acceleration sensor can be read at any time instant by calling

`float y = LinkShield.sensorRead();`

which returns a floating-point number providing acceleration data
<img src="http://latex.codecogs.com/gif.latex?y(k)=\ddot{q}(k)" border="0"/>
in m.s<sup>−2</sup>.

The servo motor can be commanded to the internally kept position of
<img src="http://latex.codecogs.com/gif.latex?u(k)" border="0"/>
degrees by the

`LinkShield.actuatorWrite(float u);`

method. The board contains a programmable potentiometer as well, its state is returned in the range of 0–100% by the `referenceRead()` method.

We remark that APIs for Simulink and possibly MATLAB are subject to ongoing work.

# <a name="examples"/>Examples
## <a name="ident"/>System identification
Before proceeding to controller design, one shall first try to identify a simple dynamic model of the tip position
<img src="http://latex.codecogs.com/gif.latex?q(t)" border="0"/>
(m) in dependence of the servo angle
<img src="http://latex.codecogs.com/gif.latex?u(t)" border="0"/>
(deg). Let us assume that the load carried by the end-effector consists of three stacked magnets weighing 2.84 g (∼0.95 g each). Assuming the manipulated load and the beam can be represented by a single dominant resonant mode, we obtain

<img src="http://latex.codecogs.com/gif.latex?\ddot{q}(t)+2\zeta\omega\dot{q}(t)+\omega^2q(t)=f(t)," border="0"/>

where
<img src="http://latex.codecogs.com/gif.latex?\omega" border="0"/>
(rad.s<sup>−1</sup>) is natural angular frequency,
<img src="http://latex.codecogs.com/gif.latex?\zeta" border="0"/>
(-) is the damping ratio and
<img src="http://latex.codecogs.com/gif.latex?f(t)" border="0"/>
is the force acting on the system. We may represent this force by assuming
<img src="http://latex.codecogs.com/gif.latex?f(t)=c\omega^2u(t)" border="0"/>
, where
<img src="http://latex.codecogs.com/gif.latex?c" border="0"/>
(m.deg<sup>−1</sup>) is the actuator constant. After performing a Laplace transform, we arrive at the well-known transfer function

<img src="http://latex.codecogs.com/gif.latex?P(s)=\frac{Q(s)}{U(s)}=\frac{c\omega^2}{s^2+2\zeta\omega{s}+\omega^2}." border="0"/>

One shall not forget that we may only measure an acceleration signal
<img src="http://latex.codecogs.com/gif.latex?y(t)=\ddot{q}(t)" border="0"/>
, however, may still use the same model structure for grey-box identification, then compensate for the discrepancy later. The servo angle was changed 90 deg while sampling the acceleration signal at a
<img src="http://latex.codecogs.com/gif.latex?T_{\mathrm{s}}=0.003" border="0"/>
s period. The identification experiment loaded to the MCU is available in a [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/LinkShield/examples/LinkShield/LinkShield_Identification/LinkShield_Identification.ino).

Identification of the above transfer function can be performed as in the [worked MATLAB example](https://github.com/gergelytakacs/AutomationShield/blob/LinkShield/matlab/examples/LinkShield/LinkShield_Identification_TF.m). First, a data section with free vibration has been selected, then a continuous two-pole and no zero transfer function has been obtained with a ∼92% fit to the estimation data. According to this, the angular natural frequency of the system with the given load is
<img src="http://latex.codecogs.com/gif.latex?\omega = 100.4" border="0"/>
rad.s<sup>−1</sup> (16 Hz) and the damping ratio is
<img src="http://latex.codecogs.com/gif.latex?\zeta = 0.0027" border="0"/>
. Remember that the numerator assumes an acceleration signal, thus, by modeling simple harmonic motion we may safely consider the position signal to be out-of-phase with acceleration and scaled down by
<img src="http://latex.codecogs.com/gif.latex?\omega^2" border="0"/>
. The actuator constant is then
<img src="http://latex.codecogs.com/gif.latex?c =-9.2718\mathrm{E}-4" border="0"/>
m.deg<sup>−1</sup>. The comparison of the identified transfer function to test data is shown in the figure below, assuming we model the acceleration signal.

![link_identres](https://user-images.githubusercontent.com/18485913/73782175-eec5ec80-4791-11ea-9f5a-663645f6343b.png)

## <a name="control"/>Feedback control
For a start you may want to experiment with a feedback control of the beam tip, or in other words, the stabilization of the end effector of the simulated robotic arm.

The manipulation angle is maintained by a feedback loop inside the servo motor to
<img src="http://latex.codecogs.com/gif.latex?r(t)" border="0"/>
(deg). This reference can be then modified by an appropriated control input
<img src="http://latex.codecogs.com/gif.latex?u(t)" border="0"/>
, so that the resulting overall angle is
<img src="http://latex.codecogs.com/gif.latex?u_{\mathrm{r}}(t)=r(t)+u(t)" border="0"/>
. In the following example we will consider positive position feedback
control (PPF) of the dominant structural mode, where
<img src="http://latex.codecogs.com/gif.latex?q(t)" border="0"/>
(m)
is the position signal and
<img src="http://latex.codecogs.com/gif.latex?u(t)" border="0"/>
(deg) is the
resulting servo angle. The PPF controller is given in the time domain by

<img src="http://latex.codecogs.com/gif.latex?\ddot{u}(t)+2\zeta_{\mathrm{c}}\omega_{\mathrm{c}}\dot{u}(t)+\omega_{\mathrm{c}}^2u(t)=gq(t)," border="0"/>

where
<img src="http://latex.codecogs.com/gif.latex?\omega_{\mathrm{c}}" border="0"/>
[deg.m<sup>−1</sup>] is the tunable controller gain,
<img src="http://latex.codecogs.com/gif.latex?g" border="0"/>
(rad.s<sup>−1</sup>)
is the angular frequency of the controller that usually equals to the angular natural frequency of the controlled mode and
<img src="http://latex.codecogs.com/gif.latex?\zeta_{\mathrm{c}}" border="0"/>
(-) is the tunable controller damping ratio affecting the “sharpness” of the controller response.

As we are controlling a single dominant mode with a prevalent harmonic response, we may assume
that the acceleration signal recorded by the system is out-of-phase to the position with an amplitude that is
<img src="http://latex.codecogs.com/gif.latex?\omega^2=\omega^2_{\mathrm{c}}" border="0"/>
smaller. The PFF controller is then transformed to

<img src="http://latex.codecogs.com/gif.latex?\ddot{u}(t)+2\zeta_{\mathrm{c}}\omega_{\mathrm{c}}\dot{u}(t)+\omega_{\mathrm{c}}^2u(t)=-g\omega_{\mathrm{c}}^2q(t)," border="0"/>

which is effectively a negative acceleration feedback (NAF) controller. Let us not forget, that besides the orientation of the feedback, the scaling of the acceleration signal is eventually combined with the tunable gain
<img src="http://latex.codecogs.com/gif.latex?g" border="0"/>
, thus is ultimately lost in the digital realization. The transfer function of the controller is

<img src="http://latex.codecogs.com/gif.latex?G(s)=\frac{U(s)}{Q(s)}=-{g\,\omega_{\mathrm{c}}^2\,}\frac{1}{s^2+2\zeta_{\mathrm{c}}\omega_{\mathrm{c}}+\omega_{\mathrm{c}}^2}," border="0"/>

which after selecting
<img src="http://latex.codecogs.com/gif.latex?\omega_{\mathrm{c}}=\omega=100.4" border="0"/>
rad.s<sup>−1</sup>,
<img src="http://latex.codecogs.com/gif.latex?\zeta_{\mathrm{c}}=0.04" border="0"/>
(-)
, the gain
<img src="http://latex.codecogs.com/gif.latex?g=2" border="0"/>
deg.m<sup>−1</sup>
and the sampling period of
<img src="http://latex.codecogs.com/gif.latex?T_{\mathrm{s}}=0.005" border="0"/>
s results in the following discrete-time transfer function:

<img src="http://latex.codecogs.com/gif.latex?G(z)=-2\,\frac{0.1218z+0.1202}{z^2-1.719z+0.9606}." border="0"/>

The computation of the NAF/PPF controller, based on the identified single-mode vibration model, is listed in a [worked MATLAB example](https://github.com/gergelytakacs/AutomationShield/blob/LinkShield/matlab/examples/LinkShield/LinkShield_PPF.m), while the digital realization of the discrete-time transfer function is listed in a [worked C/C++ example](https://github.com/gergelytakacs/AutomationShield/blob/LinkShield/examples/LinkShield/LinkShield_PPF/LinkShield_PPF.ino) that is a part of the API.

A representative experiment is shown in the figure below, where an open-loop response is compared to the closed-loop response obtained with the NAF/PPF controller presented above. As one may observe from the experimental data, the controller damps the tip vibrations very effectively whilst ultimately converging to the desired servo angle.

![link_ctrlres](https://user-images.githubusercontent.com/18485913/73746055-ab00c200-4754-11ea-89c7-caced811c81c.png)

# <a name="hardware"/>Detailed hardware description
The LinkShield is an open hardware product, you are free to make your own device. If you come up with improvements, please let us know so we can improve our design as well. The discussion below should help you to improvise a similar setup for experimentation on a breadboard or perforation board. You may even order a professionally made PCB by a PCB fabrication service.

## <a name="circuit"/>Circuit design
The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the LinkShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Link/LinkShield_Circuit.zip). 

![PCB0](https://user-images.githubusercontent.com/18485913/73638815-66e5c280-466b-11ea-85e2-6ec545ccb175.png)

The digital micro servo motor (d), only represented by its connectors **J2**, is driven by the D9 PWM capable pin of Arduino. Its power supply is drawn directly from the board, as the current consumption remains well below the allowable maximum. A diode **D1** (e) protects the microcontroller from reverse currents caused by possible back electromotive force, while transient effects on the servo supply are filtered by a capacitor **C1** (f). To minimize the size of the accelerometer unit, the I<sup>2</sup>C pull-up resistors **R1**,**R2** (g) are included on the base board. A miniature connector **J1** (h) mounted to the shield supplies power to the acceleration sensor **U2** (v), which is connected to the I<sup>2</sup>C bus of the MCU by the SCL and SDA pins. The last component located on the base is a potentiometer **POT1** (i) connected to the A0 analog pin, including a shaft (j), that allows the user to program this input for any purpose, such as providing reference to the feedback control loop.

## <a name="parts"/>Parts
To make a LinkShield either on a PCB or on a breadboard you will need the following parts or their similar equivalents:

| Part             | Name            | Type/Value/Note                                                       | PCS |
|------------------|-----------------|-----------------------------------------------------------------------|-----|
| (d),M1   	   | Servo           | digital, high-speed metal gear micro-servo,<br/>e.g. [Savox SH-0257MG](https://www.savox-servo.com/Servos/Brushed-Motor/Savox-Servo-SH-0257MG-Digital-DC-Motor-Metal-Gear/) | 1   |
| (v),U2           | Accelerometer   | [Analog Devices ADXL345](https://www.analog.com/en/products/adxl345.html#product-overview)                          | 1   |
| (h),J1,J2        | Connector       | 0.5mm pitch, 4-lead FFC/FPC, e.g. [Molex 52745-0497](https://www.tme.eu/sk/details/mx-52745-0497/konektory-ffc-fpc-raster-0-5mm/molex/52745-0497/)    | 2   |
| (i),POT1         | Potentiometer   | 250 mW, e.g. [ACP CA14NV12,5-10KA2020](https://www.tme.eu/sk/en/details/ca14v-10k/single-turn-tht-trimmers/acp/ca14nv12-5-10ka2020/)            | 1   |
| (g),R1,R2            | Resistor        | 0805, 10kΩ, e.g. [ROYAL OHM 0805S8J0103T5E](https://www.tme.eu/sk/en/details/smd0805-10k/0805-smd-resistors/royal-ohm/0805s8j0103t5e/)       | 2   |
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

The assembled LinkShield is shown in the figure below. The mechanical base of the LinkShield is a standard two-layer 1.6mm thick printed circuit board (a) that carries all electronic and mechanical components and acts as a foundation for the device. This is connected to an Arduino R3-layout compatible microcontroller prototyping board (b) by a set of stackingheader pins (c). The servo is inserted into a prefabricated slot on the PCB and raised by 10mm using a pair of spacers (k), fixed with polyamide screws (l) from the top and nuts (m) from the bottom. The slotted cylindrical hub (n), which connects the servo shaft with the beam, was designed in Autodesk Fusion 360 and printed by a Prusa I2 MK3/S 3D printer using PETG filament in 21 minutes. The hub is held in place by a machine screw connecting the servo shaft that comes with most metal-geared servos
as standard, while the slot holding the beam is tightened and fixed by a M2×8 machine screw (o) and corresponding nut (p). The flexible cantilever beam (q) measuring 85×10×0.3mm and with a Ø2mm mounting hole placed 5mm from the edge is laser-cut from stainless steel.

The tip of the beam is equipped by the accelerometer unit (v), which is connected to the base board by a 4-lead flexible flat cable (r). The accelerometer unit is based on a single layer 0.6mm thick PCB (s) glued firmly to the beam tip. Power to the accelerometer chip is filtered by a pair of 10µF (t) and 100nF (u) capacitors. The breakout PCB contains the Analog Devices ADXL345 3-axis configurable gain digital accelerometer unit (v). 

The equivalents of the circuit components described above for the schematics are marked on the assembled device with the same letters.

![PCB2](https://user-images.githubusercontent.com/18485913/73640253-38b5b200-466e-11ea-963d-d1decbc7f1c3.png)

## <a name="pcb"/>PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Link/LinkShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/Link/LinkShield_Gerber.zip).

![PCB](https://user-images.githubusercontent.com/18485913/73640210-25a2e200-466e-11ea-8d7f-955bedabe0df.png)

# <a name="about"/>About
This shield is currently designed and created within a Bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics (IAMAI). The Institute belongs to the Faculty of Mechanical Engineering, Slovak University of Technology in Bratislava in 2017/2018.
<!-- The thesis is available [here](https://github.com/gergelytakacs/AutomationShield/wiki/pdf/xxx.pdf). -->

## <a name="authors"/>Authors
* Hardware design: Martin Vríčan, Erik Mikuláš 
* Software design: Gergely Takács
* Wiki: Martin Gulan, Gergely Takács