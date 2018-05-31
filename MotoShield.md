# Introduction

The MotoShield was created within the framework of the AutomationShield open-source hardware and software project. The main aim of the AutomationShield project is to develop ready-to use Arduino compatible shields suitable for control engineering education. The MotoShield implements a motor equipped with gear reduction as an actuator and a hall-effect encoder feedback. The MotoShield enables one to run many well-known control engineering experiments, ranging from first-principles modeling and grey-box system identification, speed control and position control. 
[[/fig/Moto1.jpg| The Motoshield.]]   

# Library functions
Here you can find a short description of the functions, which have been created to make the work with the shield easier more efficient.

To initialize the pins used by the MotoShield, call

```
MotoShield.begin();
``` 

The next function is to set rotation direction. It has two input parameters: true or false. If you apply true, the motor will turn in counter-clockwise direction and vice versa. You can call it this function in the the `setup()' if a fixed rotation direction is desired. Alternatively it can also be called in the loop to change rotation direction at any time:
```
MotoShield.setDirection(bool);
```

The following function starts the motor at full speed
```
MotoShield.motorON();
``` 
while you can switch the motor off using
```
MotoShield.motorOFF();
``` 
Another function was created to set the motor speed as a percent value, where 0% a fully stopped motor and 100% means a motor turning at full speed. A speed lower than 30% will not turn on the motor, as it cannot overcome static electro-mechanical effects:
```
MotoShield.setMotorSpeed(speed);
``` 

The motor is running, but what if you want to change it's direction? This function reverses the pre-set direction of the motor:
```
MotoShield.revDirection();
```

The next function reads the value of the reference potentiometer and returns it in percents.
```
MotoShield.referenceRead();
``` 

The Motoshield allows you to perform certain measurements. The next function measures the voltage drop across the resistor that is used for measuring the current draw of the motor:
```
MotoShield.readVoltage();
```
This function measures the current draw of the motor (calculated based on a reference resistor and Ohm's law) and returns the current value in the units of mA:
```
MotoShield.readCurrent();
```
The following function measures the duration of a single complete revolution and returns the value in ms.
```
MotoShield.durationTime();
```

The last function for the MotoShield API measures motor speed in revolutions per minute (RPM). It has only one input parameter - the sampling time in milliseconds.
```
MotoShield.readRevolutions(int SamplingTime);
```

# Examples
## PID control
```
#include "AutomationShield.h"

unsigned long Ts = 30; // sampling time in milliseconds

bool enable=false; // flag for the sampling function

// variables for the PID

float r = 0.00;
float y = 0.00;
float u = 0.00;
float error = 0.00;
float T;


void setup() {
  
  Serial.begin(9600);
  
  MotoShield.begin(); // defining hardware pins
  MotoShield.setDirection(true); // setting the motor direction
  
  MotoShield.setMotorSpeed(100); // calibration
  delay(1000);
  
  Sampling.interruptInitialize(Ts * 1000);  // initialize the sampling function, input is the sampling time in microseconds
  Sampling.setInterruptCallback(stepEnable); // setting the interrupts, the input is the ISR function

 // setting the PID constants
 PIDAbs.setKp(3.07348889703209); // 626084.98495425
 PIDAbs.setKi(122.939555881283); // 34088513.7030457
 PIDAbs.setKd(0);  // -967.240858225792

}// end of the setup

void loop() {

  if (enable) {
    step();
    enable=false;
    
  }  

} // end of the loop


void stepEnable(){  // ISR
  enable=true;
}

void step(){ // we have to put our code here
  
  

r = MotoShield.referenceRead();  // reading the reference value of the potentiometer
T = MotoShield.readRevolutions(30);    // reading RPM of the motor

y = AutomationShield.mapFloat(T,12.00,24.81,0.00,100.00);

error = r - y; 

 u = PIDAbs.compute(error,0,100,0,100);

MotoShield.setMotorSpeed(u);

//Serial.print(T);
//Serial.print(" ");
Serial.print(r);
Serial.print(" ");
Serial.println(y);
}
```

## Step Response

```
#include "AutomationShield.h"

float Setpoint = 100;

bool enable=false;
unsigned long int curTime=0;
unsigned long int prevTime=0;

unsigned long Ts = 50;


float senzor = 0.00; // variables
float converted = 0.00;

float maximum = 25.00; // determining the boundaries of the mapFloat() function
float minimum = 10.27;


