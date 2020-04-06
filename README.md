

Features
=============

yotoken Coin – A Decentralized Data Sharing Platform, Hosting, and Content Distribution Network through Masternode Network.

## Coin Specifications

| Specification | Value | <br>
| NAME | yotoken |<br>
| TICKER | YOCO |<br>
| ALGO   | XEVAN |<br>
| Block Size | `3MB` |<br>
| Block Time | `300s` |<br>
| PoW Reward | `100 YOCO` |*<br>
| Masternode Requirement | `50,000 YOCO` |<br>
| Port | `23777` |
  
  
| Block Reward Distribution | </br>

100 YOCO per Block


</br>
| Masternode Reward |</br>
|:-----------|:-----------|</br>
| Block | Reward |</br>
| 20% for Stakers  | 80% of Block Reward for Masternodes |</br>



Build yotoken Wallet
----------

### Building for 64-bit Windows

The first step is to install the mingw-w64 cross-compilation tool chain. Due to different Ubuntu packages for each distribution and problems with the Xenial packages the steps for each are different.

Common steps to install mingw32 cross compiler tool chain:

    sudo apt install g++-mingw-w64-x86-64
    
Ubuntu Xenial 16.04 and Windows Subsystem for Linux

    sudo apt install software-properties-common
    sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu zesty universe"
    sudo apt update
    sudo apt upgrade
    sudo update-alternatives --config x86_64-w64-mingw32-g++ # Set the default mingw32 g++ compiler option to posix.
    
Once the tool chain is installed the build steps are common:

Note that for WSL the yotoken Core source path MUST be somewhere in the default mount file system, for example /usr/src/yotoken, AND not under /mnt/d/. If this is not the case the dependency autoconf scripts will fail. This means you cannot use a directory that located directly on the host Windows file system to perform the build.

The next three steps are an example of how to acquire the source in an appropriate way.

    cd /usr/src
    sudo git clone https://github.com/yotoken/yotoken.git
    sudo chmod -R a+r+w yotoken
    
Once the source code is ready the build steps are below.

    PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g') # strip out problematic Windows %PATH% imported var
    cd depends
    make HOST=x86_64-w64-mingw32 -j4
    cd ..
    ./autogen.sh # not required when building from tarball
    CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site 
    ./configure --prefix=`pwd`/depends/x86_64-w64-mingw32 --disable-tests
    make HOST=x86_64-w64-mingw32 -j4

### Build on Ubuntu

    sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils git cmake libboost-all-dev
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev

    # If you want to build the Qt GUI:
    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler

    git clone https://github.com/yotoken/yotoken.git --recursive
    
    cd yotoken

    # Note autogen will prompt to install some more dependencies if needed
    chmod a+x+w -R yotoken/
    cd yotoken/
    ./autogen.sh
    ./configure 
    make

### Build on OSX

The commands in this guide should be executed in a Terminal application.
The built-in one is located in `/Applications/Utilities/Terminal.app`.

#### Preparation

Install the OS X command line tools:

`xcode-select --install`

When the popup appears, click `Install`.

Then install [Homebrew](https://brew.sh).

#### Dependencies

    brew install cmake automake berkeley-db4 libtool boost --c++11 --without-single --without-static miniupnpc openssl pkg-config protobuf qt5 libevent imagemagick --with-librsvg

NOTE: Building with Qt4 is still supported, however, could result in a broken UI. Building with Qt5 is recommended.

#### Build yotoken Core

1. Clone the yotoken source code and cd into `yotoken`

        git clone --recursive https://github.com/yotoken/yotoken.git
        cd yotoken

2.  Build yotoken Core:

    Configure and build the headless yotoken binaries as well as the GUI (if Qt is found).

    You can disable the GUI build by passing `--without-gui` to configure.

        ./autogen.sh
        ./configure
        make

3.  It is recommended to build and run the unit tests:

        make check

### Run

Then you can either run the command-line daemon using `src/yotokend` and `src/yotoken-cli`, or you can run the Qt GUI using `src/qt/yotoken-qt`

For in-depth description of Sparknet and how to use yotoken for interacting with contracts, please see [sparknet-guide](doc/sparknet-guide.md).

License
-------

yotokencore is GPLv3 licensed.

Development Process
-------------------

The `master` branch is regularly built and tested, but is not guaranteed to be
completely stable. [Tags](https://github.com/yotoken/yotoken/tags) are created
regularly to indicate new official, stable release versions of yotoken.

The contribution workflow is described in [CONTRIBUTING.md](CONTRIBUTING.md).


Testing
-------

Testing and code review is the bottleneck for development; we get more pull
requests than we can review and test on short notice. Please be patient and help out by testing
other people's pull requests, and remember this is a security-critical project where any mistake might cost people
lots of money.

### Automated Testing

Developers are strongly encouraged to write [unit tests](src/test/README.md) for new code, and to
submit new unit tests for old code. Unit tests can be compiled and run
(assuming they weren't disabled in configure) with: `make check`. Further details on running
and extending unit tests can be found in [/src/test/README.md](/src/test/README.md).

There are also [regression and integration tests](/qa) of the RPC interface, written
in Python, that are run automatically on the build server.
These tests can be run (if the [test dependencies](/qa) are installed) with: `qa/pull-tester/rpc-tests.py`

### Manual Quality Assurance (QA) Testing

Changes should be tested by somebody other than the developer who wrote the
code. This is especially important for large or high-risk changes. It is useful
to add a test plan to the pull request description if testing the changes is
not straightforward.
