
Building from source
================================================================================

The cpp-ethereum codebase is written in
`Modern C++ style <https://msdn.microsoft.com/en-CA/library/hh279654.aspx>`_,
split across a (probably unnecessarily) large number of libraries and
applications.   The libraries are dynamically linked (DLLs or SOs), although
we `plan to support static linkage
<https://github.com/ethereum/webthree-umbrella/issues/337>`_ soon too.

To get the source code on your machine, the simplest approach is to clone the
whole `webthree-umbrella <http://github.com/ethereum/webthree-umbrella>`_
repository (or your fork of it) from Github, with recursive cloning
enabled, like so: ::

    git clone --recursive https://github.com/ethereum/webthree-umbrella.git

This repository gathers all of the C++ codebase under a single folder using
`git sub-modules <https://git-scm.com/book/en/v2/Git-Tools-Submodules>`_.



Building for Linux
--------------------------------------------------------------------------------

Building for Windows
--------------------------------------------------------------------------------


Building for OS X
--------------------------------------------------------------------------------

It is impossible for us to avoid OS X build breaks because `Homebrew is a "rolling
release" package manager
<https://github.com/ethereum/webthree-umbrella/issues/118>`_
which means that the ground will forever be moving underneath us unless we add
all external dependencies to our
`Homebrew tap <http://github.com/ethereum/homebrew-ethereum>`_, or add them as
git sub-modules within the umbrella projects.  End-user results vary depending
on when they are build the project.  Building yesterday may have worked for
you, but that doesn't guarantee that your friend will have the same result
today on their machine.   Needless to say, this isn't a happy situation.

If you hit build breaks for OS X please look through the `Github issues
<https://github.com/ethereum/webthree-umbrella/issues>`_ to see whether the
issue you are experiencing has already been reported.   If so, please comment
on that existing issue.  If you don't see anything which looks similar,
please create a new issue, detailing your OS X version, cpp-ethereum version,
hardware and any other details you think might be relevant.   Please add
verbose log files via `gist.github.com <http://gist.github.com>`_ or a
similar service.   The `cpp-ethereum gitter channel
<https://gitter.im/ethereum/cpp-ethereum>`_ is where we hang out, and try
to work together to get known issues resolved.

We **only** support the two most recent OS X versions:

- `OS X Yosemite (10.10) <https://en.wikipedia.org/wiki/OS_X_Yosemite>`_
- `OS X El Capitan (10.11) <https://en.wikipedia.org/wiki/OS_X_El_Capitan>`_

The cpp-ethereum code base does not build on older OS X versions and this
is not something which we will ever support.  If you are using an older
OS X version, we recommend that you update to the latest release, not
just so that you can build cpp-ethereum, but for your own peace of mind.


**Before doing anything else**

All OS X builds require you to `install the Homebrew <http://brew.sh>`_
package manager.

Before starting, it is **always wise** to ensure that your Homebrew setup
is up-to-date: ::

    brew update
    brew upgrade

Here's how to `uninstall Homebrew
<https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/FAQ.md#how-do-i-uninstall-homebrew>`_,
if you ever want to start again from scratch.  

Install `XQuartz <http://xquartz.macosforge.org/landing/>`_ X11 Window
system if you want to build the GUI apps.

** Installing with Homebrew **

To install the Ethereum C++ components, execute these commands: ::

    brew tap ethereum/ethereum
    brew install cpp-ethereum
    brew linkapps cpp-ethereum

Or ... ::

    brew install cpp-ethereum --with-gui

... if you want to build
`AlethZero and AlethOne <https://github.com/ethereum/alethzero>`_ and
the `Mix IDE <https://github.com/ethereum/wiki/wiki/Mix:-The-DApp-IDE>`_ too.

Then `open /Applications/AlethZero.app`, `open /Applications/AlethOne.app`, `open /Applications/Mix.app` or `eth` (CLI).

Here is the `Homebrew Formula
<https://github.com/ethereum/homebrew-ethereum/blob/master/cpp-ethereum.rb>`_
which details all the supported command-line options.

# Building from Source

Homebrew wraps up the manual build process for the latest version of **webthree-umbrella** into a simpler command-line process (and also uses a prebuilt "bottle", for Yosemite at least).   If you want to just run the build steps yourself, here's how to do it.

### Prerequisites

