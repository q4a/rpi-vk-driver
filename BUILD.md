
# How to compile on a Raspberry Pi

### Install prerequisites

    sudo apt-get install cmake
    sudo apt remove libvulkan1 vulkan-headers

The Vulkan-Loader version distributed with Raspbian (1.1.97-2) does not support the required performance counters.

### Clone, build, and install RPi VK Driver

    git clone https://github.com/Yours3lf/rpi-vk-driver.git
    cd rpi-vk-driver
    mkdir build && cd build
    cmake ..
    make
    sudo make install

# How to run tests on a Raspberry Pi

### Prerequisites

* Enable Full Desktop KDM using `sudo raspi-config` under Advanced if you haven't already
* Reboot into CLI using `sudo raspi-config`

### Clone, build, and run tests

    git clone https://github.com/Yours3lf/rpi-vk-driver.git
    cd rpi-vk-driver
    mkdir build && cd build
    cmake ..
    make install
    make test

### Gotchas
* You cannot run tests from the desktop environment as there can only be one DRM-Master.  Tests can only be run after rebooting into CLI.

# How to cross-compile on Linux desktop

    git clone https://github.com/Yours3lf/rpi-vk-driver.git
    cd rpi-vk-driver
    git clone https://github.com/raspberrypi/tools.git
    export PATH=`pwd`/tools/arm-bcm2708/arm-linux-gnueabihf/bin:$PATH
    mkdir build && cd build
    cmake .. -DCMAKE_TOOLCHAIN_FILE=`pwd`/../gcc.toolchain.cmake -DCMAKE_STAGING_PREFIX=./out/usr/local
    make install -j

# How to cross-compile on Linux desktop for specific device

    git clone https://github.com/Yours3lf/rpi-vk-driver.git
    cd rpi-vk-driver
    git clone https://github.com/raspberrypi/tools.git
    export PATH=`pwd`/tools/arm-bcm2708/arm-linux-gnueabihf/bin:$PATH
    mkdir build && cd build
    cmake .. -DCMAKE_STAGING_PREFIX=./out/usr/local
    make -j
    make package

# How to generate Debian package

    git clone https://github.com/Yours3lf/rpi-vk-driver.git
    cd rpi-vk-driver
    mkdir build && cd build
    cmake ..
    make package -j

# How to check contents of Debian package

    dpkg -c rpi-vulkan-driver-*.Linux-arm.deb

# CMAKE Variables

### BUILD_NUMBER
For CI system.  Default is `1.0.0`

### CMAKE_BUILD_TYPE

Sets the build type.  Options are `Debug`, `Release`, or `MinSizeRel`.  Default is `Release`

### TARGET_SYSROOT

Point to the target sysroot.

### CMAKE_STAGING_PREFIX

The sysroot staging path.  See Linux Cross-compile example above for usage.  Default value is ""

### CMAKE_INSTALL_PREFIX

This is what the prefix should be when installed on the target sysroot.  The default for this value on Linux is `/usr/local`.

### BUILD_TESTING

Enables building Unit Test Cases.  If Cross-compiling, `make test` will not run any tests.  Default is `ON`
