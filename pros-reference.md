# General PROS Reference

The [PROS docs](https://pros.cs.purdue.edu/v5/index.html) provides a more comprehensive reference and installation instructions -- this is just for our commonly-used functions.

## Build / Upload Process
Before being uploaded to the Brain, the code needs to be compiled using

    pros make

This converts the code to machine code, which can then be uploaded with

    pros upload

To view code response and print statements, run

    pros terminal

This only works when your computer is connected directly to the Brain -- not wirelessly.

## Enable Wireless Upload

Change the line `USE_PACKAGE:=0` to  `USE_PACKAGE:=1` inside `Makefile` to enable Hot/Cold linking and Wireless Upload.

This does two things:

1. It splits the compiled code into two sections -- the "hot" code that you write and the "cold" code which is the PROS library. Because the cold code never changes, it never needs to be re-uploaded, reducing the time it takes to upload to the Brain.

2. It enables wireless uploading. The first time you upload, it needs to upload both the hot code and the cold code, so this upload takes longer (around 2 minutes). In successive uploads, it only needs to upload the hot code, so there is very little to upload, and it takes only a few seconds.

## PROS Variables

Unlike in Python and Javascript, in C++ you need to specify the types of variables, for example

    int height = 5;

To reference lengths, PROS provides handy [units](https://pros.cs.purdue.edu/v5/okapi/api/units/index.html). For example, type

    QLength height = 10_in;
    QLength height2 = 0.3_m;
    QLength height3 = 0.5_m - 10_in*2;

The main issue I've come across is ratios of lengths, such as used when computing angles from two lengths. For example

    double angle = asin(1_in / 2_in);

does not work because the ratio of two `QLength`s is an `RQuantity`, not a `double` -- it's the wrong type to take the inverse sin (`asin`) of. To get the double value, use `.getValue()`, for example

    double angle = asin((1_in / 2_in).getValue());

This ratio (inches over inches) is dimensionless, so it works intuitively: `(1_in / 2_in).getValue() == 0.5`. For other quantities, `.getValue()` returns the value in SI base units. For example, `(1_cm).getValue() == 0.01` because 1 cm is 0.01 meters -- meters is the SI base unit.

This comes into play when logging values. For example, if you try to log a `QLength`, you have to convert it to a double first to print it to terminal.

    QLength height = 10_in;
    printf("The arm is at %f m\n", height.getValue());

This code will print `"The arm is at 0.254 m"` because 10 inches is 0.254 m.

## Sharing Variables Across Files

You may want to initialize the chassis controller (or some other variable) only once in `initialize.cpp`, then reference it in `autonomous.cpp` and `opcontrol.cpp`. How will you accomplish that? It takes a bit of work.

First, you need to initialize the variable as you would normally do in one file, such as `initialize.cpp`

    ChassisControllerIntegrated drive = ...
    
This declares the variable, but then you have to share it with the other files

You will need a single file such as `common.hpp` which will be shared between the `.cpp` files where you want to reference the variable. Inside the `common.hpp` file, you need a line such as

    extern ChassisControllerIntegrated drive;
    
Note the use of the keyword `extern` -- this specifies that the variable `drive` comes from another file. You need to manually specify the type `ChassisControllerIntegrated`; merely using `auto` does not work.

In all the files where you want to reference the variable, use the line

    #include "common.hpp"
    
in order to use the shared variable. Now you can reference `drive` as any other variable in those files.

### Note for constants

Sharing a constant value across files is slightly different: You need to specify `extern` in both `common.hpp` and the file where you reference it.

For example, in `common.hpp`:

    extern const QLength armLength;
    
and in `lift.cpp`:

    extern const QLength armLength = 20_in;

## Configuration Arrays

To create presets, I suggest using an array, for example

    const QLength liftHeights[] = {1_in, 18.5_in, 24.5_in, 38.0_in};

This specifies an array of `QLength`s. To use this, access array elements with `liftHeights[index]` such as `liftHeights[1]` to get `18.5_in`.
