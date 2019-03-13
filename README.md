# How to build and upload to Blue Pill

See https://youtu.be/zc11f4ZinVw

# How this sample was generated on macOS

Based on https://mynewt.apache.org/latest/get_started/project_create.html:

```
brew install go
brew tap JuulLabs-OSS/mynewt

# Must install the main branch with bluepill support
brew install mynewt-newt --HEAD

# Should show 1.6.0-dev
newt version

brew install gcc
brew tap PX4/homebrew-px4
brew update
brew install gcc-arm-none-eabi-49
```

Based on https://mynewt.apache.org/latest/tutorials/blinky/olimex.html:

```
newt new stm32bluepill-mynewt-blinky
cd stm32bluepill-mynewt-blinky
nano project.yml
```

Change
```
    vers: 1-latest
```
to
```
    vers: 0-dev
```

```
newt install

# Create bootloader
newt target create boot_bluepill
newt target set boot_bluepill build_profile=optimized
newt target set boot_bluepill app=@apache-mynewt-core/apps/boot
newt target set boot_bluepill bsp=@apache-mynewt-core/hw/bsp/bluepill

# Create app
newt target create bluepill_blinky
newt target set bluepill_blinky build_profile=debug
newt target set bluepill_blinky bsp=@apache-mynewt-core/hw/bsp/bluepill
newt target set bluepill_blinky app=apps/blinky
```

# How this sample was generated on Windows 10

Adapted from https://mynewt.apache.org/latest/newt/install/newt_linux.html

Run using Ubuntu Bash on Windows:

```
download official gcc-arm-none-eabi from Arm website
install to /opt/gcc-arm-none-eabi-8-2018-q4-major
nano ~/.bashrc
Add
<<
export PATH=$PATH:/opt/gcc-arm-none-eabi-8-2018-q4-major/bin
>>

arm-none-eabi-gcc -v
<<
gcc version 8.2.1 20181213 (release) [gcc-8-branch revision 267074] (GNU Tools for Arm Embedded Processors 8-2018-q4-major)
>>

Upgrade git to prevent "newt install" error: "Unknown subcommand: get-url":

sudo add-apt-repository ppa:git-core/ppa -y
sudo apt-get update
sudo apt-get install git -y
git --version
<<
git version 2.21.0
>>

Install go 1.10 to prevent newt build error: "go 1.10 or later is required (detected version: 1.2.X)":

sudo apt-get update
sudo apt install golang-1.10

nano ~/.bashrc
Add
<<
export PATH=$PATH:/usr/local/go/bin
export GOROOT=
>>

go version
<<
go version go1.10.1 linux/amd64
>>

mkdir /mnt/c/mynewt
cd /mnt/c/mynewt
git clone https://github.com/apache/mynewt-newt/
cd mynewt-newt/
./build.sh
<<
Building newt.  This may take a minute...
Successfully built executable: /mnt/c/mynewt/mynewt-newt/newt/newt
>>

If you see
<<
* Error: go 1.10 or later is required (detected version: 1.2.X)
>>
then install go 1.10 as shown above

sudo mv newt/newt /usr/bin
which newt
<<
/usr/bin/newt
>>
newt version
<<
Version: 1.6.0-dev
>>
 
cd /mnt/c/mynewt
newt install
<<
Downloading repository mynewt-nimble (commit: master) from https://github.com/apache/mynewt-nimble.git
Skipping "apache-mynewt-core": already installed (0.0.0)
Making the following changes to the project:
    install apache-mynewt-nimble (0.0.0)
apache-mynewt-nimble successfully installed version 0.0.0
>>

If you see
<<
Error: error: Unknown subcommand: get-url
>>
then upgrade git as shown above

# Create bootloader
newt target create boot_bluepill
newt target set boot_bluepill build_profile=optimized
newt target set boot_bluepill app=@apache-mynewt-core/apps/boot
newt target set boot_bluepill bsp=@apache-mynewt-core/hw/bsp/bluepill

# Create app
newt target create bluepill_blinky
newt target set bluepill_blinky build_profile=debug
newt target set bluepill_blinky bsp=@apache-mynewt-core/hw/bsp/bluepill
newt target set bluepill_blinky app=apps/blinky

newt build -p -v boot_bluepill
<<
Mem FLASH: 0x8000000-0x8004000
Mem RAM: 0x20000000-0x20005000
Target successfully built: targets/boot_bluepill
>>

newt build -p -v bluepill_blinky
<<
Mem FLASH: 0x8008000-0x8013800
Mem RAM: 0x20000000-0x20005000
Target successfully built: targets/bluepill_blinky
>>

newt create-image -v bluepill_blinky 1.0.0
<<
App image successfully generated: /mnt/c/mynewt/stm32bluepill-mynewt-blinky/bin/targets/bluepill_blinky/app/apps/blinky/blinky.img
Mem FLASH: 0x8008000-0x8013800
Mem RAM: 0x20000000-0x20005000
>>

newt load -v boot_bluepill
<<
TODO: Fix for Windows
>>

newt load -v bluepill_blinky
<<
TODO: Fix for Windows
>>
```

<!--
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#  KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
-->

# Apache Blinky

## Overview

Apache Blinky is a skeleton for new Apache Mynewt projects.  The user downloads
this skeleton by issuing the "newt new" command (using Apache Newt).  Apache
blinky also contains an example app and target for use with Apache Mynewt to
help you get started.

## Building

Apache Blinky contains an example Apache Mynewt application called blinky.
When executed on suitably equipped hardware, this application repeatedly blinks
an LED.  The below procedure describes how to build this application for the
Apache Mynewt simulator.

1. Download and install Apache Newt.

You will need to download the Apache Newt tool, as documented in the [Getting Started Guide](https://mynewt.apache.org/latest/get_started/index.html).

2. Download the Apache Mynewt Core package (executed from the blinky directory).

```no-highlight
    $ newt install
```

3. Build the blinky app for the sim platform using the "my_blinky_sim" target
(executed from the blinky directory).

```no-highlight
    $ newt build my_blinky_sim
```

The Apache Newt tool should indicate the location of the generated blinky
executable.  Since the simulator does not have an LED to blink, this version of
blinky is not terribly exciting - a printed message indicating the current LED
state.  To learn how to build blinky for actual hardware, please see the
[Getting Started Guide](https://mynewt.apache.org/latest/get_started/index.html).
