# There is a very good instruction on Nvidia website. Here just record those steps.

Here we use a 64Gb SanDisk Micro SD card. A card read is required. The operations system is Ubuntu 22.04 LTS. 

### 1. Download the SD card image from [Nvidia](https://developer.nvidia.com/jetson-nano-sd-card-image)

### 2. Write the image into SD card using [Etcher](https://www.balena.io/etcher)

### 3. Using GParted under Ubuntu to extend the unused space on SD card to the used partition. 

Now the SD card is ready to go. Easy and simple !

### 4. Insert the SD card to boot the Jetson Nano. ( connect J48 jumper to use the DC power supply ) 

### 5. Use either HDMI or DisPlay Port with keyboard & mouse to run under Ubuntu GUI, or connect the USB-to-MicroUSB cable to use the serial termimal. 

#### a. Locate the tty device

Before connecting to your Jetson developer kit for initial setup, check to see what Serial devices are already shown on your Linux computer.

`$ dmesg | grep --color 'tty'`

Connect your Linux computer to the developer kit’s Micro-USB port and run the same command to find what’s newly added.

`$ dmesg | grep --color 'tty'`

…

…

[xxxxxx.xxxxxx] cdc_acm 1-5:1.2: ttyACM0: USB ACM device

The new serial device is for your Jetson developer kit.

`$ ls -l /dev/ttyACM0`

crw-rw---- 1 root dialout 166, 0 Oct  2 02:45 /dev/ttyACM0

#### b. Using screen command to connect Jetson Nano

Install the Screen program on your Linux computer if it is now already available. For example, use this command to install Screen if you are running Ubuntu.

`$ sudo apt-get install -y screen`

Use the device name discovered previously as a command line option for the `screen` command.

`$ sudo screen /dev/ttyACM0 115200`

#### c. Terminate screen to disconnect Jetson Nano

To terminate your screen session, press C-a + k (Ctrl + a, then k), then press y on confirmation.


### ( Note that, on the first boot up, it might require to create the username and password. )
