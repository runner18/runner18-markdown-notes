## Install nRF for VS Code
### Installation
- Install nRF Command line tools
	- May be asked to install Segger JLink Driver - click yes to that
- Install nRF Connect SDK
	- This is now done through the VS Code extension
- Install SDK
	- Under the VS Code extension you just installed
	- Click Manage SDKs > Install SDK
	- Tag release to install - keep in simple
### Creating an Application
- Open nRF connect extension. Here are your options:
	- Manage toolchains - install new toolchains or revert to old toolchains
	- Manage SDKs - install new SDK or revert to old SDK
	- Open an existing application
	- Create a new application
	- Create a new board
	- Browse samples
- Will "Create a new application" for this
	- Options
		- Create a blank application
		- Copy a sample
		- Browse application index
	- Select copy a sample
		- Can sort by the module (to right of "Create New App from sample")
		- Can filter by boards compatible with the sample
		- Search for the name
		- Will be copying blinky for this
	- Enter the directory where you want to create the project

### Building an Application
- Create a build configuration
	- Tells the compiler which board files to include in your build
	- This making the output compatible with your board
	- ![[Pasted image 20260910132453.png|260]]
	- Under Add Build Configuration > Board
		- Select you development kit
		- The configuration for your board is stored in a **prj.conf** file (which is generated based on your board selection)
		- Can **add extra configurations** with the "Add Fragments" button
		- Can **add arguments** with the "Add Argument" button
		- Can leave the Build directory name as "Build"
		- Can enable debugging with "Enable debug options"
	- Here's how you build
		- ![[Pasted image 20260910132813.png|235]]
	- Can also save the build configuration

### CMakeLists.txt
![[Pasted image 20260910132900.png|313]]

Place all your files you want to include under "target_sources".

## Configuring Devices
### Troubleshooting: \[property\] undeclared here
In the Zephyr device tree, you will have devices in the dts file:
![[Pasted image 20260910133120.png|198]]
The device will need to be compatible with a BINDING. 
This binding file includes all the properties this device needs to have:
![[Pasted image 20260910133153.png|163]]

![[Pasted image 20260910133209.png|460]]

When building, Zephyr will take the properties from the DeviceTree/Binding and turn them into macros:
![[Pasted image 20260910133247.png|335]]

However, in this example, notice that the spi-max-frequency macro is nowhere to be found.
This is because Zephyr is **stupid** and won't generate it.

Luckily, you can create your own custom yaml binding file.

Okay I fixed the issue, it's because there needs to be a vendor before spi-device
![[Pasted image 20260910133357.png|254]]

Yes, there's vs code errors, but it will *actually* build. [Discovered from this link]([https://devzone.nordicsemi.com/f/nordic-q-a/108613/yet-another-zephyr-devicetree-problem-this-time-with-spi-spi-max-frequency-amongst-others](https://devzone.nordicsemi.com/f/nordic-q-a/108613/yet-another-zephyr-devicetree-problem-this-time-with-spi-spi-max-frequency-amongst-others))

### Creating Your Own Bindings
Each device has its own custom binding file:
![[Pasted image 20260910133609.png|419]]
The issue is that nrf/zephyr hasn't created a binding file for every device that has existed ever.
Which means that if your device isn't on the list, you'll need to create your own binding file.

You can look at existing binding files for examples, then just alter those.

(Future Alex follow up questions)
- What *is* a device exactly? Is it a specific function on a controller? What properties of the device is the device file binding to? Where exactly am I supposed to find these example, existing binding files?
![[Pasted image 20260910133856.png|340]] ![[Pasted image 20260910133909.png|211]]

And here is that device being used in a DeviceTree file:
![[Pasted image 20260910133937.png|342]]

### Config Files

**default vs board**
There's a config file for the project, then there's a config file for the board:

Project config files:
- app_debug.conf
- prj.conf

### How to find Zephyr header files
This:
```
#include <zephyr/device.h> 
```
Is inaccurate to where device.h ACTUALLY is:

![[Pasted image 20260910134212.png|108]] ![[Pasted image 20260910134224.png|265]]

## What Does Each Part Do?
There are several files involved with Nordic Zephyr SDK, and each plays a part with connecting the code to the hardware.

### Build Configuration
Tells Nordic SDK how to build the firmware for your controller and hardware. 
Pretty comprehensive, points to a bunch of stuff. Includes the following:
- SDK version
- Toolchain version
- Board target
- Base configuration files
- Extra Kconfig fragments
- Base Devicetree overlays
- Extra Devicetree overlays
- Snippets
- Optimization level
- Extra CMake arguments
- Use sysbuild? (yes or no)

### CMakePresets.json
You can save Build Configurations as Presets.
These Presets will show up on the VS Code extension's UI.
### Configuration Files (.conf)
You can create multiple configuration files.
Configuration files contain core zephyr settings you can change.
You may also create your own, custom settings.
Settings may include:'
- Stack size
- Enable temperature sensor
- Enable CRC checking
- Enable external clock crystal
- Make UART interrupt driven
- Set logging level (warning, debug, etc.)
### Boards, Board target
You can create "boards". 
A "board" tells Nordic SDK, what features (aka "devices") the hardware has.
In theory this would let you run the same code on different boards seamlessly.

A board is made up of multiple files.

#### board.cmake
Tells Zephyr...something about how to compile the board.

#### board.yml
Contains core name info, and what processors may in included I think.

#### \_defconfig file
Contains configuration info for the board. Is SPI enabled? Is I2C enabled? Is analog-to-digital conversion enabled?

#### DeviceTree file (.dts)
This connects the code to the hardware:
- Which pins do I use for SPI communications?
- Which pin is used for this digital input/output?
- Which analog pin is used for this analog input?
 
The details of settings this up deserve its own section, really. Each "Device" type (SPI, I2C, ADC, Digital I/O) has its own quirks.
### CMakeLists.txt
This has to do with the programming language "c".
You list all the files you the linker to include in the final, compiled firmware.
You can also add conditional statements so, depending on the setting, the linker will swap out one file for another.
This lets you compile different firmware with different code more easily.
