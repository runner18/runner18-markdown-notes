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
	- ![394](Pasted%20image%2020260910154529.png)
	- Under Add Build Configuration > Board
		- Select you development kit
		- The configuration for your board is stored in a **prj.conf** file (which is generated based on your board selection)
		- Can **add extra configurations** with the "Add Fragments" button
		- Can **add arguments** with the "Add Argument" button
		- Can leave the Build directory name as "Build"
		- Can enable debugging with "Enable debug options"
	- Here's how you build
		- ![406](Pasted%20image%2020260910154532.png)
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
### Troubleshooting: \[property] undeclared here
In the Zephyr device tree, you will have devices in the dts  file:
```
&spi{
	compatible = "nordic,nrf-spim";
	status="okay";
	pinctrl-0=<&spi_default>;
	pinctrl-1=<&spi1_sleep>;
	pinctrl-names="default","sleep"
	cs-gpios = <&gpio1 5 GPIO_ACTIVE...

	spicdlcdtest:spicdlcd@1{
		status="okay";
		compatible="spi-device";
		reg=<0x1>;
		spi-max-frequency=<125000>;
	};
};
```
This device will need to be compatible with a BINDING. This binding file includes all the properties this device needs to have:

spi-device.yaml
```
include: [base.yaml, power.yaml]

on-bus:spi

properties:
	reg:...
	spi-max-frequency:...
	duplex:...
	frame-format:...
	spi-cpol:...
	spi-cpha:...
	spi-hold-cs:...
```

error output:
```
error: 'DT_N_S_soc_S_spi_40004000_S_spicdlcd_1_P_spi_max_frequency' undeclared here (not in a function); did you mean 'DT_N_S_soc_S_spi_40004000_spicdlcd_1_O_reg_IDX_0_EXISTS'?

10998 | #define DT_N_ALIAS_spicdlcdtest DT_N_S_soc_S_spi_40004000_S_spicdlcd_1

```

When building, Zephyr will take the properties from the devicetree/binding and turn them into macros:

```c
#define DT_N_ALIAS_spicdlcdtest DT_N_S_soc_S_spi_40004000_S_spicdlcd_1
#define DT_N_INST_0_spi_device    DT_N_S_soc_s_spi_40004000_S_spicdlcd_1
```

However, in this example, notice that the spi-max-frequency macro is nowhere to be found. This is because Zephyr is stupid and won't generate it.

Luckily, you can create your own custom yaml binding file.

Okay, I fixed the issue, it's becase there needs to be a vendor before spi-device:
```
spicdlcdtest:spicdlcd@1{
	status="okay";
	compatible="vnd,spi-device";
	reg=<0x1>
	spi-max-frequency=<125000>;
};
```
Yes, there's vs code errors, but it will actually build. Discovered from this link:
[link](https://devzone.nordicsemi.com/f/nordic-q-a/108613/yet-another-zephyr-devicetree-problem-this-time-with-spi-spi-max-frequency-amongst-others)

### Troubleshooting: has x strings,  expected y  strings
```
pinctrl-0=<&spi1_default>;
pinctrl-1=<&spi1_sleep>;
pinctrl-names="default","sleep";
```
Make  sure these all  match  up.
## I2C
### Troubleshooting: ord undeclared here
```
error: '__device_dts_org_DT_N_NODELABEL_i2c_fram_BUS_ORD' undeclared here (not in a  function)
```

```
i2c_fram: fram2s0 {
	compatible = "ramtron, fram";
	reg = <0x50>;
}
```
Make sure the node label matches how it's referenced in your code.

## Misc. Troubleshooting
### Storage class specified for parameter
Changes are you've forgotten a semi colon in a header file someplace. Make sure each line ends in ;

### Conflicting types for - have
Make sure your function's parameters are a proper class and  that the  class name is  spelled correctly

### Logging errors
```
error: '__log_current_const_data' undeclared (first use in  this  function); did you mean 'log_source_const_data'?
```
If you have errors like these, it's probably because you forgot to do this:
``` c
LOG_MODULE_REGISTER(Main, LOG_LEVEL_DBG);
```

### No SOURCES given to target: app
Make sure that the included modules in cmakelists.txt are spelled correctly