# Ubuntu-on-Samsung-Galaxy-Book

Welcome in the area where you will get the progress related to Ubuntu 19.04 on the Samsung Galaxy Book 12".
All element and information can be used at your own risk.

## What is working just after the Ubuntu installation:
  - UEFI : booting from external USB type C SSD
  - Keyboard cover
  - Touch sreen
  - Pen
  - Bluetooth

## What is working with tweak:
  - Wifi
  - Screen Brightness keyboard shortcut
  
## What is not working yet:
  - Front Camera
  - Back Camera
  - Sound card
  - Pen calibration
  - Pen Button/Eraser
  
## Wifi tweak:
```
sudo apt-get install git build-essential
git clone https://github.com/jeremyb31/ath-4.15.git
cd ath-4.15
cp /lib/modules/$(uname -r)/build/.config ./ 
cp /lib/modules/$(uname -r)/build/Module.symvers ./
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
sudo cp ath.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/ath
sudo reboot
```

After other kernel updates you will need to do
```
cd ath-4.15
make -C /lib/modules/$(uname -r)/build M=$(pwd) clean
cp /lib/modules/$(uname -r)/build/.config ./ 
cp /lib/modules/$(uname -r)/build/Module.symvers ./
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
sudo cp ath.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/ath
```

## Screen brightness tweak:
Create the following brightness script file.
```
mkdir ~/scripts/
nano ~/scripts/brightness.sh
```

```
#!/bin/sh
CURRBRIGHT=$(xrandr --current --verbose | grep -m 1 'Brightness:' | cut -f2- -d:)
if [ "$1" = "+" ] && [ $(echo "$CURRBRIGHT < 1" | bc) -eq 1 ] 
then
xrandr --output $2 --brightness $(echo "$CURRBRIGHT + 0.1" | bc)
elif [ "$1" = "-" ] && [ $(echo "$CURRBRIGHT > 0" | bc) -eq 1 ] 
then
xrandr --output $2 --brightness $(echo "$CURRBRIGHT - 0.1" | bc)
fi
```
Create a shortcut in Ubuntu :
```
CTRL + F2 => ~/scripts/brightness.sh - eDP-1
CTRL + F3 => ~/scripts/brightness.sh + eDP-1
```


## Sound stay always silent:

Unfortunatly for the moment there is no sound out of the box. Here is the alsa-test result link :
http://alsa-project.org/db/?f=35c521abab94f8163105c8a52b6cbd17802a6eeb
