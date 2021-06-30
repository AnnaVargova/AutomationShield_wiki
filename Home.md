[[/fig/header.gif|AutomationShield logo and site header.]]

AutomationShield is an open-source hardware and software initiative focused on creating tools for control engineering and mechatronics education.

The core of the project are reference designs of extension modules for the popular Arduino microcontroller prototyping boards, which are implementing feedback control experiments to teach control systems engineering and mechatronics. These hardware extensions—known in the Arduino world as shields—are then in essence experimental systems on a single printed circuit board (PCB). 

[[/fig/AS_Illustration.jpg|AutomationShield devices.]]

The didactic process for teaching control engineering is often overly focused on theory and lacks practical implementation and hands on experience. In numerous universities the didactic tools and lab equipment for control engineering and mechatronics is simply not available, since a single laboratory tool for feedback control costs in the order of tens of thousands of dollars. Even if the tools are available in the laboratory, students cannot take these home and complete assignments or experiment outside of the academic environment.

Our aim is to design feedback control experimental devices that can fit on an Arduino shield and provide an application programmer's interface (API) and examples for students, educators and researchers.

# About the Shields

Currently the reference designs of these shields are available:
* [MagnetoShield](https://github.com/gergelytakacs/AutomationShield/wiki/MagnetoShield)  - a magnetic levitation feedback  experiment
* [FloatShield](https://github.com/gergelytakacs/AutomationShield/wiki/FloatShield) - a floating ball feedback experiment
* [HeatShield](https://github.com/gergelytakacs/AutomationShield/wiki/HeatShield)  - a thermal system feedback experiment
* [MotoShield](https://github.com/gergelytakacs/AutomationShield/wiki/MotoShield) - motor speed and position feedback experiment
* [OptoShield](https://github.com/gergelytakacs/AutomationShield/wiki/OptoShield) - a low cost optical feedback experiment
* [LinkShield](https://github.com/gergelytakacs/AutomationShield/wiki/LinkShield) - a rotational link feedback experiment

We are in the process of developing new types of shields. We are currently working on 3 new experimental devices to add to this portfolio in 2020-2021, namely

* [BoBShield](https://github.com/gergelytakacs/AutomationShield/wiki/BoBShield) - the classical ball-on-beam (seesaw) control experiment
* [BlowShield](https://github.com/gergelytakacs/AutomationShield/wiki/BlowShield) - a pressure control experiment
* [TempShield](https://github.com/gergelytakacs/AutomationShield/wiki/TMShield) - a device to teach temperature metrology concepts
* TugShield - an elastic structural deformation feedback experiment

## Where can I buy an AutomationShield Device?

Unfortunately we lack the infrastructure to manufacture and sell the shields, also, this is a non-commercial initiative. However, we include downloadable CAD files for the PCB, a list of required components and in some cases files necessary for 3D printing. A major design goal while making the boards was low-cost, simplicity and universality. We always attempt to exclude exotic mechanical or electrical components. The files necessary to produce the circuit boards are available for download, these can be sent to PCB manufacturing services and made for as low as $5 for 10 pieces. Making the shield itself is a great educational experience too!

# About the Library [![Build Status](https://travis-ci.org/gergelytakacs/AutomationShield.svg?branch=master)](https://travis-ci.org/gergelytakacs/AutomationShield) [![CodeFactor](https://www.codefactor.io/repository/github/gergelytakacs/automationshield/badge)](https://www.codefactor.io/repository/github/gergelytakacs/automationshield) [![Codacy Badge](https://api.codacy.com/project/badge/Grade/bae54207cca24ef2929c38b87e279764)](https://app.codacy.com/app/gergelytakacs/AutomationShield?utm_source=github.com&utm_medium=referral&utm_content=gergelytakacs/AutomationShield&utm_campaign=Badge_Grade_Dashboard) [![DOI](https://zenodo.org/badge/126338636.svg)](https://zenodo.org/badge/latestdoi/126338636)


The main role of the software part of the AutomationShield project is to provide C/C++ source code to manage inputs and outputs to the individual boards. In other words, the library is an application programming interface (API), so that students and educators can focus on feedback control design, instead of programming low-level hardware drivers. In addition to this, the AutomationShield library aims to provide complete routines for implementing precise timing for control. The library contains numerous examples that implement examples in system identification an feedback control. The library contains examples written for the Arduino IDE, MATLAB and Simulink. Those who do not wish to complete the hardware may still benefit from the library, as there are numerous experimental measurements that can be used for system identification tasks and feedback control simulations based on the physical hardware.

## Supported Shields and Software

The library currently supports the following AutomationShield hardware in their respective release (R) versions:
|                | Release | Beta | Hardware  | 
|----------------|---------|-------|----------|
| MagnetoShield  | ✅      |       | R4      | 
| FloatShield    | ✅      |       | R4      | 
| BOBShield      | ✅      |       | R2      |  
| LinkShield     | ✅      |       | R1      | 
| MotoShield     | ✅      |       | R1      |
| PressureShield | ✅      |       | R2      | 
| HeatShield     | ✅      |       | R1      | 
| OptoShield     | ✅      |       | R1      | 
| TurboShield    |         |        |❌¹     |        
| TugShield      |         |✅     | R1      | 
| TempShield     |         |❌²    | -³        | 
- ¹ Under development
- ² Not available
- ³ Hardware has not been realized yet.

The current status of the library for individual API is as follows:

|                |  Arduino  | MATLAB | Simulink | Python³ | LabView | Octave | Scilab |
|----------------|-----------|--------|----------| --------| --------| -------| -------|
| MagnetoShield  | ✅        | ❌¹   | ✅       |✅      |❌¹⋅²    |❌¹     |❌¹   |
| FloatShield    | ✅        | ✅¹⋅⁴   | ✅      |||||
| BOBShield      | ✅        |        | ✅       |||||
| LinkShield     | ✅        | ❌¹   |            |||||
| MotoShield     | ✅        | ✅¹   |  ✅         |||||
| PressureShield | ✅        | ✅    | ✅          |||||
| HeatShield     | ✅        | ✅¹   | ✅          |||||
| OptoShield     | ✅        | ❌¹   | ✅          |||||
| TurboShield    | ✅        |       |              |||||
| TugShield      | ✅        | ✅    | ✅          |||||
| TempShield     |            |       |             |||||

- ❌ Can not be deployed for technical reasons.
- ¹ Serial communication only, no deployment possible.
- ² LabView LINX: lower end of loop speed is about 140 Hz (7 ms) for a LED toggle example. Tested with LabView 2020.
- ³ CyrcuitPython
- ⁴ Works for FloatShield R1-R3. Currently under development for R4 - it is likely to be implementable.

The library implements the following feedback control concepts in real-time hardware examples in at least one of the API for at least one supported prototyping board:

|                | Model¹  | Identification² | PID³ |  LQ⁴  | MPC⁵  | PP⁶  |Kalman⁷ | Luenberger⁸ |
|----------------|---------|-----------------|------|-------|-------|------|--------|-------------|
| MagnetoShield  |✅       |✅              |✅    |✅    |✅    |✅    | ✅     | ✅         |
| FloatShield    |✅       |✅              |✅    |✅    |✅    |      | ✅     |             |
| BOBShield      |✅       |✅              |✅    |✅    |       |      |        |             |
| LinkShield     |✅       |✅              |✅ᵝ   |       |      |       |        |             |
| MotoShield     |✅       |✅              |✅    |       |      |       |        |             | 
| PressureShield |          |✅              |✅    |✅ᵞ    |      |       |        |             |
| HeatShield     | ✅      |✅              |✅    |       |      |       |        |             |
| OptoShield     |          |✅             |✅    |       |      |       |        |             |
| TurboShield    |          |                |       |       |      |       |        |             |             
| TugShield      |       
| TempShield     |          |                |       |       |      |       |        |             |    


- ¹ Mathemaical-physical analysis of the system dynamics
- ² Experimental system identification
- ³ Proportional-Integral-Derivative (PID) control
- ⁴ Linear Quadratic (LQ) control
- ⁵ Model Predictive Control 
- ⁶ Pole placement
- ⁷ Kalman filtering
- ⁸ Luenenberger observer
- ᵝ According to the Positive Position/Velocity Feedback variant
- ᵞ Contains bugs

## How to Get the Library

If you are not familiar with Git, please download the latest release of the library from the [Releases](https://github.com/gergelytakacs/AutomationShield/releases) section, as the production code download does not include certain dependencies. Search for the `AutomationShield_vX.Y.tar` file in the Assets, where `vX.Y` is the major and minor version number of the release. Do not use the Source Code files in the Assets, since these lack the dependent code as well.

For those who wish to use Git, this repository contains submodules, therefore you should use `git clone --recursive git://github.com/gergelytakacs/AutomationShield.git` to get these as well. In case you have already cloned the repository, the submodule directories in `src/lib/` may be empty. In this case, you have to initialize it by calling `git submodule update --init --recursive`.

### Arduino IDE Quick Start
- In case you have cloned the repository in Git, please create a Zip archive, then open the Arduino IDE and read the archive `Tools > Include Library > Add .ZIP Library`.
- You may browse for examples in `File > Examples > AutomationShield`

### Simulink Quick Start
- Download and install MATLAB / Simulink R2019a or newer. Be sure to install Simulink Coder and [Embedded Coder](https://www.mathworks.com/products/embedded-coder.html) and DSP System Toolbox features along with the base installation. In case your current installation does not support these features, go to  `Add-Ons > Get Add Ons` and find the missing packages.
- Install [Simulink Support Package for Arduino Hardware](https://www.mathworks.com/matlabcentral/fileexchange/40312-simulink-support-package-for-arduino-hardware). On the `Home` tab find `Add-Ons > Get Hardware Support Packages` then find the Simulink Support Package for Arduino Hardware. Click `Install` and follow the usual installation procedure. Installing a support package requires a MathWorks account.

Tips:
- Should you run into the `rtiostream.h` missing error, follow [this](https://www.mathworks.com/matlabcentral/answers/343237-fatal-error-rtiostream_utils-h-no-such-file-or-directory) procedure.