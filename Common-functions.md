# Mathematics

## Map floating point numbers

The `map()` function as a part of the Arduino package is for integer numbers, its use with floating point arithmetic can be unpredictable. To linearly map a floating point number `value` from a given input range to a given output range, you should use  
```
AutomationShield.mapFloat(value, fromLow, fromHigh, toLow, toHigh);
```
where `fromLow` is the start of the input range and `fromHigh` the end, while `toLow` is the start of the output range and `toHigh` is its end.

## Constrain floating point numbers

Lorem ipusm