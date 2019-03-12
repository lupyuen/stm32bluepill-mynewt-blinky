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
