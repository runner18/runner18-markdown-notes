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
	- ![](Pasted%20image%2020260910154529.png)
	- Under Add Build Configuration > Board
		- Select you development kit
		- The configuration for your board is stored in a **prj.conf** file (which is generated based on your board selection)
		- Can **add extra configurations** with the "Add Fragments" button
		- Can **add arguments** with the "Add Argument" button
		- Can leave the Build directory name as "Build"
		- Can enable debugging with "Enable debug options"
	- Here's how you build
		- ![](Pasted%20image%2020260910154532.png)
	- Can also save the build configuration

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
CMakePresets.json contains a list of the presets.

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

The .conf file listed last in a build configuration gets top priority. Will override the others.

### KConfig
Declares settings and their types

### Boards, Board target
You can create "boards", which tells Nordic SDK what the hardware looks like.
The selling point here is that you could in theory run the same firmware on different hardware. 

Made up of multiple parts:
- board.cmake - set up flashing and debugging
- board.yml - contains core name info
- \_defconfig file - core config info for the board: Is SPI enabled? etc., sets values for Kconfig settings
- DeviceTree file (.dts) - connects the code to the hardware: which pins do I use for SPI? etc.
- Kconfig - declares settings and their types
 
The DeviceTree file deserves its own section, honestly.
### CMakeLists.txt
CMake is a build planner for programming languages.
1. CMake plans
2. The compiler turns each .c into a piece
3. The linker joins the pieces

CMakeLists.txt lets you decide which files will get compiled.

### west . command
Runs the whole chain: finds the board, merges the settings, calls CMake, calls the compiler.

## DeviceTree 
The DeviceTree is an essential part of a "Board".

The DeviceTree connects the code to the hardware: 
- which pins do I use for this Digital Out? 
- What frequency are we running SPI at and which pins? etc.

Each piece of hardware (SPI, I2C, I/O, etc.) is represented as a "Node".