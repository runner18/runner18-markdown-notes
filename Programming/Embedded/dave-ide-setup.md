## Workspaces and Projects
DAVE IDE uses Eclipse. Eclipse has Workspace folders and Project folders. 
The Project where the actual code is, Workspaces just point to Projects.

Workspace folders have a .metadata folder inside, which tells is where its projects are.
```
Workspace --> .metadata/ --> .plugins/ --> org.eclipse.core.resources/
```

Project folders do not have to be inside a Workspace folder, but I would recommend it.

OPTION 1: Project folder(s) **outside** Workspace folder
```
some-parent/
	├─ dave-workspace/
	|   └─ .metadata/
	└─ my-project/
```

OPTION 2: Project folder(s) **inside** Workspace folder
```
dave-workspace/
	└─ project-A/
	└─ project-B/
	└─ .metadata/
```

```
└ 192
─ 196
├ 195
```

## Set Up Projects
### Create a new Project
File
New
Dave Project . . . 
Infineon Projects/ARM-GCC Application/DAVE CE Project

### Add Existing Projects
File
Open Project from File System . . .
Import Source
Directory . . .

### Microcontroller Selection
![](Pasted%20image%2020260910154357.png)
If your board is 1302, for example:
- pick from the XMC1000 dropdown
- then the XMC1300 series dropdown

### Set Active Project
Right-click on a project, then click "Set Active Project"

### Set Up Debugger
![](Pasted%20image%2020260910154409.png)

![](Pasted%20image%2020260910154418.png)

![](Pasted%20image%2020260910154424.png)

### Build Configurations
To set the Build Configuration:
- Right click on project folder
- Build Configurations
- Set Active
- Select "Debug" or "Release"

To create debug configurations:
- Set the Active Build Config to "Debug" or "Release"
- 

## Set up ADC_MEASUREMENT
Instead, add the ADC_MEASUREMENT DAVE APP
Click ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAUCAYAAAB4d5a9AAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAEQSURBVEhLY/z09c9/BhoDxk9fftPcEiaQDbTGjB8+4/eJwHJnKIuB4UPkXiiLNMAEpWkKmP4D/YEPIwNs8sRgoE+AJF6MDLDJE8aM7z7+ArHgQGilC5RFGLwL3wNl4QcYqYsUgK4XF2bCECEFoOvFgRnfvEcNLnQgshoRfDP/R0NZuEFwWCKUhQCMr9//xGuJ6GpXKAvoqNSDUBZ2MGvWLIagsAQoDwEwgwsdYwGO7Qew0mCAxQyCxQo2sL/SASsNAtjMIOiTV8G7wXGBHFRU9wkIowOSfYKtGEDHMIDLB8g+waafiGIFhCEAlw+QfYJNP+Pz1z8QpuAAm9YthLIIA7+geCgLARifEWEJZYCBAQDBkCsYzzNImAAAAABJRU5ErkJggg==) and search for ADC_MEASUREMENT
![](Pasted%20image%2020260910154428.png)
You should only need to add one ADC_MEASUREMENT App, as it will read from multiple pins.
After adding, you should see something like this is in the APP Dependency tab.
![](Pasted%20image%2020260910154431.png)
Double click on ADC_MEASUREMENT_0

You should see something like this below. Enter in the number of analog pins you'll be using for input in the "Number of measurements:" box.

![](Pasted%20image%2020260910154435.png)

Click the "Measurements" tab. This is where you enter in the names of your readings.
![](Pasted%20image%2020260910154439.png)

### Assign Pins for Reading ADC
Click the ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB0AAAAeCAYAAADQBxWhAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAGbSURBVEhLxZZfTsJAEId36xGMVzDRB+MJBNpyCx/0ZPLgJYSWcgTjgya+a6LGGBHE+A/ntyxlumU33aTUj3xMZ2FnulsoyOnsey4aRk7em28a6Ngo8m36DytFx6aV48kXouLsenO7fbr/q4+o6Str2qOmhzt5WhuXT1KcsKbBnHrk0kAhr0ujrtd+tpPQGatSXCmkQZvAFa3SE1e+jD8xrujd2K9pJ4n0kZssTvXRCnVN9yzXFPB8adWGAO815wOel76ndRCmkXKJ2SMojTDNyVUZhqlyXU1Y3F52DNNOqvQlHNLJkra6tL32R5RFSl+STqLktfijuL2A5UmbJpO+xFksuqStrvOGH49ipS8DOtE+ua4mLH+QGJgMfenSicIco0dhpYDnpckVwSr5SgGvW7rh81f7LZpMesNqQBUQtXTDV0NawPOF/daAYjUW7zVrgFXu/J5yL44GSuCK6+aadb3/rgBXrKK8f/xAVJzfbomD7dWvQV1cPQfiePdHZ9T0zmi6KYpNH2Z506bwvqZ1WP670oDO39PNKMQfs/xb+jQvmYAAAAAASUVORK5CYII=) in the upper right-hand corner. Notice that the ADC_MEASUREMENT_0 app has virtual pins that you can assign.

![](Pasted%20image%2020260910154443.png)

Click on pin_Potentiometer. Notice that pins light up grteen. These are the pins you can assign pin_Potentiometer to.

![](Pasted%20image%2020260910154447.png)

Right-click on the green part of pin 41. Click "Assign". You have assigned that pin!

![](Pasted%20image%2020260910154451.png)

## Using Dave with Git
Have the Git repo be the Project folder, not the workspace folder.

Consider leaving generated code out of the repo via the .gitignore
```
# Build output
/Debug/
/Release/
/Default/

# Object files and dependencies
*.o
*.d
*.lst

# Linked binaries
*.elf
*.hex
*.bin
*.map

# Eclipse internals
.metadata/
.cproject.user
*.launch
.settings/

# OS
.DS_Store
Thumbs.db

# If NOT tracking generated code:
# /Dave/Generated/
```

## Troubleshooting
### Launching __ has encountered a problem
![](Pasted%20image%2020260910154454.png)

Make sure that DAVE knows where JLink is. (Check the launcing command filepath in details).

Go to Window --> Preferences --> Run/Debug --> SEGGER J-Link --> Folder

And make sure that the JLink path actually exists. Sometimes it things SEGGER is in the x86 program files when it's just in the program files.

### Project file does not exist
Workspace folder --> .metadata --> .plugins --> org.eclipse.core.resources --> .projects

Delete the project that is giving you issues

Reimport the project with:

File --> import --> general --> existing projects into workspace

### Launch File Issues
Open the launch file

![](Pasted%20image%2020260910154459.png)

Make sure that the PROGRAM_NAME and PROJECT_ATTR match the actual elf file name and project name exactly, respectively.

When you do this, it will show up in the debug options.