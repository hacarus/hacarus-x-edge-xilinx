# HACARUS-X Edge for Xilinx – Technical Document(English Version)


# 1. Introduction

HACARUS-X Edge for Xilinx is a cutting-edge, high-speed, low-power AI starter kit which is capable of machine learning and inference.

This kit consists of the Xilinx ZCU104 Evaluation Board, software which can installed through an SD card, and AI hardware files.

## 1.1 Release Notes

- 2018/12/27 Motion Detection Functionality Release

## 1.2 Support

We offer technical support via Email to customers that have purchased the HACARUS-X Edge for Xilinx. For customers that require support, please send an Email including the FPGA Board’s serial number to <fpga-support@hacarus.com>.

---
# 2. Overview

## 2.1 System Configuration

The HACARUS-X Edge for Xilinx is put together using Hacarus’s original AI algorithms which are hardware accelerated through the [Xilinx Zynq UltraScale+ MPSoC ZCU104 Kit](https://japan.xilinx.com/products/boards-and-kits/zcu104.html). (For the sake of simplicity, it will be called ZCU104 Evaluation Kit henceforth.) The ZCU104 Evaluation Kit is an FPGA platform setup for the sake of embedded vision applications, so peripheral devices and interfaces are furnished with the board itself.


![HACARUS-X Edge for Xilinx Overview](https://i.imgur.com/MoRkhuR.png)

Using the HACARUS-X Edge for Xilinx, the live video stream from a USB camera is processed as learning data through Hacarus’s original AI algorithms. It is possible to view the results of machine learning predictions from monitor output. Through serial communication, analysis results can also be sent to the host machine.

### 2.1.1 Motion Detection (Calculated On A Cloud Server)

A USB camera input and an HDMI/DisplayPort monitor output is used for motion detection. Computations are performed on a Cloud Server. From the live video stream obtained from a fixed USB camera, moving objects are recognized by the algorithm and marked before being output to the monitor. It is possible to view the resulting video by downloading the corresponding zip file with the filename extention “moving-object.”

---
# 3. System Requirements

## 3.1 Hardware

The hardware listed below is required to operate the HACARUS-X Edge for Xilinx.

* Items included in ZCU104
    * ZCU104 Evaluation Board
    * e-con Systems See3CAM_CU30_CLT_TC USB Camera
    * 4-Port USB 3.0 Hub
    * Power Cord and Adapter
    * Micro USB Cable (used for connecting the ZCU104 board to the host machine)
    * Micro SD Card
* Items that must be provided by the user
    * Any DisplayPort or HDMI input Monitor with the one of the resolutions listed below.
        * 3840x2160
        * 1920x1080
        * 1280x720
    * Display Port Cable or HDMI Cable

## 3.2 Software

* Serial Terminal Emulator
* SD Card Installation File
    * You can get the required files from the download website. A link to the download website will be sent to you upon purchase of the HACARUS-X Edge for Xilinx.

## 3.3 License

The license for HACARUS-X Edge for Xilinx becomes effective immediately after purchase. The license is included with 3 months of software and hardware support, as well as software updates.

To receive software or hardware support after 3 months from the time of purchase, please contact customer support.

## 3.4 Compatibility

The functionality of HACARUS-X Edge for Xilinx has been confirmed for the hardware configurations listed below. Please note that, in the event that the user decides to use hardware other than what is listed below (i.e. OS, USB camera), they will be unable to receive support for any issues that may arise thereafter.

* Host Machine/Terminal Emulator

OS| Emulator
------------- | -------------
Ubunts 16.04 | gnome-terminal
Mac OS | Terminal.app
Windows 10 Home | PuTTY

* USB3 Camera

Model | Resolution
------------- | -------------
e-con See3CAM_CU30 | 1920x1080

Functionality has been confirmed using LG Monitor 27UD58 (1920x1080) connected via HDMI. If a problem occurs using a different type of connection or resolution, please contact customer support.


---
# 4. Installation and Method of Operation

## 4.1 Board Setup

1.Connect the 12V power plug to connector (3).
2.Connect your output monitor to connector (7) using an HDMI cable. Alternatively, you can connect a DisplayPort cable to connector (6).
3.Connect the Micro side of your Micro-USB cable to to the USB-UART (connector (1)), and connect the USB side to a USB port on our host machine. 
4.Next, take the SD card, which should contain the ZIP file that you downloaded (refer to Section 3.2), and plug it into the Micro-USB slot (2). Confirm that the file hierarchy is as stated below.

 * [SD_CARD]/gstreamer-1.0/libgstsdxmotiondetection.so
 * [SD_CARD]/lib/libgstsdxallocator.so
 * [SD_CARD]/lib/libgstsdxbase.so
 * [SD_CARD]/lib/libmotiondetection.so
 * [SD_CARD]/BOOT.BIN
 * [SD_CARD]/gstdemo
 * [SD_CARD]/image.ub
 * [SD_CARD]/init.sh
 * [SD_CARD]/run.sh
 * [SD_CARD]/video_cmd
 * [SD_CARD]/README.txt

5.Connect your e-con See3CAM_CU30 USB Camera to USB connector (5).
6.Confirm that the Boot Mode Switch (8) is set to (1,2,3,4)=(ON, OFF, OFF, OFF).


![Board Overview](https://user-images.githubusercontent.com/76630/50266373-9dac4980-0466-11e9-82c9-a465b5ee18b9.png)

<div style="font-size:0.8em;">(※)You can reboot the Linux system on the FPGA using the Reboot Switch(either of the (9) switches)Mid-execution、in the event that there are connection mistakes (e.g. USB camera being unplugged)、you can simply reboot the system using this switch when necessary.</div>

## 4.2 [xyz] Application Execution

### 4.2.1 Step 1 (Preparation)
1.Using the same method from section 4.1, copy the files to the Micro SD card and insert the card into the Micro SD slot (2).
2.Switch the power switch (4) on the FPGA on and wait for the FPGA to boot up.

 * In the event that the micro USB is configured correctly, the *** LED should change from red to green before turning off. If the LED stays green, you may not have configured the FPGA correctly in section 4.1. In particular, please confirm that the boot mode switch is set to (1,2,3,4)=(ON,OFF,OFF,OFF), and also that the micro SD card’s directory hierarchy is correct.
 * By flipping the reboot switch (either of the (9) switches), you can reboot the Linux system on the FPGA. Mid-execution, you may reboot the Linux system with the reboot switch if you need make adjustments to the hardware setup (USB camera, etc).

3.Open a Serial Terminal Emulator (e.g. PuTTY, Tera Term) from your host machine and connect to the FPGA board with the Serial Port configured to the settings listed below.

Item | Value
------------- | -------------
Port  | refer to (*2)
Baud Rate | 115200
Data Bits | 8bit
Stop Bits | 1bit
Parity | None
Flow Control | None
   
   (*w) The port number will vary depending on what host machine you are using.

 * In order to confirm that your computer's serial port is connected to the FPGA:
    * If you are using Linux,
        Typing "ls -l /dev/ttyUSB*" will display your devices. You have to select the 2nd device. For example、if /dev/ttyUSB0, /dev/ttyUSB1, /dev/ttyUSB2, and /dev/ttyUSB3 are displayed、you have to select /dev/ttyUSB1. Furthermore、you have to change the permissions of the port that you select using chmod 666.
    * If you are using Mac,
        Typing "ls -l /dev/tty.*" will display your devices. You have to select the 2nd device. For example, if /dev/tty.usbserial-000000, /dev/tty.usbserial-000001, /dev/tty.usbserial-000002, and /dev/tty.usbserial-000003 are displayed, you have to select /dev/tty.usbserial-000001.
    * If you are using Windows OS,
        You will have to select the port that corresponds to the second COM/LPT port that is displayed in the device manager. For example, if the device manager shows USB Serial Port(COM1), USB Serial Port(COM2), USB Serial Port(COM3), and USB Serial Port(COM4), you need to select the COM2 port.
 * In order to connect your computer to the FPGA's serial port:
    * If you are using Linux,
        Open gnome-terminal, type "screen /dev/ttyUSB1 115200" and press enter. This connects your computer to the FPGA's serial port with a Baud Rate of 115200. If you ever want to disconnect, press Ctrl+a followed by k using your keyboard. A message saying "Really kill this window [y/n]" will display in the bottom left hand corner of your screen. Typing y will close your connection.
    * If you are using Mac,
        Execute Terminal.app, type "screen /dev/tty.usbserial-000000 115200" and press enter. This connects your computer to the FPGA's serial port with a Baud Rate of 115200. If you ever want to disconnect, press Ctrl+a followed by k using your keyboard. A message saying "Really kill this window [y/n]" will display in the bottom left hand corner of your screen. Typing y will close your connection.
    * If you are using Windows OS,
        Execute PuTTY. Open the PuTTY Configuration window and change Connection type to "Serial". Next, change Serial line to "COM2" and Speed to "115200". Clicking Open will finalize these changes and enable you to connect to the serial port of the FPGA board with a Baud Rate of 115200.


### 4.2.2 Step 2 (Execution)
1.Wait until the command prompt appears in the serial terminal window.
2.When the command prompt appears, type in the following command to change to the SD card directory.

```
# cd /media/card
```

3.Next, enter the following command in order to copy the shared library from the micro SD card to the FPGA.

```
# bash init.sh
```

4.Enter the following command in order to execute the application.

```
# bash run.sh
```

# 5 Application Settings

## 5.1 Motion Detection Settings

When necessary, the user may modify the Motion Detection settings that are written in the app.cfg configuration file. However, the user may only adjust the values of the variables listed below within the specified range (between the specified minimum and maximum values).
Furthermore, if you change the names of any of the variables below (Example: Changing MD_THRESHOLD to MD_THRESH), the changed variable will be ignored by the program, so please refrain from doing so.

Variable| Default Value | Minimum Value | Maximum Value | Details 
------------- |  ------------- | ------------- | ------------- | -------------
MD_THRESHOLD |  15 | 1 | 100 | Smaller values increase the sensitivity of the motion detection.
THRE_RECT_MIN_W |  20 | 1 | Maximum width of rectangles |  Minimum width of rectangles(*3)
THRE_RECT_MIN_H |  20 | 1 | Maximum height of rectangles |  Minimum height of rectangles(*3)
RECT_BORDER_W |  3 | 1 | 10 | Rectangle border thickness
THRE_MIN_G | 0.002 | > 0 | < THRE_MAX_G | Smaller values decrease the number of rectangles.
THRE_MAX_G | 0.7 | > THRE_MIN_G | 1 | Larger values decrease the number of rectangles.
THRE_SUP_D | 0.1 | 0 | 1.00 | Smaller values decrease the number of rectangles

(*3) Rectangles with width less than THRE_RECT_MIN_W and height less than THRE_RECT_MIN_H are not printed to the monitor.

---
# 6 Reference Materials

* [Xilinx Zynq UltraScale+ MPSoC ZCU104 Kit](https://www.xilinx.com/products/boards-and-kits/zcu104.html)
