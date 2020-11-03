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
The current status of the library is as follows:

|               | Release | Beta | Arduino  | MATLAB | Simulink | Python³ | LabView | Octave | Scilab |
|---------------|---------|------|----------|--------|----------| --------------| --------| ------| ------|
| BOBShield   |         | ✅    | ✅        |        |          |||||
| FloatShield   | ✅      |     | ✅        | ✅¹       | ✅         |||||
| HeatShield    | ✅       |      | ✅        | ✅¹      | ✅        |||||
| LinkShield    | ✅       |      | ✅        |   ❌¹     |         |||||
| MagnetoShield | ✅        |     | ✅        |  ❌¹     | ✅         |✅|❌¹⋅²|❌|❌ |
| MotoShield    |    ✅       |     | ✅        |    ✅      |  ✅          |||||
| OptoShield    | ✅       |      | ✅        |   ❌¹     | ✅        |||||
| PressureShield    |        |✅      | ✅        |        |         |||||
| TugShield    |        |✅      | ✅         |        |         |||||
| TempShield    |        |      |         |        |         |||||

- ❌ Can not be deployed for technical reasons.
- ¹ Serial communication only, no deployment possible.
- ² LabView LINX: lower end of loop speed is about 140 Hz (7 ms) for a LED toggle example. Tested with LabView 2020.
- ³ CyrcuitPython

## How to Get the Library

If you are not familiar with Git, please download the latest release of the library from the [Releases](https://github.com/gergelytakacs/AutomationShield/releases) section, as the production code download does not include certain dependencies. Search for the `AutomationShield_vX.Y.tar` file in the Assets, where `vX.Y` is the major and minor version number of the release. Do not use the Source Code files in the Assets, since these lack the dependent code as well.

For those who wish to use Git, this repository contains submodules, therefore you should use `git clone --recursive git://github.com/gergelytakacs/AutomationShield.git` to get these as well. In case you have already cloned the repository, the submodule directories in `src/lib/` may be empty. In this case, you have to initialize it by calling `git submodule update --init --recursive`.