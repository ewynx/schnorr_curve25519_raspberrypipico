# Curve25519 on Raspberry Pi Pico

This codebase contains code to run curve25519 on a Raspberry Pi Pico. Both source code and test are taken from repo's from Adam Langley, who in turn based their code on Dan Bernstein's original implementation of curve25519. 

The test has been adjusted to fewer test loops (originally 1000, but the Pico seemed to not be able to take that) & blink the LED in order to give visual feedback. Additionally, there is output printed to the console, below are steps to get these visible. 

Source code curve25519: [repo](https://github.com/skeeto/enchive/blob/master/src/curve25519-donna.c)

Test code is based on: [repo](https://github.com/agl/curve25519-donna/blob/master/test-curve25519.c)

Resources used: 

- https://www.codalogic.com/blog/2023/01/07/Pico-Assembly-Programming
- https://www.youtube.com/watch?v=NCaL6tXAF0c
- https://www.robertthasjohn.com/post/how-to-set-up-the-raspberry-pi-pico-for-development-on-macos
- https://github.com/garyexplains/examples/tree/master/Raspberry%20Pi%20Pico/C

## Preparation

### Get started with the Raspberry Pi Pico (C)

There are various instructions to be found for this, for example the official documentation [here](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf). 

The following commands follow the steps from [this blogpost](https://www.robertthasjohn.com/post/how-to-set-up-the-raspberry-pi-pico-for-development-on-macos), which is tailored to MacOS. 

Install toolchain:
```
brew install cmake
brew tap ArmMbed/homebrew-formulae
brew install arm-none-eabi-gcc
```

Install `pico-sdk`:
```
git clone -b master https://github.com/raspberrypi/pico-sdk.git 
cd pico-sdk
git submodule update --init
```
Add the location of this sdk to you (`bash_`)`profile` for `PICO_SDK_PATH`. 

### Get Output from Raspberry Pi Pico in console

To see printed output, `minicom` could be used. 

Using brew, install with

```
brew install minicom
```

This gets started easily by using the command `minicom` in any console. In the settings (which can be changed using `minicom -s`), the correct port for the device can be used. Check what the location of the Raspberry Pi Pico is using:

```
ls -l /dev/tty.*
```

An example of what it could be: `/dev/tty.usbmodem14401`.

Note: this can change after (re)loading code on it, so make sure to double check. 

## Build & Run

With the following steps:
```
cd build
cmake ..
make
```

Now, press the BOOTSEL button on the Raspberry Pi Pico and connect to the laptop. Open the folder of the device and drag the `build/curve25519.uf2` to it. The code is now loaded onto the device.

It should automatically run the code in `curve25519.c`, which has a test in the `main` function. In this test `curve25519_donna` is called multiple times, and in between tests the LED blinks with a pause. The LED therefore gives a visual representation of whether the test code is running. Furthermore, testvalues are printed to the console - which is visible for example using `minicom` (see steps above).

## Additional info
This info is not needed to run this project, but was used to get it started. 

### Pico-project-generator

This project was created using the `pico-project-generator`:
```
git clone https://github.com/raspberrypi/pico-project-generator.git
```

#### Generate a new project with pico-project-generator

To start a new project, you can use the GUI.
```
cd pico-project-generator
export PICO_SDK_PATH="/mnt/c/Users/Gary/pico/pico-sdk"
export DISPLAY=127.0.0.1:0
./pico_project.py --gui
```

Here, enter a name and select the correct folder. The additional option selected for this project apart from the default settings is: `Console over USD`. 



