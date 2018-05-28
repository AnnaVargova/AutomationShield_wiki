# Introduction

Motoshield was created within the open-source project called AutomationShield, which's main aim is to develop Arduino compatible shields suitable for control engineering education. The main component of the shield is a 6V brushed DC motor equipped with an encoder. The motor unit has 6 outputs, 4 of them are belonging to the encoder (you can find a link about the used motor in the table of used components). The L293D IC performs the motor control. L293D is a very handy and easy to use IC containing a four-channel H-bridge, which makes it able to control two DC motors, a stepper motor or four other loads like solenoid or relay. Motoshield also contains an LM358 OPAMP, which performs two operations. First, it subtracts  two values of the voltage (calculates the voltage drop through the resistor used for measuring current) and as a second operation amplifies the subtracted voltage. The gain of the OPAMP can be set changing the values of the used resistors. The fourth component of the board is a potentiometer. It's purpose is to set the reference values at speed control. The shield is suitable to perform, among other things, PID control of the motor's speed or making step response experiments (see examples below). 

[[/fig/Moto1.jpeg| The Motoshield.]]   
[[/fig/Moto2.jpeg| Applied Motoshield on Arduino.]]  
[[/fig/Opto_Schematics.png|OptoShield Circuit Schematics.]]  

# Library functions
Here you can find a short description of the functions, which were written for making the work with the Motoshield more easier and efficient.

You have to call this function in the setup, it initializes the pins used by the components.

```
MotoShield.begin();
``` 

The next function is for setting the direction of the rotation. It has two input parameters - true or false. If you apply true, the motor will turn in counter-clockwise direction and vice versa. You can call it in the setup - if the constant direction suits to you, or it can be called in the loop too and the direction can be changed later.  

```
MotoShield.setDirection(true/false);
```
If the direction is set, only one thing is remaining to start the motor - setting it's speed. You can do it using different functions. First of these function switches on the motor at full speed. 

```
MotoShield.motorON();
``` 

You can switch off the motor using the next function:

```
MotoShield.motorOFF();
``` 
Another function was created to set the motor's speed. It has one input parameter - the speed in percents. Please, do not apply lower speed than 30%.

```
MotoShield.setMotorSpeed(speed in %);
``` 

Now the motor is running, but what if you want to change it's direction? This function reverses the pre-set direction of the motor.

```
MotoShield.revDirection();
```
The next function reads the value of the potentiometer and returns it in percents.

```
MotoShield.referenceRead();
``` 
The Motoshield allows you to perform some measurements, like voltage, current or speed. The next function measures the voltage drop across the resistor used for measuring the current draw of the motor. 

```
MotoShield.readVoltage();
```

This function measures the current draw of the motor and returns the current value in mA. 

```
MotoShield.readCurrent();
```
This function measures the duration of one revolution and returns the value in ms.

```
MotoShield.durationTime();
```

The last function measures the motor speed in revolutions per minute (RPM). It has only one input parameter - the sampling time (used by the Sampling() function) in milliseconds.

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

Motoshield is an open-source hardware designed primarily for control engineering students. Feel free to use the whole project or any part of it, and if you come up with improvements, please let us know so we can improve our design as well. You can find the documentation, needed for making your own prototypes, below. 

To be continued...