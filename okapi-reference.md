# Okapi Reference

Okapi is a library for PROS that is intended to ease development through wrapper and convenience functions.

The [okapi docs](https://pros.cs.purdue.edu/v5/okapi/index.html) provides a more comprehensive reference -- this is just for our commonly-used functions.

For a broader tutorial, see [Getting Started with OkapiLib](https://pros.cs.purdue.edu/v5/okapi/tutorials/walkthrough/getting-started.html)

## Enable

Uncomment the line `//#include "okapi/api.hpp"` in `include/main.h` so that it reads `#include "okapi/api.hpp"`.

We will use the line `using namespace okapi;` to automatically namespace calls for further convenience.

## Driving

To initialize the drive, use

    ChassisControllerIntegrated drive = ChassisControllerFactory::create(
      <Left Motor>, <Right Motor>,
      <Gearset>,
      {<Wheel size>, <Wheel base size (distance between wheels)>}
    );

For example, use

    ChassisControllerIntegrated drive = ChassisControllerFactory::create(
      -10, 1,
      AbstractMotor::gearset::green,
      {4_in, 11.5_in}
    );

## Joystick Controls

For button controls, initialize them with

    ControllerButton buttonLift(ControllerDigital::left);
    ControllerButton buttonRails(ControllerDigital::R1);

This creates two buttons one named `buttonLift` and one named `buttonRails`. The first button is bound to the left arrow, and the second button is bound to the R1 button.

To test for when they're pressed, use `.changedToPressed()`. For example, to do something when a button is pressed, use

    if (buttonLift.changedToPressed) {
      // do something
      moveLift()
    }

For joystick controls, initialize with

    Controller masterController;

To get the raw value of a joystick between `-1.0` and `1.0`, use

    float joystickValue = masterController.getAnalog(controllerAnalog::leftY)

For driving (tank drive) use

    drive.tank(
  		masterController.getAnalog(ControllerAnalog::leftY),
  		masterController.getAnalog(ControllerAnalog::rightY)
  	);

or arcade drive is

    drive.arcade(
      masterController.getAnalog(ControllerAnalog::leftY),
      masterController.getAnalog(ControllerAnalog::rightY)
    );

### Partner Joystick

The initializers for `ControllerButton` and `Controller` assume master joystick by default. If you want to use the partner joystick, pass in `ControllerId::partner` as the first argument, for example to initialize a partner ControllerButton:

    ControllerButton buttonLift(ControllerId::partner, ControllerDigital::left)

Or to initialize a partnerController for analog joystick values:

    Controller partnerController(ControllerId::partner)
