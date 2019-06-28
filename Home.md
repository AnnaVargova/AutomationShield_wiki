[[/fig/header.gif|AutomationShield logo and site header.]]

AutomationShield is an open-source hardware and software project focused on tools for control engineering and mechatronics education.

The core of the project are extension modules for the popular Arduino microcontroller prototyping boards, which are  implementing feedback control experiments to teach control systems engineering and mechatronics. These hardware extensions—known in the Arduino world as shields—are then in essence experimental systems on a single printed circuit board (PCB). 

[[/fig/AS_Illustration.jpg|AutomationShield devices.]]

The didactic process for teaching control engineering is often overly focused on theory and lacks practical implementation and hands on experience. In numerous universities the didactic tools and lab equipment for control engineering and mechatronics is simply not available, since a single laboratory tool for feedback control costs in the order of tens of thousands of dollars. Even if the tools are available in the laboratory, students cannot take these home and complete assignments or experiment outside of the academic environment.

Our aim is to design feedback control experimental devices that can fit on an Arduino shield and provide an application programmer's interface (API) and examples for students, educators and researchers.

# About the Shields

Currently these shields are available:
* [MotoShield](https://github.com/gergelytakacs/AutomationShield/wiki/MotoShield) - motor speed and position feedback experiment
* [FloatShield](https://github.com/gergelytakacs/AutomationShield/wiki/FloatShield) - a floating ball feedback experiment
* [OptoShield](https://github.com/gergelytakacs/AutomationShield/wiki/OptoShield) - a low cost optical feedback experiment
* [HeatShield](https://github.com/gergelytakacs/AutomationShield/wiki/HeatShield)  - a thermal system feedback experiment
* [MagnetoShield](https://github.com/gergelytakacs/AutomationShield/wiki/MagnetoShield)  - a magnetic levitation feedback  experiment

We are in the process of developing new types of shields. We are currently working on more than 4 new experimental devices to add to this portfolio by the end of 2019 or early 2020, namely
* TugShield - an elastic structural deformation feedback experiment
* LinkShield - a rotational link feedback experiment
* VibroShield - an active vibration control experiment
* BoBShield - the classical ball-on-beam (seesaw) control experiment

## Where can I buy an AutomationShield Device?

Unfortunately we lack the infrastructure to manufacture and sell the shields. However, we include downloadable CAD files for the PCB, a list of required components and in some cases files necessary for 3D printing. A major design goal while making the boards was low-cost, simplicity and universality. We always attempted to exclude exotic mechanical or electrical components. The files necessary to produce the circuit boards are available for download, these can be sent to PCB manufacturing services and made for as low as $5 for 10 pieces. Making the shield itself is a great educational experience too!

# About the Library [![Build Status](https://travis-ci.org/gergelytakacs/AutomationShield.svg?branch=master)](https://travis-ci.org/gergelytakacs/AutomationShield)


The main role of the software part of the AutomationShield project is to provide C/C++ source code to manage inputs and outputs to the individual boards. In other words, the library is an application programming interface (API), so that students and educators can focus on feedback control design, instead of programming low-level hardware drivers. In addition to this the AutomationShield library aims to provide complete routines for implementing precise timing for control. The library contains numerous examples that implement examples in system identification an feedback control. The library contains examples written for the Arduino IDE, Matlab and Simulink. Those who do not wish to complete the hardware may still benefit from the library, as there are numerous experimental measurement that can be used for system identification tasks, and feedback control simulations based on the physical hardware.

## Supported Shields and Software
The current status of the library is as follows:

|               | Release | Beta | Arduino  | MATLAB | Simulink |
|---------------|---------|------|----------|--------|----------|
| BOBShield   |         | ✅    | ✅        |        |          |
| FloatShield   |         | ✅    | ✅        |        |          |
| HeatShield    | ✅       |      | ✅        | ✅      | ✅        |
| MagnetoShield |         | ✅    | ✅        |        |          |
| MotoShield    |         | ✅    | ✅        |        |          |
| OptoShield    | ✅       |      | ✅        |        | ✅        |

## How to Get the Library

If you are not familiar with Git, please download the latest release of the library from the [Releases](https://github.com/gergelytakacs/AutomationShield/releases) section, as the production code download does not include certain dependencies. Search for the `AutomationShield_vX.Y.tar` file in the Assets, where `vX.Y` is the major and minor version number of the release. Do not use the Source Code files in the Assets, since these lack the dependent code as well.

For those who wish to use Git, this repository contains submodules, therefore you should use `git clone --recursive git://github.com/gergelytakacs/AutomationShield.git` to get these as well. In case you have already cloned the repository, the submodule directories in `src/lib/` may be empty. In this case, you have to initialize it by calling `submodule update --init --recursive`.