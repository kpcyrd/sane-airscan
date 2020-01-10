# sane-airscan -- Linux support of Apple AirScan (eSCL) compatible document scanners

If you are a lucky owner of scanner or MFP which works via network with
Apple devices, I have a good news for you: now your scanner works with
Linux as well!

In theory, this backend should work with any scanner which support
**eSCL** protocol (unofficially known as **AirScan** or **AirPrint scanning**).
In practice, I have tested it with my **Kyocera ECOSYS M2040dn**
and waiting to feedback regarding other devices.

Apple maintains [a comprehensive list](https://support.apple.com/en-us/HT201311)
of compatible devices, but please note, this list contains not only scanners
and MFP, but pure printers as well.

### Features

1. Scan from platen and ADF, in duplex and simplex modes, multi-page
scan from ADF supported as well
2. Scan in color and gray scale modes
3. Reasonably low memory footprint, achieved by on demand decompression of
image received from scanner
4. The cancel operation is as fast as possible, depending on your hardware
5. Automatic discovery and configuration of the hardware
6. Manual configuration is also possible, in case zeroconf doesn't work
(i.e., computer and scanner are connected to the different subnets)

### Compatibility

Any **eSCL** capable scanner expected to work, but only few of them
were actually tested.

Sane-airscan was tested and found to work with the following scanners:
* Canon D570
* HP DeskJet 2540
* HP ENVY 4500
* HP LaserJet Pro M28w
* Kyocera ECOSYS M2040dn
* TODO

If you have success with a scanner not included into this list,
please let me know.

### Installation from pre-build binaries

Thanks to [openSUSE Build Service](https://build.opensuse.org/), I can
provide pre-built packages for many popular Linux distros.

Currently, the following distros are supported "officially" (i.e., by me):
**Debian** (9.0 and 10), **Fedora** (29, 30 and 31),
**openSUSE** (Leap and Tumbleweed), **Ubuntu** (18.04, 19.04 and 19.10).
There are also some "3rd party" repos, currently for **Arch Linux**.

If your distro is not listed, see
[Installation from sources](https://github.com/alexpevzner/sane-airscan#installation-from-sources)
section below.

#### RPM-based distros (Fedora, openSUSE)

1. Navigate to https://download.opensuse.org/repositories/home:/pzz/
2. Find your distro in the list and enter its directory
3. Download file `home:pzz.repo`
4. Install it, using your distribution's package manager:

    * Fedore: `dnf config-manager --add-repo ./home:pzz.repo`
    * openSUSE: [see the link](https://en.opensuse.org/SDB:Add_package_repositories)

5. Install package `sane-airscan`

#### DEB-based distros (Debian, Ubuntu)

1. Navigate to https://download.opensuse.org/repositories/home:/pzz/
2. Find your distro in the list and enter its directory
3. Download file `Release.gpg`
4. Add it to the list of trusted keys:

        apt-key add Release.key

5. Add the repository:

        add-apt-repository -m "deb https://download.opensuse.org/repositories/... ./"

    note, the URL above is the path to repository that you have choosen in step 2

6. Install package `sane-airscan`

#### Other distros

##### Arch Linux

Thomas Kiss <thokis@gmail.com> maintains package for Arch Linux:

* https://aur.archlinux.org/packages/sane-airscan-git/

### Installation from sources
#### Install required libraries - Fedora and similar
As root, execute the following commands:
```
dnf install gcc git make pkgconf-pkg-config
dnf install avahi-devel avahi-glib-devel
dnf install glib2-devel libsoup-devel libxml2-devel
dnf install libjpeg-turbo-devel sane-backends-devel
```
#### Install required libraries - Ubuntu, Debian and similar
As root, execute the following commands:
```
apt-get install libavahi-client-dev libavahi-glib-dev
apt-get install gcc git make pkg-config
apt-get install libglib2.0-dev libsoup2.4-dev libxml2-dev
apt-get install libjpeg-dev libsane-dev
```
#### Download, build and install sane-airscan
```
git clone https://github.com/alexpevzner/sane-airscan.git
cd sane-airscan
make
make install
```
### Reporting bugs
To report a bug, please [create a new GitHub issue](https://github.com/alexpevzner/sane-airscan/issues/new)

To create a helpful bug report, please perform the following steps:

1. Enable protocol trace in the sane-airscan, by adding the following
entries into the configuration file:
```
[debug]
trace = ~/airscan/trace ; Path to directory where trace files will be saved
```
You may use an arbitrary directory path, assuming you have enough rights
to create and write this directory. The directory will be created automatically.

2. Reproduce the problem. Please, don't use any confidential documents
when problem is being reproduces, as their content will be visible to
others.

3. Explain the problem carefully

4. In the directory you've specified as the trace parameter, you will find
two files. Assuming you are using program xsane and your device name is
"Kyocera MFP Scanner", file names will be **"xsane-Kyocera-MFP-Scanner.log"**
and **"xsane-Kyocera-MFP-Scanner.tar"**. Please, attach both of these files
to the new issue.

## References

The eSCL protocol is not documented, but this is simple protocol,
based on HTTP and XML, easy for reverse engineering. There are many
Internet resources around, related to this protocol, and among others
I want to note the following links:

* [kno10/python-scan-eSCL](https://github.com/kno10/python-scan-eSCL) - a tiny
Python script, able to scan from eSCL-compatible scanners
* [SimulPiscator/AirSane](https://github.com/SimulPiscator/AirSane) - this
project solves the reverse problem, converting any SANE-compatible scanner
into eSCL server. Author claims that it is compatible with Mopria and
Apple clients
* [markosjal/AirScan-eSCL.txt](https://gist.github.com/markosjal/79d03cc4f1fd287016906e7ff6f07136) - document,
describing eSCL protocol, based on reverse engineering. Not complete and
not always accurate, but gives the good introduction