void setup() {
  
Serial.begin(9600);

Sampling.interruptInitialize(Ts * 1000);
Sampling.setInterruptCallback(stepEnable);

MotoShield.begin(); // initializing pins

MotoShield.setDirection(true);       // starting the motor
MotoShield.setMotorSpeed(Setpoint);
}

void loop() {

    curTime=millis();
  if (enable) {
    step();
    enable=false;
    
  } // end of the if statement  
} // end of the loop

void stepEnable(){
  enable=true;
}

void step(){ 
senzor = MotoShield.readRevolutions(50);  // measuring the RPM
converted = AutomationShield.mapFloat(senzor,minimum,maximum,0.00,100.00); // converting the RPM
   
   Serial.print(Setpoint);
  Serial.print(",");
  Serial.println(converted); 
}
```

# Hardware description

The MotoShield is an open-source hardware designed primarily for control engineering students. Feel free to use the whole project or any part of it, and if you come up with improvements, please let us know so we can improve our design as well. You can find the full hardware documentation that is needed for making your own prototypes here. 

## Circuit design

The main component of the shield is a 6 V brushed DC motor equipped with an encoder. The motor unit has 6 outputs, 4 of them belong to the encoder (you can find a link to the motor in the table of components). The L293D H-bridge IC is used as amotor driver. The L293D is a very handy and easy to use IC containing a four-channel H-bridge, which enables it to control two DC motors, one stepper motor or four other loads like solenoids or relays. The MotoShield also contains an LM358 operational amplifier that has two roles. First, it subtracts  two voltage values and thereby calculates the voltage drop through the resistor used for measuring current. Second, it amplifies the subtracted voltage for the A/D of the Arduino. 

The circuit schematics were designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. You may download the circuit schematics for the OptoShield from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_Circuit.rar). 
[[/fig/Moto_Schematics.png|MotoShield Circuit Schematics.]] 

## Components

|Part              | Name             | Value | PCS  | Note                       |
|------------------|------------------|-------|------|----------------------------|
| DC motor         | brushed DC motor with encoder   | -    | 1    |  [URL](https://www.dfrobot.com/product-1437.html)|
| LM358            | OPAMP            |  -    |  1   |       -                    |
| L293D            | Motor driver IC  |  -    |  1   |      -                     |
| POT1             | Potentiometer    | 10 k  | 1    | Pin size 12.5x10, [URL](https://www.tme.eu/sk/Document/a8800d4bf548c3723171950d7cc2898f/ACP_CA14-CE14.pdf)|
| R                | Resistor         | 10    |   1   |used for measuring the current draw of the motor |
| R1               | Resistor         | 1 M  | 1    |      SMD  0805          |
| R2               | Resistor         | 1 M  | 1    |      SMD  0805          |
| R3               | Resistor         | 1 M  | 1    |      SMD  0805          |
| R4               | Resistor         | 1 M  | 1    |      SMD  0805          |
| Rf               | Resistor         | 10 k  |  1  |      SMD  0805          |
|   Rg             | Resistor         |  5.1 k |  1 |      SMD 0805           |
| R8               | Resistor         | 270   | 1    |     SMD 0805           |
| D1               | LED              |       | 1    | SMD 0805, Green color  |
|                  | Header           | 10 pin| 1    | long, stackable            |
|                  | Header           | 8 pin | 2    | long, stackable            |
|                  | Header           | 6 pin | 1    | long, stackable            |
|                  |                  |       |      |                            |


## PCB layout

The printed circuit board has been designed in the Freeware version of the [DIPTrace](https://diptrace.com/) CAD software. The PCB is two-layer and fits within the customary 100 x 100 mm limit of most board manufacturers. The DIPTrace PCB layout can be downloaded [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_PCB.zip), while the ready-to-manufacture Gerber files with the NC drilling instructions are available from [here](https://github.com/gergelytakacs/AutomationShield/wiki/file/MotoShield_Gerber.zip).
[[/fig/MotoShieldFront.png|MotoShield PCB from the front.]]
[[/fig/MotoShieldBack.png|MotoShield PCB from the back.]]

# Photogallery

[[/fig/Moto2.jpg| Applied Motoshield on Arduino.]]  

# About

The board was developed within the framework of a bachelor's thesis at the Institute of Automation, Measurement and Applied Informatics of the Faculty of Mechanical Engineering (FME), Slovak University of Technology in Bratislava in 2017/2018. You may download the resulting thesis [here](https://github.com/gergelytakacs/AutomationShield/wiki/Publications)

## Authors

* Hardware design: Tibor Konkoly, Gergely Takács
* Software design: Tibor Konkoly
* Wiki: Tibor Konkoly, Gergely Takács