# Google Summer of Code: Final Evaluation Report
### Porting NetBSD to HummingBoard Pulse
```<SauravPrakash98/netbsd-src> (branch -> build_1)``` is the result of the work done during the period of GSoC.
The following commits list out my contribution to ```netbsd-src```.


|Commit|Link|
|---|---|
| Created imx8_platform.c and imx8_platform.h, and enabled its compilation with GENERIC64 kernel|https://github.com/SauravPrakash98/netbsd-src/commit/1a11c5e494f62b418eff7937bb1ad461a3b352c7|
|Adding a temporary clock controller modules for imx8 | https://github.com/SauravPrakash98/netbsd-src/commit/4e8ddc777b03578925e45975dc1d8e4d6e494c1a |
| Added composite clock driver code and an incomplete driver code for imx8mq ccm|https://github.com/SauravPrakash98/netbsd-src/commit/1aac7b016646ff19dac170500a6870947a3a4c00|
|Added uart clocks and its parents in imx8mq ccm|https://github.com/SauravPrakash98/netbsd-src/commit/fd1b6fc03fac6f2bb785058950b8947b747efff6|
|Added gate and gate2 driver, and created the struct for sccg pll clocks |https://github.com/SauravPrakash98/netbsd-src/commit/637f336422e971f4bf425f3b1e157daf7a545b98|
| Added fixed-factor clock driver, and corrected the mux and div clock drivers|https://github.com/SauravPrakash98/netbsd-src/commit/118c90458b771eee8691c9683e5164ae164d7de4|
|Added fdt based bus glue for uart driver of imx8|https://github.com/SauravPrakash98/netbsd-src/commit/5ff42a50b7cba5acc78b88e2e2c59b8a370a0ef6|
|Added fdt based iomuxc driver of imx8mq|https://github.com/SauravPrakash98/netbsd-src/commit/15d88f799cd5e4b3d4be10f7d1db95de1e1adf95|
| Enabled earlycons for hummingboard, the board boots till root device|https://github.com/SauravPrakash98/netbsd-src/commit/fa9befb8af15a7d1d3827f6d174a398b61c7f574|
|Enabled the GPC driver for IMX8MQ, uart is now interrupting|https://github.com/SauravPrakash98/netbsd-src/commit/932405c2dcc7a33f82aabfd66e47fbf94ea4289f|

Blog report for the phase 1

what's been done after that

##### Creating a bootable image for HummingBoard Pulse :


###### 1.Building NetBSD evbarm64 release:


    git clone https://github.com/SauravPrakash98/netbsd-src.git
    ./build.sh -U -m evbarm64 -O <destination folder> tools
    ./build.sh -U -u -m evbarm64 -O <destination folder> kernel=GENERIC64
    ./build.sh -U -u -m evbarm64 -O <destination folder> release


NetBSD evbarm64 release has a bootable image in ```<destination folder>/releasedir/evbarm/binary/gzimg/arm64.img.gz```

###### 2.Building the ATF and U-boot (Boot loader)
Documention present on this website to build ```flash.bin```         (https://developer.solid-run.com/knowledge-base/i-mx8m-atf-u-boot-and-linux-kernel/)
Follow steps till U-boot

Resulting flash.bin is renamed as imx8mq-flash.bin 
###### 3.Installing U-boot to netbsd image
Writing ```imx8mq-flash.bin``` to ```arm64.bin```

    dd if=imx8mq-flash.bin of=arm64.img bs=1024 seek=33 conv=notrunc

Writing the ```arm64.img``` to sd-card after unmounting the card

    dd if=arm64.img of=/dev/sdb bs=4k conv=fsync 
    
Bootable sd-card is ready !!

##### Testing:
Output can be testing through its serial port output. We use kermit to connect to the serial port of the board
Create a kermit-usb0 file:

    set line /dev/ttyUSB0
    set speed 115200
    set carrier-watch off
    set flow-control none
    set handshake none
    set prefixing all
    set streaming off
    set parity none
    connect

```/dev/ttyUSB0``` should be changed to the ```/dev/ttyUSB*``` your machine detects as the board.

Use ```dmesg | tail -20``` to find which the port the board connects to.

    dmesg | tail -15
    [ 7324.391402] usb 1-2: new full-speed USB device number 6 using xhci_hcd
    [ 7324.538843] usb 1-2: New USB device found, idVendor=0403, idProduct=6015
    [ 7324.538850] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [ 7324.538854] usb 1-2: Product: FT230X Basic UART
    [ 7324.538858] usb 1-2: Manufacturer: FTDI
    [ 7324.538862] usb 1-2: SerialNumber: DM02XSL8
    [ 7324.702956] usbcore: registered new interface driver usbserial
    [ 7324.703017] usbcore: registered new interface driver usbserial_generic
    [ 7324.703073] usbserial: USB Serial support registered for generic
    [ 7324.720811] usbcore: registered new interface driver ftdi_sio
    [ 7324.720859] usbserial: USB Serial support registered for FTDI USB Serial Device
    [ 7324.721013] ftdi_sio 1-2:1.0: FTDI USB Serial Device converter detected
    [ 7324.721093] usb 1-2: Detected FT-X
    [ 7324.721548] usb 1-2: FTDI USB Serial Device converter now attached to ttyUSB0


Now, execute the ```kermit-usb0``` file using ```kermit```

    kermit kermit-usb0
    
Plug in the power supply of the board to see the output.
##### Result:




Dillinger is a cloud-enabled, mobile-ready, offline-storage, AngularJS powered HTML5 Markdown editor.

  - Type some Markdown on the left
  - See HTML in the right
  - Magic



You can also:
  - Import and save files from GitHub, Dropbox, Google Drive and One Drive
  - Drag and drop markdown and HTML files into Dillinger
  - Export documents as Markdown, HTML and PDF


> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

### Tech

Dillinger uses a number of open source projects to work properly:

* [AngularJS] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor
* [markdown-it] - Markdown parser done right. Fast and easy to extend.
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Express] - fast node.js network app framework [@tjholowaychuk]
* [Gulp] - the streaming build system
* [Breakdance](http://breakdance.io) - HTML to Markdown converter
* [jQuery] - duh


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
