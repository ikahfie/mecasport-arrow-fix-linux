# Digital Alliance Meca Sport Arrow Keys Rebind for Linux
## Description
This 60% keyboard has a weird way to activate the arrow keys by using the fn+w combination, making it annoying to use because the input will registered everytime the arrow keys toggled.

This .hwdb script remap the existing keys in to arrow keys without activating the fn+w combination and only affects the connected Meca Sport keyboard.

There's two rebind method available:
* Using fn+h, fn+j, fn+k, and fn+l to simulate the vim like arrow keys. This also remap the mute, play/pause, volumeup, and volumedown to fn+x, fn+c, fn+v, and fn+b.
* Using fn+x, fn+c, fn+v, and fn+b as arrow keys while keeping the volume controls at the original combinations.

Tested on ubuntu 18.04

#### ONLY WORKS ON LINUX!

## How To Use
### **IMPORTANT! READ IT THOROUGHLY!**
* Clone or download this repo.
* Install `evtest` using your package manager.
* Run `sudo evtest` in the terminal.  

  Usually the keyboard's going to be detected as two different devices like this for example:

	`/dev/input/event*:	Gaming KB 	GamingKB`  
	`/dev/input/event*:	Gaming KB 	GamingKB`  
	`Select the device input event number [0-*]:  `  

  Each of them representing the layers of the keys. One device for the letters, numbers, ctrl keys, win keys, and alt keys, pageup and page down. The other one is for the macro keys
like volume keys, arrow keys, etc.

* Check them one by one to figure which one is the right device.  

  You know it's the right device **when the program respond only when you press the keys that need fn macro keys like volume control keys and not responding to the letter keys**. The output is something like this:  

	`Event: time 1584766683.015817, type 4 (EV_MSC), code 4 (MSC_SCAN), value c00ea`   
	`Event: time 1584766683.015817, type 1 (EV_KEY), code 114 (KEY_VOLUMEDOWN), value 1`   
	`Event: time 1584766683.015817, -------------- SYN_REPORT ------------`   
	`Event: time 1584766683.159795, type 4 (EV_MSC), code 4 (MSC_SCAN), value c00e9`   
	`Event: time 1584766683.159795, type 1 (EV_KEY), code 114 (KEY_VOLUMEUP), value 0`   
	`Event: time 1584766683.159795, -------------- SYN_REPORT ------------`   

* After you find out which one is the right one, terminate the evtest program and scroll up your terminal until you find a line like this:  

	`Input device ID: bus 0x3 vendor 0x5ac product 0x24f version 0x11`

* Open one of the downloaded .hwdb file you want to use in a text editor. Then, find and replace this line:  

  `evdev:input:bAAAAvBBBBpCCCCeDDDD*`  

  remove the 0x from each value and add zero from the left so it will become 4 digits.

  with A is the bus id (0003 if using the example above.)  
  with B is the vendor id (05ac if using the example above.)  
  with C is the product id (0024f if using the example above.)  
  and D is the version id (0011 if using the example above.)

  The replaced line will be something like this:  

	`evdev:input:b0003v05ACp024Fe0111*`  

* Save the file after you done replacing	.
* Copy the edited .hwdb file to `/etc/udev/hwdb.d/` folder. You may need to create the folder.
* Run this command in your terminal:

	`sudo systemd-hwdb update && sudo udevadm trigger --sysname-match="event*" `

## How to revert to default
* Delete the copied .hwdb file from `/etc/udev/hwdb.d/` folder.
* Run this command in your terminal:  

	`sudo systemd-hwdb update && sudo udevadm trigger --sysname-match="event*" `

## License

See the [LICENSE](LICENSE.md) file for license rights and limitations (MIT).