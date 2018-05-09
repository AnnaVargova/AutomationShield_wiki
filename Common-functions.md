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

```
#include "AutomationShield.h"

bool stepEnable=false;

void setup() {
  
  Serial.begin(9600);
  Timer.interruptInitialize(1000000);
  Timer.setInterruptCallback(stepTimer);
}

void loop() {

    if (stepEnable) {
    Serial.println(“Hello world“);
    stepEnable=false;
    }  
}

void stepTimer(){
  stepEnable=true;
}
```

## PID


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

