# Introduction

The MotoShield is a didactic device proposed within the AutomationShield open-source initiative, where the goal was to create a low-cost miniaturized ready-to use experimental tool for examining the well known topic of DC motor feedback control. The device is equipped with a geared motor accompanied by a Hall effect quadrature encoder. Also, we are also able to measure the current draw of the motor, with the use of High-side shunt current measurement sensor. Although, this device was primarily designed to work with micro-controller boards from Arduino family, it is also compatible with many other boards from various manufacturers supporting AVR, SAM, and SAMD architecture. The device is mounted onto a micro-controller board by inserting the stackable headers into the board pins, thus making it modular and compact.

![image](https://user-images.githubusercontent.com/58009306/171625844-46fd8678-5da5-4d06-a8ad-d0f97a87287a.png)
***




# Application programming interface
Within the AutomationShield project, the **API** is provided for each device in order to simplify the experimental operations. In the library, you will be able to  find programming interfaces for **Arduino IDE**, **MATLAB**, and also **Simulink**. The library also contains various examples that demonstrate the features of the programming interfaces. The nomenclature of methods within the APIs is uniform.  

To initialize the device, call
```
MotoShield.begin(float);
``` 

where the input argument symbolizes the sampling period for quadrature encoder in milliseconds. The best practice is to use values between 30 and 10 milliseconds. 

To set the direction of a motor, call
```
MotoShield.setDirection(bool);
``` 
The method expects boolean input argument, where **true** sets clockwise and **false** sets the counter-clockwise direction. If direction is not specified when calling this method, the direction will be set to clockwise. _This method is not mandatory to call_, since the default direction (clockwise) is set within the initializing method.

Actuating signal is set by
```
1. MotoShield.actuatorWrite(float);
2. MotoShield.actuatorWriteVolt(float);
```
The first method expects the input value ranging from 0 to 100 %. This number represents the duty cycle of a PWM actuating signal. Whereas, the second method expects the input value ranging from 0 to 5 V. This number represents the root mean square voltage of the PWM actuation.


For measuring the angular velocity of the shaft, three methods are provided
```
1. MotoShield.sensorReadRPM(void); 
2. MotoShield.sensorReadRadian(void);
3. MotoShield.sensorRead(void);
```
The first method returns angular velocity in revolutions per minute, while the second one returns the value in radians per second. The last velocity measuring method returns the value in percentage. In order to measure velocity with the last method, it is required that you calibrate the device first.

In order to start the calibration sequence, call 
```
MotoShield.calibration(void)
```
This method does not expect nor return a value. By performing calibration, the device measures the maximum and minimum angular velocity. These values are written into internal variables. Beware, the calibration method should be executed only once in the beginning of an operation. Do not call calibration method within loop or in the middle of an operation!

To measure current, call
```
MotoShield.sensorReadCurrent(void)
```
This method returns the current value in amperes. Since the actuating signal is PWM and the motor is fairly small, expect to measure PWM alike signal. In order to approximate realistic value use digital lowpass filter, see the figure below.

![image](https://user-images.githubusercontent.com/58009306/172304047-4997e2d5-a8f3-4d28-809f-3e93e2ae0f0b.png)

## Simulink
In Simulink, the API is represented as a blockset, consisting of:
* **Reference Read block** - reading the position of potentiometer,
* **Actuator Write block** - actuation and setting direction,
* **Sensor Read block** - measuring angular velocity,
* **Sensor Read Current** block - measuring current draw,
* **MotoShield** - actuation, setting direction and measuring angular velocity.

![wikiblockset](https://user-images.githubusercontent.com/58009306/172340686-f3d8b11d-d485-4c63-96b7-ad68a8e09ab6.PNG)

All of the mentioned blocks provide the choice of input/output value units except the **MotoShield** block, where input and output is given in percentage (0 - 100) %.

# Examples

## Feedback Control

In order to understand the utility of the API when it comes to system control, it is advised to first take a look at examples that include proportional-integral-derivative (abbrev. PID) Controller. This example is available in [Arduino IDE](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/MotoShield/MotoShield_PID/MotoShield_PID.ino), [MATLAB](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/examples/MotoShield/MotoShield_PID.m) and [Simulink](https://github.com/gergelytakacs/AutomationShield/blob/master/simulink/examples/MotoShield/MotoShield_PID.slx), where the absolute PID algorithm is used. The controller was tuned with the heuristic approach. Since all of the experiments performed with **MotoShield** are repeatable and are non-destructive for the device itself, the heuristic tuning approach is acceptable. In the figure below, you can see the diagrammatic like negative feedback control loop including the PID controller created in Simulink.

![simuwiki](https://user-images.githubusercontent.com/58009306/172309884-28061b4f-67f8-456d-8289-cebb28fad97e.png)

## System Identification and model based control

In order to tune the controllers analytically, the system identification was carried out in MATLAB with the help of the System Identification Toolbox. Identification of the [Transfer Function](https://github.com/gergelytakacs/AutomationShield/blob/MotoShield_R2/matlab/examples/MotoShield/MotoShield_TransferFunction_Identification_R2.m) and [State-space](https://github.com/gergelytakacs/AutomationShield/blob/MotoShield_R2/matlab/examples/MotoShield/MotoShield_StateSpace_Identification_R2.m) grey-box models was carried out in two separate scripts. The system response experiment for system identification was done in [Arduino IDE](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/MotoShield/MotoShield_Identification/MotoShield_Identification.ino), where the data is gathered, with the use of APRBS signal generated by MATLAB [Script](https://github.com/gergelytakacs/AutomationShield/blob/master/matlab/aprbsGenerate.m).

![idwiki](https://user-images.githubusercontent.com/58009306/172313481-b817a01a-9987-4312-81c2-62f97787b09f.png)

Based on the identified transfer function model, we were able to successfully [design](https://github.com/gergelytakacs/AutomationShield/blob/MotoShield_R2/matlab/examples/MotoShield/MotoShield_Compensator_Design.m) the Lag Compensator and [implement](https://github.com/gergelytakacs/AutomationShield/blob/MotoShield_R2/simulink/examples/MotoShield/MotoShield_Compensator.slx) it, using the Simulink environment. Simulink model featuring the Lag Compensator and its impact on the system is depicted in the following figure.

![compwiki](https://user-images.githubusercontent.com/58009306/172315976-a80da498-ac96-4f57-9f2f-8dfc68a521d6.png)

Identified state-space model can be for the design of the state-space controllers. In order to validate the quality of identified state-space model, the LQ/LQI controller was [designed](https://github.com/gergelytakacs/AutomationShield/blob/MotoShield_R2/matlab/examples/MotoShield/MotoShield_LQ_Gain_Calc.m) and implemented in the Arduino IDE [example](https://github.com/gergelytakacs/AutomationShield/tree/MotoShield_R2/examples/MotoShield/MotoShield_LQ). The LQ regulator is notorious for not being able to eliminate the steady-state error, hence the integrator state was added into the optimization process. With the integrator included, we were able to achieve the desired angular velocity in relatively short time while avoiding the appearance of overshoot, which is evident seeing the following figure.

![wikilq](https://user-images.githubusercontent.com/58009306/172321201-f7af1f1f-45b0-4ef9-91c7-cd45fc9f430a.png)

# Hardware
The MotoShield is an open-source hardware designed primarily for control engineering students. Feel free to use the whole project or any part of it, and if you come up with improvements, please let us know so we can improve our design as well. You can find the full hardware documentation that is needed for making your own prototypes here.

## Schematics
The used DC Motor is equipped with a Hall quadrature encoder, mounted on backside of the Motor as a breakout board. The terminals of motor and encoder are connected to the main PCB by soldering the cables onto the six holed pads, from which four belong to the encoder and two of them are terminals of the Motor. Channels of the encoder are connected to the microcontroller via the D2 and D3 digital input pins, since these pins are usable for external interrupts on most of the Arduino boards. The motor is driven through ZXBM5210 _(U1)_, which is integrated circuit specially designed for DC Motor speed and direction control. This IC also includes a protection diode for back EMF. The current draw of the motor is measured with the use of High-side shunt current sensor INA169 _(U2)_, where the shunt resistor _(R)_ is 10 Ω and gain _(R2)_ has value of 10 kΩ. Since the maximum current draw, when motor is not under load, is around 20 mA, the gain resistor could be upscaled to 13 kΩ. Finally, the measured current signal is then fed into the OPA340 _(U3)_ operational amplifier. The purpose of operational amplifier is to buffer the incoming signal and thus nullifying the impedance of the analogue-digital converter of a microcontroller.

The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the MotoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_Circuit.rar).

![schema_R2-1-1](https://user-images.githubusercontent.com/58009306/172332551-ca6982a5-35c7-43b5-b832-f272850fc114.png)
| **Part**              | **Description**                                                              | **Mark** | **Pcs.** |
|-----------------------|------------------------------------------------------------------------------|----------|----------|
| DC Motor              | DFRobot - Geared 380:1 DC Motor w/ Hall encoder - 6V                         | U4       | 1        |
| DC Motor Driver       | DIODES Incorporated ZXBM5210 - REVERSIBLE DC MOTOR DRIVE WITH SPEED CONTROL    | U1       | 1        |
| Current Sensor        | Texas Instruments INA169 High-Side Measurement Current Shunt Monitor - SOT23 | U2       | 1        |
| Operational Amplifier | Texas Instruments OPA340 - universal operational amplifier - SOT-23          | U3       | 1        |
| Potentiometer         | 10 kΩ, 150 mW (CA9MVV 10K)                                            | POT1     | 1        |
| Shunt Resistor        | 0805, 10 Ω, 0.5 W, 1%                                                 | R        | 1        |
| Resistor              | 0805, 1 kΩ                                                            | R1       | 1        |
| Gain Resistor         | 0805, 10 kΩ, 1%                                                       | R2       | 1        |
| Capacitor             | 0805, Ceramic,  100 nF                                                       | C1, C2   | 2        |
| LED                   | 0805, Light-emitting diode, green                                            | D1       | 1        |
| PCB                   | 2 layer, FR4, thickness 1.6 mm                                               | -        | 1        |

## PCB
The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_Gerber.zip).

![wikipcb](https://user-images.githubusercontent.com/58009306/172334274-1373be8a-c11b-4a31-9444-2b3a2e87558a.png)


# About

The board was developed within the framework of a bachelor's and master's thesis at the Institute of Automation, Measurement and Applied Informatics of the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava. You may download the resulting thesis [here](https://github.com/gergelytakacs/AutomationShield/wiki/Publications).

## Authors

* Hardware design: Ján Boldocký, Tibor Konkoly, Gergely Takács
* Software design: Ján Boldocký
* Wiki: Ján Boldocký, Tibor Konkoly, Gergely Takács