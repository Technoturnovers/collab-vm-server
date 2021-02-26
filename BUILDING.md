## Building the collab-vm-server 

Compiling the server is fairly easy. The server is confirmed to work on the i386 (32-bit), amd64 (64-bit), and armhf architectures on both Windows and Linux. It should also work on Aarch64, PowerPC, and MIPS machines, but note that this isn't tested.

CollabVM Server is confirmed to build and work in the following environments:

* Ubuntu, Debian, Arch Linux, CentOS, etc. 
* Windows XP Professional, Windows 7, and Windows 10, and their server counterparts.
* FreeBSD and OpenBSD
* Haiku
* Raspbian 
* Android OS (using Termux)

To build the server you will need at least GCC 5 (or greater), or Visual C++ 2012 (11.0) (or greater).

Make flags (To enable, set these ala `FLAG=1`):

- JPEG - Controls/enables JPEG support.
- DEBUG - Enables debug symbols
- V - Displays compile command lines, useful for debugging errors

### All Required Dependencies
* Boost 1.67 or above (1.67 is untested)
* GCC 8
#* Visual C++ 2019 if building with Visual Studio
* Clang 7
* libvncserver 
* libpng
* libturbojpeg
* libsqlite3-dev
* libodb
* libodb-sqlite
* libossp-uuid-dev
* RapidJSON
* UriParser


### Windows
On Windows, you have the following options:
* MSYS2 with MinGW
* Cygwin
#* Visual Studio

#### MSYS2 with MinGW 
To build the server with MSYS2 with MinGW, do the following: 

Download ODB [here.](http://www.codesynthesis.com/products/odb/download.xhtml)
 - Grab the executable for your platform from the website. Extract wherever, just make sure it's in the PATH.
	 - Go to the src/Database folder and type this command in: `odb -d sqlite -s -q Config.h VMSettings.h`

Then execute the following commands:
```
pacman -Syu
# Restart MSYS for the arch you want to compile (e.g on win32 MSYS MinGW 32bit, or on Win64 MSYS MinGW x86_64)
# Install the Win32 compiler
pacman -S --noconf mingw-w64-i686-toolchain git
# Restart MSYS again
# Install the Win64 compiler
pacman -S --noconf mingw-w64-x86_64-toolchain git
# Restart MSYS again.
# Get all of the CollabVM Server dependencies (for Win64).
./scripts/grab_deps_mw64.sh
# Get all of the CollabVM Server dependencies (for Win32).
./scripts/grab_deps_mw32.sh
# If you get a Permission Denied error, go into the scripts directory and type chmod +x *.sh.
make
# Or for Win32.
make WARCH=w32
```

and then everything should be good.

#### Cygwin 
Cygwin is currently not working correctly as it will build but it cannot start any virtual machines. There is also a bug that prevents it from being connected from IPv4, which is currently being investigated.

Regardless, to build the server with Cygwin, obviously install Cygwin with GCC and then run the following commands:

```
# Grab the dependencies 
./scripts/grab_deps_cyg.sh
# Build the server 
```

#### Visual Studio 
Visual Studio is currently unsupported, however it may be supported later once stuff is completed.

#### Linux 
On Linux, compiling with both clang and gcc seem to work fine. It is recommended to use clang.

First download ODB from either [the CodeSynthesis website](http://www.codesynthesis.com/products/odb/download.xhtml) or download it from your local repositories. (On Ubuntu, you can type `sudo apt install -y odb`).

Then run the following commands:

```
# on Arch Linux:
sudo pacman -S base-devel
# on Debian/Ubuntu:
sudo apt install -y build-essential
# on Fedora/Red Hat/CentOS/Etc:
sudo yum groupinstall 'Development Tools'
# Get all of the CollabVM Server dependencies for Linux.
# If you get a Permission Denied error, go into the scripts directory and type chmod +x *.sh.
./scripts/grab_deps_linux.sh
# cd back to the root folder, and finally, build the server 
make
```

To build with Clang (and, also possibly instrument the binary), do `make CC=clang CXX=clang++`.

#### MacOS X
**NOTE**: This is untested, and is not guaranteed to work.

On MacOS X, there is a development kit called "XCode". I have no idea at all if it would be compatible with this, so instead, this will use Homebrew to build the server.

- Open a Terminal and a web browser. Go to https://brew.sh.
- Copy the command from the home page.
- Enter your password if it asks and start the installation.
- Once it is finished installing, run the following command: **brew install boost cairo gcc ossp-uuid sqlite3**
- Because libvncserver is not available in the Homebrew Forumlas, it will have to be compiled manually. Type **git clone https://github.com/LibVNC/libvncserver**. Make sure you have the dependencies and build it with **cmake -DLIBVNCSERVER_WITH_WEBSOCKETS=OFF**
- Compile libodb and libodb-sqlite by downloading the files from Code Synthesis's website.
- Verify all the dependencies have been successfully installed and build the solution.


#### BSD
CollabVM Server is confirmed to be working on FreeBSD and OpenBSD, but it requires building ODB by yourself, which can be quite a pain. The instructions are the same as Linux for the most part except having to build odb by yourself. The source code is available on the [the CodeSynthesis website](http://www.codesynthesis.com/products/odb/download.xhtml).

##  Others?
It's possible that CollabVM Server may run on other operating systems such as GNU/Hurd, OpenIndiana, etc. 

Its possible it might run on MINIX, but these platforms are unsupported and are NOT tested. 
Let me know what crazy operating systems you get it to run on, though!