* Install [xcode](https://developer.apple.com/xcode/download/)

### Install dependencies

    brew install boost --c++11
    brew install cmake cryptopp miniupnpc leveldb gmp jsoncpp libmicrohttpd libjson-rpc-cpp llvm37
    brew install homebrew/versions/v8-315
    brew install qt5 --with-d-bus

NB:  The Qt5 step takes many hours on most people's machines, because it is using non-default build settings which result in build-from-source.  It also appears to use around 20Gb of temporary disk space.   Beware!

### Clone source code repository, including sub-modules

    git clone --recursive https://github.com/ethereum/webthree-umbrella.git
    cd webthree-umbrella

### Make
You can either generate a makefile and build on command-line or generate an Xcode project and build Ethereum in the IDE.

#### Generate a makefile

From the project root:

    mkdir build
    cd build
    cmake ..
    make -j6
    make install

This will also install the cli tool and libs into /usr/local.

#### Xcode

From the project root:

    mkdir build_xc
    cd build_xc
    cmake -G Xcode ..

This will generate an Xcode project file along with some configs for you: **cpp-ethereum.xcodeproj**. Open this file in XCode and you should be able to build the project

## Troubleshooting

* error: verify_app failed - you will need to use the [QTBUG-50155-workaround](https://github.com/ethereum/webthree-umbrella/wiki/QTBUG-50155-workaround)
* Build error "non-virtual thunk to CryptoPP::Rijndael::Dec::AdvancedProcessBlocks" - this is due to a [bad bottle for CryptoPP 5.6.3](https://github.com/ethereum/webthree-umbrella/wiki/CryptoPP-5.6.3-workaround)
* Unexpected "No such file or directory (or similar)" error e.g. `Sentinel.h.tmp`, `AdminUtilsFace.h.tmp`. Read the [libjson-rpc-cpp workaround](https://github.com/ethereum/webthree-umbrella/wiki/libjson-rpc-cpp-OS-X-workaround)
* Build or runtime errors, complaining about missing [libmicrohttpd.10.dylib](https://github.com/ethereum/webthree-umbrella/wiki/homebrew-47806-workaround)

Building for Android and iOS
--------------------------------------------------------------------------------

We don't currently have working Android and iOS builds, though they are on the
roadmap for the doublethinkco cross-builds.  They are fairly ordinary ARM
platforms, though with different ABIs than other ARM Linux platforms.   Those
`ABI differences <http://doublethink.co/2015/12/31/a-tale-of-two-abis/>`_ mean
that different binaries will be required for these platforms.

Building for Raspberry Pi Model A, B+, Zero, 2 and 3
--------------------------------------------------------------------------------
`EthEmbedded <http://EthEmbedded.com>`_
maintain build scripts for all Raspberry Mi models.
They are on Github in the 
`Raspi-Eth-Install <https://github.com/EthEmbedded/Raspi-Eth-Install>`_ repository.
It is also possible to cross-build for these platforms.

Building for Odroid XU3/XU4
--------------------------------------------------------------------------------
`EthEmbedded <http://EthEmbedded.com>`_
maintain build scripts for both of these Odroid models.  Support
for a broader range of Odroid devices is likely in the future.
They are on Github in the 
`OdroidXU3-Eth-Install <https://github.com/EthEmbedded/OdroidXU3-Eth-Install>`_ repository.
It is also possible to cross-build for these platforms.

Building for BeagleBone Black
--------------------------------------------------------------------------------
`EthEmbedded <http://EthEmbedded.com>`_
maintain build scripts for BBB on Github in the
`BBB-Eth-Install <https://github.com/EthEmbedded/BBB-Eth-Install>`_ repository.
It is also possible to cross-build for this platform.

Building for WandBoard
--------------------------------------------------------------------------------
`EthEmbedded <http://EthEmbedded.com>`_
maintain build scripts for the WandBoard on Github in the
`WandBoard-Eth-Install <https://github.com/EthEmbedded/WandBoard-Eth-Install>`_ repository.
It is also possible to cross-build for this platform.

Cross building
--------------------------------------------------------------------------------
`doublethinkco <http://doublethink.co>`_
maintain a Docker-based cross-build infrastructure which is
hosted on Github in the
`webthree-umbrella-cross
<http://github.com/doublethinkco/webthree-umbrella-cross>`_
repository.

At the time of writing, these cross-built binaries have been successfully used
on the following devices:

- Jolla Phone (Sailfish OS)
- Nexus 5 (Sailfish OS)
- Meizu MX4 Ubuntu Edition (Ubuntu Phone)
- Raspberry Pi Model B+, Rpi2 (Raspbian)
- Odroid XU3 (Ubuntu MATE)
- BeagleBone Black (Debian)
- Wandboard Quad (Debian)
- C.H.I.P. (Debian)

Still TODO:

- Tizen
- Android
- iOS