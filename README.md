# TJBot instructions
## Intro
![DevBot](https://i.imgur.com/q0cUuiJ.jpg)

## 3D printing and assembling the bot
First, the body of TJBot has to be printed. Some parts take quite some time to print, hence it is possible to get busy with software at the same time.
Here is useful [link](http://www.instructables.com/id/Build-a-3D-Printed-TJBot//) for instructions for 3D printing. Bot also could be laser cut instead of printing. [Here](https://ibmtjbot.github.io/images/TJBot3DPrintFiles.zip) it is possible to download a zip archive with all the necessary files. Programs such as [PrusaControl](https://prusacontrol.org/) can be used in order to prepare 3D models for printing. After printing the TJBot needs to be assembled - in addition to printed parts Raspberry Pi, a LED, and a servo are used. Raspberry Pi camera can also be added, together with a USB microphone and a USB speaker (for example, [this mic](https://www.adafruit.com/product/3367) and [this speaker](https://www.adafruit.com/product/3369)). Good video about the assembling process is [here](https://www.youtube.com/watch?v=bLt3Cf2Ui3o). Connection of LED and servo can be seen [here](https://cdn.instructables.com/FY3/RNIW/IVO4XDQN/FY3RNIWIVO4XDQN.MEDIUM.jpg?width=614), LED should preferrably be of Neopixel brand - then one would not need resistors and breadboards to connect it, and it's much easier to change its colour. 

## Preparing Raspberry Pi
SD card (for example, Samsung EVO 32 GB; general list of SD cards suitable for usage with Raspberry Pi can be found [here](https://elinux.org/RPi_SD_cards#Working_.2F_Non-working_SD_cards)) should be formatted, good program for this would be [Minitool Partition Wizard](https://www.partitionwizard.com/download.html). Afterwards, image of Raspbian has to be [downloaded](https://www.raspberrypi.org/downloads/) and written to SD card with [Windows Disk imager](https://sourceforge.net/projects/win32diskimager/). Insert SD card into Raspberry Pi, connect it via microUSB and you're done with this step!

## Setting up Raspberry Pi
After starting a Raspberry Pi and connecting Ethernet cable (or, for the sake of decreasing the number of connected cables, it can be connected by WiFi, however then Ethernet connection will not work; WiFi can be turned on directly from desktop panel), the network settings and addresses can be checked by `sudo ip addr show` via the console. Also, in the future Raspberry settings can be checked and/or edited by inputting `sudo raspi-config` in command line. At this point Raspberry can be controlled with USB mouse and keyboard and HDMI monitor, but for the future usage of TJBot it is much better to set up a 'headless' configuration, where Raspberry will be controlled remotely from, for example, your Windows laptop. Prior to that, it is a good idea to change the name and password from default `pi` and `raspberry` respectively, by using `passwd` command. Note: when inputting the passwords you will not see any symbols. 

It is also important for using the TJBot recipes in the future to install Node.js and npm (Node Package Manager) by executing the following command:
`curl -sL http://ibm.biz/tjbot-bootstrap | sudo sh -`

So, for the headless configuration, you will need [Xming server](https://sourceforge.net/projects/xming/) and [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) installed. ![PuTTY configuration](https://i.imgur.com/6joCCJL.png)

In PuTTY, input the IP address of Raspberry Pi, and then, for better connectivity, input 5 in Connection tab in "Seconds between keepalives" field, and in Connection->SSH->Kex in "Max minutes before rekey" field input 2. Finally, go to Connection->SSH->X11 and put a tick on "Enable X11 forwarding". You can save the settings by saving the session in order to avoid changing all these settings again during the next run. Finally, before opening the session, start up the Xming server, and then open the session. Xming icon should be in the tray. Say 'Yes' to the PuTTY Security Alert. Log in to your Pi with your chosen username and password, and from here on this terminal can be used as Raspberry command line. For graphical interface and direct access to the Raspberry's desktop, you can use `startlxde-pi` command.
![Raspberry Desktop via PuTTY+Xming](https://i.imgur.com/coeX06e.png)
To exit it, at any time in the PuTTY's terminal press Ctrl+C. Now your Pi is ready to become the basis for the TJBot!

There is also an easier way to use the headless Raspberry that works for both Windows and iOS. For this, one needs first to enable VNC by inputting `sudo raspi-config` in console of Pi, then going to Interfacing Options and enabling VNC. Afterwards, if Pi and another device from which you wish to control it, are on the same network, Raspberry can be accessed from that device via VNC viewer app (available for free), the only requirement is to know Raspberry's IP address which can be found by using an`ip a` command in the console and looking up the address at the required network. 

## Updating credentials, creating workspaces

The best way to start would be downloading/cloning the sample recipes from github and installing the required dependencies with the following commands:
```
git clone https://github.com/ibmtjbot/tjbot.git
cd tjbot/recipes/conversation
npm install
```
The next step is to get the credentials from IBM Bluemix for the API access. In order to get these sample recipes running we'll need three services: [Speech to Text](https://www.ibm.com/watson/services/speech-to-text/), [Text to Speech](https://www.ibm.com/watson/services/text-to-speech/) and [Conversation](https://www.ibm.com/watson/services/conversation/). Of course, if you do not have a Bluemix account, you need to create one first. After creating an instance of each service, you will need to (for each of the services) go to "Service Credentials" and copy them to clipboard. Each recipe has a `config.js` file in its folder, where corresponding usernames and passwords for the services should be pasted. Finally, for the workspace ID, you need to log in to [IBM Watson Conversation](https://watson-conversation.ng.bluemix.net/login). There, after clicking on "Import Conversation" button, choose `workspace-sample.json` from `tjbot/recipes/conversation` and import everything. Afterwards, click the dotted menu and select 'View details'. Copy your Workspace ID from there to the `config.js` file. Now it will be possible to run the code.

## Using the recipes

There are usually a few (sometimes one) `.js` files in each recipe folder. After updating the `config.js` file with the credentials as described above, and installing the dependencies with `npm install` for each recipe, those files can be run by navigating to the folder from the console and executing `sudo node recipename.js`. Each recipe allows TJBot to do something unique, for example, the Conversation one allows TJBot to answer to certain prompts, by introducing itself or making a joke. Other recipes can be found, for example, [here](https://github.com/ibmtjbot/tjbot/tree/master/featured), including visual recognition projects and projects controlling arm movement and LED blinking with the music rhythm. 

Currently implemented TJBot recipes include:
* TJ Wave (waving arm, dancing)
* TJ Vision (taking photos of surroundings, analysis of the objects in the photo)
* TJ Conversation (simple introduction and joke prompts)
* TJ Tone analysis (conversation in which TJBot responds in a different way based on the emotions detected in user's voice)


Multiple recipes can be accessed from one interface using [this](https://github.com/obielikh/tj/blob/master/test.java.txt) JAVA program. Currently this program has an extremely simple GUI which includes 6 buttons, which in turn execute shell commands and run the recipes. In order to run it, one needs to navigate to the folder where it is located from console and then execute `java test`. 

It is quite possible to edit `.js`  files inside the recipes in order to change bot's behaviour or to compelte create a new dialogue/conversation flow from IBM bluemix website. One of good tutorials about constructing your own conversation instance is [here](https://www.ibm.com/blogs/watson/2017/03/bot-yourself/).
