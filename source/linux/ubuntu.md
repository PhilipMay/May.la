# Ubuntu

## Package Management

### Update the System
``` bash
apt update       # Fetches the list of available updates
apt list --upgradable # see list of packages that can be upgraded
apt upgrade      # Installs some updates; does not remove packages
apt full-upgrade # Installs updates; may also remove some packages, if needed
apt autoremove   # Removes any old packages that are no longer needed
apt clean        # clean cache
```

### Get Info about a .deb File
`dpkg -I <package_name>.deb`

### List Content of a .deb File
`dpkg -c <package_name>.deb`

### List installed packages
`apt list --installed`

### List manually installed Packages
`apt-mark showmanual`

### Remove Package with dependencies
`apt-get remove --auto-remove <package_name>`

### Clean Package Cache
`apt-get clean`

## Check Ubuntu Version
`lsb_release -a`

## Install Ubuntu as a VirtualBox Guest
This describes how to install Ubuntu as a VirtualBox guest system to do
experiments with docker or other stuff.

### Basic installation
  - download a server version of Ubuntu
  - the current LTS version might be a good idea
  - do normal installation

### Install XFCE
To be able to copy and paste to and from the guest system you need a
desktop environment like XFCE. XFCE is small compared to GNOME and the
minimal installation does not contain LibreOffice and other stuff you do
not need. - install `tasksel`: `sudo apt install tasksel` - execute
`tasksel`: `sudo tasksel` - select `Xubuntu minimal installation` -
press tabulatur and select `Ok` with return key - reboot

### Install VirtualBox Guest Additions
  - `apt install build-essential dkms`
  - link the VirtualBox Guest Additions iso image to a cd drive
  - cd should be auto mounted to somewhere at `/media` if XFCE or GNOME
    is installed
  - change to that directory and execute installation: `sudo
    ./VBoxLinuxAdditions.run`

also see here:
<https://askubuntu.com/questions/1035030/virtualbox-guest-additions-installation-problem/1047193#1047193>

# Install Docker
To install docker do not install the package called `docker`. Docker is
a " System tray for KDE3/GNOME2 docklet applications". The package you
need is called `docker.io` - install with: `sudo apt install docker.io`
- start with: `sudo systemctl start docker` - stop with: `sudo systemctl
stop docker` - enable (always start at boot time) with: `sudo systemctl
enable docker`
