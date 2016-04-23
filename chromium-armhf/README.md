Install Chromium on Debian Jessie (ARM)
=======================================

Debian does not have binary builds for Chromium in its repositories.
If you don't want to compile it from source, you can grab the binaries
from Ubuntu, which has Chromium binaries for armhf.


### The quick way...

Just download the packages from Ubuntu 14.04 Trusty and install them manually.

The advantage of this way is that you don't need to modify your system configuration,
but on the other hand you will need to manually repeat the procedure for every
update.


Get the packages from here:
[https://launchpad.net/ubuntu/trusty/+source/chromium-browser](https://launchpad.net/ubuntu/trusty/+source/chromium-browser)

E.g. version 49.0.2623.108
- [chromium-browser](https://launchpad.net/ubuntu/trusty/armhf/chromium-browser/49.0.2623.108-0ubuntu0.14.04.1.1113)
- [chromium-codecs-ffmpeg](https://launchpad.net/ubuntu/trusty/armhf/chromium-codecs-ffmpeg/49.0.2623.108-0ubuntu0.14.04.1.1113)



Because Debian Jessie ships `libgcrypt20`, you need to manually install 
`libgrypt11` beforehand, e. g. from Wheezy:  
[https://packages.debian.org/wheezy/libgcrypt11](https://packages.debian.org/wheezy/libgcrypt11)

Afterwards install the downloaded packages using
```sh
# dpkg -i <package>.deb
```


### The proper way...

Adding the Ubuntu repositories to the package manager will make updating painless, 
involves some configuration though.


First, add Ubuntu Trusty repositories as an apt source.

Create `/etc/apt/sources.list.d/ubuntu-trusty.list` with the following content:
``` /etc/apt/sources.list.d/ubuntu-trusty.list 
deb http://ports.ubuntu.com/ trusty universe
deb-src http://ports.ubuntu.com/ trusty universe
deb http://ports.ubuntu.com/ trusty-updates universe
deb-src http://ports.ubuntu.com/ trusty-updates universe

deb http://ports.ubuntu.com/ trusty-security universe
deb-src http://ports.ubuntu.com/ trusty-security universe
deb http://ports.ubuntu.com/ trusty-backports universe
deb-src http://ports.ubuntu.com/ trusty-backports universe
```

Additionally set [Apt Pin-Priorities](http://linux.die.net/man/5/apt_preferences) 
to make sure only `chromium-*` packages are installed from Ubuntu repos.

Create `/etc/apt/preferences.d/ubuntu-trusty`:
``` /etc/apt/preferences.d/ubuntu-trusty 
Package: *
Pin: origin ports.ubuntu.com
Pin-Priority: -1

Package: chromium-*
Pin: origin ports.ubuntu.com
Pin-Priority: 501
```

(Make sure the "Package: *" Pin-Priority is negative)


Once you have done that you need to install `libgcrypt11` first.
You can get it from Debian Wheezy: 
[https://packages.debian.org/wheezy/libgcrypt11](https://packages.debian.org/wheezy/libgcrypt11)  
Install it using `dpkg -i <...>.deb`

Afterwards you can install Chromium using normal apt-get:
```sh
# apt-get update
# apt-get install chromium-browser chromium-codecs-ffmpeg
```
