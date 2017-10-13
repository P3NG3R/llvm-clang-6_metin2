# Core update to LLVM6
Hello, today I will tell you how to update your old gcc to a new llvm6.

Programm:
 - [VirtualBox](https://www.virtualbox.org/)
 - [WinSCP](https://winscp.net/eng/download.php)
 - [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
 - [Notepad++](https://notepad-plus-plus.org/download)

## Structure

- [Install Freebsd](#install-freebsd)
- [Freebsd setting](#freebsd-setting)
- [Install some packages](#install-some-packages)
- [Compile libs](#compile-libs)
- Fix source
- Compile source

## Content

### Install Freebsd

The first thing we should start with is the installation of freebsd.
 - Go to [official site freebsd](https://download.freebsd.org/ftp/releases/i386/i386/ISO-IMAGES/11.1/) and download [this .iso](https://download.freebsd.org/ftp/releases/i386/i386/ISO-IMAGES/11.1/FreeBSD-11.1-RELEASE-i386-dvd1.iso)
 - Then, we need install this disk image. I used a VirtualBox.
 - Now, we create a machine and all steps will be in pictures.
 - ![installfreebsd1](https://image.prntscr.com/image/DJnt0mv_TXeVtSJT9Uq6cQ.jpeg)
 - ![installfreebsd2](https://image.prntscr.com/image/2ok5_p7IRa6Mp9nfPH48EA.jpeg)
 - ![installfreebsd3](https://image.prntscr.com/image/b-pMiW1_SJamPo9H3m4r1Q.jpeg)
 - ![installfreebsd4](https://image.prntscr.com/image/D1zdIQ47QAaq-h2ZMhRRcA.jpeg)
 - ![installfreebsd5](https://image.prntscr.com/image/AOVOCUEjSzy7dlyM9yHRaw.jpeg)
 - ![installfreebsd6](https://image.prntscr.com/image/AfQc8f7sRVSBpdrKqy_7Og.jpeg)
 - ![installfreebsd7](https://image.prntscr.com/image/TTdQc8aURSyBXVKcjD94qw.jpeg)
 - ![installfreebsd8](https://image.prntscr.com/image/qfz8DnstSzaG3z5sr8hZ8g.jpeg)
 - Then, start the virtual machine and presses "install".
 - ![installfreebsd9](https://image.prntscr.com/image/FMjzQlTMT9uh2lbXOP2kQw.jpeg)
 - ![installfreebsd10](https://image.prntscr.com/image/FMjzQlTMT9uh2lbXOP2kQw.jpeg)
 - After choose your encoding, I left by the standard.
 - Enter the name of the machine, I wrote "llvm6".
 - ![installfreebsd11](https://image.prntscr.com/image/BIHilA_6RAm1_1JxUbXIQg.jpeg)
 - Choose (UFS), if you are an advanced user, you can choose (ZFS).
 - Then "Entire Disk" - > "MBR" ("Finish" and "Commit").
 - ![installfreebsd12](https://image.prntscr.com/image/IxGfs2w7TdyV33d7djYHJA.jpeg)
 - After installation, enter the password (my password is "dev" and repeat).
 - Select network interface. (IPv4 -> "Yes", DHCP -> "Yes", IPv6 -> "No").
 - Skip, by pressing "ENTER".
 - Choose your region and time zone. Then we choose "Set Date" twice.
 - Press "ENTER" and skip twice. After, we are asked if we want to create another user. We answer "No".
 - Manual Configuration -> "No", Complete -> "Reboot".
 - Go to the settings and turn off the drive -> "Remove". ![installfreebsd13](https://image.prntscr.com/image/-9sC639kSZiqiNkfTX7bGw.jpeg)

### Freebsd setting

First step it is open access for root users.
 - Write this in command line "ee /etc/ssh/sshd_config".
 - Then found line "#PermitRootLogin no" and change to "PermitRootLogin yes". ![freebsdsetting1](https://image.prntscr.com/image/f5GWdT7gTkSz6v7uB4kPtA.jpeg)
 - Save changes (ESC -> ENTER -> ENTER).
 - Restart SSHD (service sshd restart).
 - ![freebsdsetting2](https://image.prntscr.com/image/xzwcP0opRvWokA6SHqPH-A.jpeg)
 - Then we need to know your local ip. Press ("WIN + R" -> cmd -> ipconfig) ![freebsdsetting3](https://image.prntscr.com/image/oy7383dxRmm7MsqiL6eqkA.jpeg)
 - And last step. For SFTP connection I use [WinSCP](https://winscp.net/eng/download.php) and [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) for ssh. ![freebsdsetting3](https://image.prntscr.com/image/OA1vTtpyRk6ZtkjBYWMQ9g.jpeg) ![freebsdsetting3](https://image.prntscr.com/image/Pv2AEfNlRXSjMYXilPNxxQ.jpeg)

### Install some packages

 - pkg update
 - pkg upgrade
 - pkg install gmake
 - pkg install subversion
 - pkg install clang-devel

### Compile libs

 - Upload source. I choose [VINCHENZOO files](https://forum.turkmmo.com/konu/3516730-metin2-altyapi-server-files-guncelleme-costume-weapon-slot-effect-aciklar-fix/) and upload to /root/.
 - #### First lib (libgame), go to /src/Makefile and edit:
 ```
  CXX = g++
 ```
 replace it with
 ```
  CXX = clang++-devel
 ```
 and
 ```
  ifeq ($(GCC_VERSION), 4)
  CFLAGS  = -Wall -O2 -pipe -mtune=i686 -fno-exceptions -fno-rtti
  else
  CFLAGS  = -Wall -O2 -pipe -mcpu=i686 -fno-exceptions -fno-rtti
  endif
 ```
 replace it with
 ```
  CFLAGS = -Wall -Ofast -D_THREAD_SAFE -pipe -msse2 -mssse3 -m32 -fno-exceptions -std=c++17 -stdlib=libc++ -I../include
 ```
 After write:
 ```
  gmake clean
  gmake dep
  gmake -j20
 ```
 and we have this error:
 ```
  ./../../common/stl.h:12:10: fatal error: 'ext/functional' file not found
  #include <ext/functional>
           ^~~~~~~~~~~~~~~~
  1 error generated.
 ```
 to fix it, go to /common/stl.h and delete 3 line.
 ```
  #ifdef __GNUC__
  #include <ext/functional>
  #endif
 ```
 Then compile again (gmake -j20). If successful, then in the folder lib there will be libgame.a
 
 - Second lib (liblua), go to /config and edit:
 ```
  CC= clang-devel
 ```
 After write:
 ```
  gmake clean
  gmake -j20
 ```
 If successful, then in the folder lib there will be liblua.a & liblualib.a

 - Third lib (libpoly), go to /Makefile and edit:
 ```
  CXX = g++
 ```
 replace it with
 ```
  CXX = clang++-devel
 ```
 and
 ```
  ifeq ($(GCC_VERSION), 4)
  CFLAGS  = -Wall -O2 -pipe -mtune=i686 -fno-exceptions -fno-rtti
  else
  CFLAGS  = -Wall -O2 -pipe -mcpu=i686 -fno-exceptions -fno-rtti
  endif
 ```
 replace it with
 ```
  CFLAGS = -Wall -Ofast -D_THREAD_SAFE -pipe -msse2 -mssse3 -m32 -fno-exceptions -std=c++17 -stdlib=libc++
 ```
 After write:
 ```
  gmake clean
  gmake dep
  gmake -j20
 ```
 If successful, then there will be libpoly.a

 - Fourth lib (libserverkey), go to /Makefile and edit:
 ```
  CXX = g++
 ```
 replace it with
 ```
  CXX = clang++-devel
 ```
 and
 ```
  ifeq ($(GCC_VERSION), 4)
  CFLAGS  = -Wall -O2 -pipe -mtune=i686 -fno-exceptions -fno-rtti
  else
  CFLAGS  = -Wall -O2 -pipe -mcpu=i686 -fno-exceptions -fno-rtti
  endif
 ```
 replace it with
 ```
  CFLAGS = -Wall -Ofast -D_THREAD_SAFE -pipe -msse2 -mssse3 -m32 -fno-exceptions -std=c++17 -stdlib=libc++
 ```
 After write:
 ```
  gmake clean
  gmake dep
  gmake -j20
 ```
 If successful, then there will be libserverkey.a

 - Fifth lib (libsql), go to /Makefile and edit:
 ```
  CXX = g++
 ```
 replace it with
 ```
  CXX = clang++-devel
 ```
 and
 ```
  CFLAGS  = $(IFLAGS) -Wall -O2 -pipe -mtune=i686 -D_THREAD_SAFE -fno-exceptions 
 ```
 replace it with
 ```
  CFLAGS = $(IFLAGS) -Wall -Ofast -D_THREAD_SAFE -pipe -msse2 -mssse3 -m32 -fno-exceptions -std=c++17 -stdlib=libc++
 ```
 After write:
 ```
  gmake clean
  gmake dep
  gmake -j20
 ```
 If successful, then there will be libsql.a

 - Sixth lib (libthecore):
 In this lib very much needs to be corrected. So I suggest just delete old files and [upload my (./some files/libthecore)](https://github.com/nikita322/llvm-clang-6_metin2/tree/master/some%20files/libthecore).
 ```
  gmake clean
  gmake dep
  gmake -j20
 ```

 If successful, then there will be libthecore.a


