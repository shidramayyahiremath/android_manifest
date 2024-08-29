# **VIM3/3L Download Android Source** 
The Android Source Tree of our Khadas VIM3 are hosted on https://github.com/khadas. There are many different repositories. Walk through the steps below to download the Source Code.
refer this link for khadas docs https://docs.khadas.com/

**Steps**
Firstly, install git-lfs tool for downloading Android SDK.

$ mkdir git_lfs
$ cd git_lfs
$ wget https://github.com/git-lfs/git-lfs/releases/download/v2.3.4/git-lfs-linux-amd64-2.3.4.tar.gz
$ tar xvzf git-lfs-linux-amd64-2.3.4.tar.gz
$ cd git-lfs-2.3.4
$ sudo ./install.sh
$ git lfs install


Create an empty directory to hold your working files:
$ mkdir -p WORKING_DIRECTORY
$ cd WORKING_DIRECTORY 

Run repo init to download the manifest repository first:

**android 9.0 64 bit:**

$ repo init -u https://github.com/khadas/android_manifest.git -b khadas-vim3-p-64bit

**android 9.0 32 bit:**

$ repo init -u https://github.com/khadas/android_manifest.git -b khadas-vims-pie

Run repo sync to pull down the Android Source Tree:

$ repo sync

# NOTE: Don't perform repo commands as root user.


## VIM3/3L Install Toolchains

The Amlogic Platform requires extra toolchains for cross-compiling, you will need to follow these steps to setup.

### Toolchains for U-Boot

Install Cross Compiler for U-Boot BL:

$ sudo apt-get install gcc-arm-none-eabi
$ wget https://releases.linaro.org/archive/13.11/components/toolchain/binaries/gcc-linaro-aarch64-none-elf-4.8-2013.11_linux.tar.bz2
$ wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/6-2017q2/gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2
$ sudo mkdir /opt/toolchains
$ sudo tar -xjf gcc-linaro-aarch64-none-elf-4.8-2013.11_linux.tar.bz2 -C /opt/toolchains
$ sudo tar -xjf gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2 -C /opt/toolchains

### Toolchains for Linux Kernel

$ wget https://releases.linaro.org/components/toolchain/binaries/6.3-2017.02/arm-linux-gnueabihf/gcc-linaro-6.3.1-2017.02-x86_64_arm-linux-gnueabihf.tar.xz
$ wget https://releases.linaro.org/components/toolchain/binaries/6.3-2017.02/aarch64-linux-gnu/gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz
$ sudo mkdir /opt/toolchains
$ sudo tar xvJf gcc-linaro-6.3.1-2017.02-x86_64_arm-linux-gnueabihf.tar.xz -C /opt/toolchains
$ sudo tar xvJf gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz -C /opt/toolchains

# Building

Before you start to build, make sure you have done all the Preparations listed above.

## Build U-Boot

$ cd PATH_YOUR_PROJECT
$ cd bootloader/uboot
$ ./mk TARGET

## Build Android

$ cd PATH_YOUR_PROJECT
$ source build/envsetup.sh
$ lunch TARGET_LUNCH
$ make -jN otapackage

**NOTE:**
- We have seen errors when performing build using python 3.6 and higher version, so for smooth build use python 2.7.
- If you are getting this error /home/expleo/aosp9/common/scripts/gcc-version.sh: line 26: aarch64-linux-gnu-gcc: command not found, Make sure you have the necessary cross-compilation tools installed on your system, Common environment variables that may need to be configured include PATH and CROSS_COMPILE.

PATH: This variable should include the path to the cross-compilation tools. Run the following command to append the necessary path to the PATH variable:

export PATH=/path/to/cross-compilation-tools:$PATH

Replace /path/to/cross-compilation-tools with the actual path where the cross-compilation tools are installed.

CROSS_COMPILE: This variable is used to specify the prefix for cross-compilation tools. It should typically be set to the toolchain's prefix followed by a hyphen (-). Run the following command to set the CROSS_COMPILE variable:

export CROSS_COMPILE=aarch64-linux-gnu-


Use below commands to change python version.

cd /usr/bin
ls -l python*
sudo mv python python_old3
sudo ln -s python3.8 python

Gernerate images at: out/target/product/TARGET/update.img.

- Replace N as the number you want when you run make -jN.
- Replace TARGET_LUNCH to your lunch select.
-    For VIM3, it’s kvim3-userdebug.
-    TARGET should be kvim3







