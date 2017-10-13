# Core update to LLVM6
Hello, today I will tell you how to update your old gcc to a new llvm6.

## Structure

- Install Freebsd
- Freebsd setting
- Install some packages
- Compile libs
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
 - ![installfreebsd6](https://image.prntscr.com/image/JaG11tW0SuOmlSJIHscmfQ.jpeg)
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
 - And last step. ![freebsdsetting2](https://image.prntscr.com/image/xzwcP0opRvWokA6SHqPH-A.jpeg)

### Install some packages

