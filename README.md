# Gentoo for Forensics

Or "How to prepare a Gentoo system with the tools required to perform forensic investigations."

TODO: Intro

Be aware that installing a Gentoo Linux system can be quite challenging and time-consuming if you've never used a Linux system before. Even if you know what you're doing, the entire installation process might take you up to TODO, depending on your hardware. If you don't know a lot about Linux system internals, use a day off and ensure sufficient caffeine supply. You will learn a lot about the assembly and inner workings of a Linux system, which is never a bad thing if you want to do forensic investigations.

Finally, if you find any errors or would like to add to this guide, feel free to fork the repo and submit a pull request. 

## Installation

### Base Installation

Use the [Handbook Main Page](https://wiki.gentoo.org/wiki/Handbook:Main_Page) to identify the correct installation procedure to follow for your hardware platform, then perform the installation as per the manual. The author(s) only have access to a very limited number of hardware platforms, and the manuals guide you through the process way better than we could. Don't worry about the length of the manual chapters - you will be able to skip over many sections that cover particular scenarios that are of no interest to you. 

General notes:
 * When setting the USE flags, include `aff ewf` globally.
 * It is recommended to install [`cfg-update`](https://wiki.gentoo.org/wiki/Cfg-update) for easier handling of the configuration file changes.
 * It is recommended to use a Desktop profile - some of the tools installed later on are GUI tools.
 * You will have to install a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) like Xfce after the basic system setup to get full GUI functionality. You'll probably also want to install a [display manager](https://wiki.gentoo.org/wiki/Display_manager).
 * From here on, `#` denotes a root prompt and `$` denotes a user prompt.


Some notes regarding the installation are collected in the following sub-chapters.

#### AMD64

 * Installation within a virtual machine (e. g. VirtualBox) is possible and recommended if you want to have a simple way of removing the setup once you don't need it any longer. Make sure you have enough RAM and multiple CPU cores assigned to the VM, and install `app-emulation/virtualbox-guest-additions` or whatever package is appropriate for your hypervisor of choice.
 * Unless you're already familiar with Gentoo and systemd, use OpenRC (for no particular reason other than that it's easier to follow the installation guide).
 * Consider using a [distribution kernel](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) to make the installation and configuration process A LOT easier. You can alway switch later if you feel you have to.

### Using additional repositories

Some of the packages are not part of the main repository, but have to be pulled in from other repositories. To do so, install [`app-eselect/eselect-repository`](https://wiki.gentoo.org/wiki/Eselect/Repository) and make sure you read the page on how to use it. You will also need to install `dev-vcs/git`. 

```
# emerge -av app-eselect/eselect-repository dev-vcs/git
```

### Additional Packages

Be aware that most of the packages are masked as *testing* - read [this page on how to accept keywords for an individual package] to understand what that means and how to install these packages anyway. **DO NOT set any of the testing flags globally** unless you know what you're doing (and if you try to do that, you don't).

Also, it is a good idea to install all packages using `emerge -av` so that you can see the state of the USE flags and find out which other options you might want to enable.

#### Main Repository

Install the following additional packages from the main repository:

```
# emerge -av app-admin/sudo 
# emerge -av app-forensics/afflib 
# emerge -av app-forensics/foremost 
# emerge -av app-forensics/sleuthkit 
# emerge -av app-misc/binwalk 
# emerge -av app-mobilephone/heimdall
# emerge -av dev-util/android-tools
# emerge -av dev-util/xxdiff
# emerge -av media-gfx/exiv2 
# emerge -av media-gfx/feh 
# emerge -av net-analyzer/hping
# emerge -av net-analyzer/tcpdump 
# emerge -av net-wireless/iw
# emerge -av net-fs/nfs-utils
# emerge -av net-fs/sshfs
# emerge -av sys-block/gparted
# emerge -av sys-fs/xfsprogs 
# emerge -av sys-process/lsof
# emerge -av x11-misc/grsync
# emerge -av --autounmask=y --autounmask-write net-analyzer/tcpflow
```

Also install the following Python packages globally:

```
# emerge -av dev-python/pip 
# emerge -av dev-python/wheel
# emerge -av dev-python/fakeredis 
# emerge -av dev-python/mock 
# emerge -av dev-python/redis-py 
```

You may want to install other packages like `app-office/libreoffice-bin`, `media-gfx/gimp`, `media-video/vlc`, `www-client/chromium-bin`, `www-client/firefox-bin`. Using precompiled binaries where available will save you a lot of compilation time.

#### Additional tools from Pentoo

Enable the repository like this:

```
# eselect repository enable pentoo
# emaint sync -r pentoo
```

Install the following additional packages from the pentoo repository:

```
# emerge -av --autounmask=y --autounmask-write app-forensics/libvshadow 
# emerge -av --autounmask=y --autounmask-write app-forensics/scalpel 
# emerge -av --autounmask=y --autounmask-write app-forensics/xmount 
# emerge -av --autounmask=y --autounmask-write dev-libs/libvhdi 
```

#### Kazam for screen recording

If you want to use Kazam to create screen recordings, you first need to install an additional version of python:

```
# emerge -av dev-lang/python:3.9
```

Then, enable the repository menelkir like this:

```
# eselect repository enable menelkir
# emaint sync -r menelkir
```

Next, create a file `/etc/portage/package.use/kazam` with the following content:
```
media-video/kazam PYTHON_TARGETS: python3_9
```

Then install the following additional packages from the menelkir repository:

```
# emerge -av --autounmask=y --autounmask-write media-video/kazam 
```

### Tools requiring manual installation

Be aware that you neded to add the following lines to your `~/.bashrc` in order for the tools installed with `pip` to work:

```
export PATH="$PATH:/home/YOUR_USERNAME/.local/bin"
export PYTHONPATH="$PYTHONPATH:/home/YOUR_USERNAME/.local/lib/python3.10/site-packages"
```

#### [analyzeMFT](https://github.com/dkovar/analyzeMFT)

analyzeMFT can be installed using `pip`:

```
$ pip install --user analyzeMFT
```

TODO - not completed yet, to be continued
