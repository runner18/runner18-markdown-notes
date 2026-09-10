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
	- ![[Pasted image 20260910132453.png|373]]
	- Under Add Build Configuration > Board
		- Select you development kit
		- The configuration for your board is stored in a **prj.conf** file (which is generated based on your board selection)
		- Can **add extra configurations** with the "Add Fragments" button
		- Can **add arguments** with the "Add Argument" button
		- Can leave the Build directory name as "Build"
		- Can enable debugging with "Enable debug options"
	- Here's how you build
		- ![[Pasted image 20260910132813.png|381]]
	- Can also save the build configuration

### Header Files
### CMakeLists.txt
![[Pasted image 20260910132900.png|505]]

Place all your files you want to include under "target_sources".