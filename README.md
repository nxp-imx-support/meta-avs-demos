# meta-avs-demos for AVS_SDK
---

Recipes to include Amazon's Alexa Voice Services in your applications.
---
### Step 1 : Get iMX Yocto AVS setup environment

Review the instructions under Chapter 3 of the **i.MX_Yocto_Project_User's_Guide.pdf** on the [L4.X.X_X.X.X_LINUX_DOCS](https://www.nxp.com/webapp/Download?colCode=L4.1.15_1.0.0_LINUX_DOCS&Parent_nodeId=1276810298241720831102&Parent_pageType=product) to prepare your host machine. Including at least the following essential Yocto packages

	$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
      build-essential chrpath socat libsdl1.2-dev u-boot-tools

#### Install the i.MX NXP AVS repo

Create/Move to a directory where you want to install the AVS yocto build enviroment.
Let's call this as <yocto_dir>

	$ cd <yocto_dir>
	$ repo init -u https://source.codeaurora.org/external/imxsupport/meta-avs-demos -b master -m imx7d-pico-avs-sdk_4.1.15-1.0.0.xml

#### Download the AVS BSP build environment:

	$ repo sync
---
### Step 2: Run i.MX NXP Yocto Setup for Alexa_SDK image with AVS-SETUP-DEMO script:

Run the avs-setup-demo script as follows to setup your environment for the imx7d-pico board:

	$ MACHINE=imx7d-pico DISTRO=fsl-imx-x11 source avs-setup-demo.sh -b <build_sdk>

Where <build_sdk> is the name you will give to your build folder.

After acepting the EULA the script will prompt if you want to enable:

#### Sound Card selection

The following Sound Cards are supported on the build:

* SGTL (In-board Audio Codec for PicoPi)
* 2-Mic Conexant

The script will prompt if you are going to use the Conexant Card. If not then SGTL will be assumed as your selection

	Are you going to use Conexant Sound Card [Y/N]?

#### Install Alexa SDK

Next option is to select if you want to pre-install the AVS SDK software on the image.

	Do you want to build/include the AVS_SDK package on this image(Y/N)?

If you select **YES**, then your image will contain the AVS SDK ready to use (after authentication).
Note this AVS_SDK will not have WakeWord detection support, but it can be added on runtime.

If your selection was **NO**, then you can always manually fetch and build the AVS_SDK on runtime.
All the packages dependencies will be already there, so only fetching the AVS_SDK source code and building it is required.


#### Finish avs-image configuration

At the end you will see a text according with the configuration you select for your image build.

Next is an example for a Preinstalled AVS_SDK with Conxant Sound Card support and WiFi/BT not enabled.

	==========================================================
	 AVS configuration is now ready at conf/local.conf

	 - Sound Card = Conexant
	 - AVS_SDK pre-installed

	 You are ready to bitbake your AVS demo image now:

	    bitbake avs-image

	==========================================================


---

### Step 3: Build the AVS image

Go to your <build_sdk> directory and start the build of the avs-image

    $ cd  <yocto_dir>/<build_sdk>
    $ bitbake avs-image
---

### Step 4 : Deploying the built images to SD/MMC card to boot on target board.

After a build has succesfully completed, the created image resides at

    <build_sdk>/tmp/deploy/images/imx7d-pico/

In this directory, you will find the **imx7d-pico-avs.sdcard** image.

To Flash the .sdcard image into the eMMC device of your PicoPi board follow the next steps:

- Download the [bootbomb flasher](ftp://ftp.technexion.net/development_resources/development_tools/installer/pico-imx7-imx6ul-imx6ull_otg-installer_20170112.zip) and follow the instruction on **Section 4. Board Reflashing** of the [Quick Start Guide for AVS kit](https://www.nxp.com/docs/en/user-guide/Quick-Start-Guide-for-Arrow-AVS-kit.pdf) to setup your board on flashing mode.

- Copy the built SDCARD file

		$ sudo dd if=imx7d-pico-avs.sdcard of=/dev/sd<partition> bs=1M && sync
		$ sync

- Properly eject the pico-imx7d board:

		$ sudo eject /dev/sd<partition>

---

### NXP Documentation

Refer to the [Quick Start Quide for AVS SDK](https://www.nxp.com/docs/en/user-guide/Quick-Start-Guide-for-Arrow-AVS-kit.pdf) to fully setup your PicoPi board with  Synaptics 2Mic and PicoPi i.mx7D

For a more comprehensive understanding of Yocto, its features and setup; more image build and deployment options and customization, please take a look at the [i.MX_Yocto_Project_User's_Guide.pdf](https://www.nxp.com/webapp/Download?colCode=L4.1.15_1.0.0_LINUX_DOCS&Parent_nodeId=1276810298241720831102&Parent_pageType=product) document from the Linux documents bundle mentioned at the beginning of this document.

For a more detailed description of the Linux BSP, u-boot use and configuration, please take a look at the [i.MX_Linux_User's_Guide.pdf](https://www.nxp.com/webapp/Download?colCode=L4.1.15_1.0.0_LINUX_DOCS&Parent_nodeId=1276810298241720831102&Parent_pageType=product) document from the Linux documents bundle mentioned at the beginning of this document.


---
