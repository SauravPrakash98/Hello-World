# Google Summer of Code: Final Evaluation Report
### Porting NetBSD to HummingBoard Pulse
```<SauravPrakash98/netbsd-src> (branch -> build_1)``` is the result of the work done during the period of GSoC.
The following commits list out my contribution to ```netbsd-src```.
|Commit|Link|
|---|---|
| Created imx8_platform.c and imx8_platform.h, and enabled its compilation with GENERIC64 kernel|https://github.com/SauravPrakash98/netbsd-src/commit/1a11c5e494f62b418eff7937bb1ad461a3b352c7|
|Adding a temporary clock controller modules for imx8 | https://github.com/SauravPrakash98/netbsd-src/commit/4e8ddc777b03578925e45975dc1d8e4d6e494c1a |

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

# New Features!
  - Import a HTML file and watch it magically convert to Markdown
  - Drag and drop images (requires your Dropbox account be linked)


You can also:
  - Import and save files from GitHub, Dropbox, Google Drive and One Drive
  - Drag and drop markdown and HTML files into Dillinger
  - Export documents as Markdown, HTML and PDF

Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.  As [John Gruber] writes on the [Markdown site][df1]

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

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Installation

Dillinger requires [Node.js](https://nodejs.org/) v4+ to run.

Install the dependencies and devDependencies and start the server.

```sh
$ cd dillinger
$ npm install -d
$ node app
```

For production environments...

```sh
$ npm install --production
$ NODE_ENV=production node app
```

### Plugins

Dillinger is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |


### Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantanously see your updates!

Open your favorite Terminal and run these commands.

First Tab:
```sh
$ node app
```

Second Tab:
```sh
$ gulp watch
```

(optional) Third:
```sh
$ karma test
```
#### Building for source
For production release:
```sh
$ gulp build --prod
```
Generating pre-built zip archives for distribution:
```sh
$ gulp build dist --prod
```
### Docker
Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the Dockerfile if necessary. When ready, simply use the Dockerfile to build the image.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version} .
```
This will create the dillinger image and pull in the necessary dependencies. Be sure to swap out `${package.json.version}` with the actual version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on your host. In this example, we simply map port 8000 of the host to port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart="always" <youruser>/dillinger:${package.json.version}
```

Verify the deployment by navigating to your server address in your preferred browser.

```sh
127.0.0.1:8000
```

#### Kubernetes + Google Cloud

See [KUBERNETES.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)


### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
