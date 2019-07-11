# pros-notes
Collection of notes on PROS v5 for our reference

## General PROS

### Build / Upload Process
Before being uploaded to the Brain, the code needs to be compiled using

    pros make

This converts the code to machine code, which can then be uploaded with

    pros upload

To view code response and print statements, run

    pros terminal

This only works when your computer is connected directly to the Brain -- not wirelessly.

### Enable Wireless Upload

Change the line `USE_PACKAGE:=0` to  `USE_PACKAGE:=1` inside `Makefile` to enable Hot/Cold linking and Wireless Upload.

This does two things:

1. It splits the compiled code into two sections -- the "hot" code that you write and the "cold" code which is the PROS library. Because the cold code never changes, it never needs to be re-uploaded, reducing the time it takes to upload to the Brain.

2. It enables wireless uploading. The first time you upload, it needs to upload both the hot code and the cold code, so this upload takes longer (around 2 minutes). In successive uploads, it only needs to upload the hot code, so there is very little to upload, and it takes only a few seconds.

## Okapi

Okapi is a library for PROS that is intended to ease development through wrapper and convenience functions.

### Enable

Uncomment the line `//#include "okapi/api.hpp"` in `include/main.h` so that it reads `#include "okapi/api.hpp"`.

We will use the line `using namespace okapi;` to automatically namespace calls for further convenience.
