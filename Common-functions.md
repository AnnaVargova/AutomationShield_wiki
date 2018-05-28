# Control Engineering

## Sampling

Library Sampling.h is a part of the AutomationShield.h library.
### Simplified description of Sampling method
`void interruptInitialize(unsigned long microseconds)`

You must call this method to specify the sampling period in microseconds.

`void setInterruptCallback(void (*interruptCallback)())`

You must call this method to specify the function, which will be executed when interrupt occur.

`float getSamplingPeriod()`

This method return sampling period in seconds.

### Example
Here is the [Link](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/Sampling/Sampling.ino).

## PID

Automationshield.h contains two PID versions. If we want to calculate an action by using an incremental form of equation,  we call the `PIDInc` object that is the instance of the `PIDIncClass` class. If we want to calculate the action by using an equation in absolute form, then we must call the `PIDAbs` object, which is an instance of the `PIDAbsClass` class.

This method accepts a control deviation as an input parameter and returns the action.
```
float compute (float err)
```
This method accepts two parameters: a control deviation and the boundaries that can be acquired. It returns an action within the given boundaries.
```
float compute (float err, float saturationMin, float saturationMax)
```
this method accepts as a parameter the regulatory deviation, the boundaries of which the intervention can be acquired and the boundaries that the integrating member can acquire. You are returning an action hit within the given boundaries. This method can only be called on the `PIDAbs` object.
```
float compute (float err, float saturationMin, float saturationMax, float antiWindupMin, float antiWindupMax)
```
### Example

Feel free to use our example [link](https://github.com/gergelytakacs/AutomationShield/blob/master/examples/FloatShield_PID/FloatShield_PID.ino).

# Mathematics

## Map floating point numbers

The `map()` function as a part of the Arduino package is for integer numbers, its use with floating point arithmetic can be unpredictable. To linearly map a floating point number `value` from a given input range to a given output range, you should use  
```
AutomationShield.mapFloat(value, fromLow, fromHigh, toLow, toHigh);
```
where `fromLow` is the start of the input range and `fromHigh` the end, while `toLow` is the start of the output range and `toHigh` is its end.

## Constrain floating point numbers

Lorem ipusm

# Other functions

## Error handling
Calling the error function with an error message in the string
```
AutomationShield.error("String");
````
will stop all activity, letting you know that something definitely is not working. The error reporting function by default also lights up the on-board LED on pin 13 to let you know that something is not working correctly. To change this, use
```
#undef ERRORPIN
#define ERRORPIN YOURPIN
```
where `YOURPIN` is the new pin to send a logical high signal upon calling the error handling function. 

The error handling function prints the message in `"String"` by default at the time the function is called to make debugging easier. If you wish to turn printing messages to the serial line off, call
```
#undef ECHO_TO_SERIAL
#define ECHO_TO_SERIAL 0
```
which will still cause the system to stop upon calling the error handling, but will not print the exact messages to the serial port.

