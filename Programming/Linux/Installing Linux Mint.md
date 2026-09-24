## Verifying
https://forums.linuxmint.com/viewtopic.php?f=42&t=291093

Follow that tutorial for windows ^

I downloaded three things
1. ISO File
2. txt verification file
3.  gpg verification file
And put all three into the same folder

I installed gnupg software to verify everything was legit
Right clicked on the folder with the three files, ran this commmand:
```
CertUtil -hashfile filename.iso SHA256
```

Then compares the output of the command to the contents of the text file
Checked out

## Loading
Created "bootable media" aka turned my USB Stick into a Linux booter or whatnot
Installed Etcher, and Etcher had me select the linux mint iso, then flashed the USB stick

Put the USB  stick into my Thinkpad, and booted into Linux Mint live from the USB stick

## Uh oh
Ran into an issue when installing Linux Mint where it asked me where i am and I typed in my location and the installer crashed

Tried running the installer again and I encountered another error when trying  to install media drivers

Tried going into linux live again, and it told me to remove the bootable media and press enter
	(Might have made things  worse because I removed the USB stick despite linux not actually being fully installed)

From here i
