# polybar-openbsd-patches

As of publishing this repo, OpenBSD ships an older release of Polybar (3.5.7) in ports and packages. These patches are for any other OpenBSD users who want to use the latest Polybar (3.6.3) instead!

The following patches were adapted from [OpenBSD ports](https://cvsweb.openbsd.org/cgi-bin/cvsweb/ports/x11/polybar/patches/), and I take absolultely no credit for them beyond rebasing them against Polybar 3.6.3:
* patch-include_modules_cpu_hpp
* patch-src_modules_cpu_cpp
* patch-src_modules_temperature_cpp

## Building

Ensure build libraries are installed

```
doas pkg_add libmpdclient pulseaudio jsoncpp libinotify cairo curl
```

Clone this repo somewhere in your home directory

```
git clone https://github.com/seabassapologist/polybar-openbsd-patches
```

Download and unpack the Polybar 3.6.3 source

```
curl -LJO https://github.com/polybar/polybar/releases/download/3.6.3/polybar-3.6.3.tar.gz
tar -zxvf polybar-3.6.3.tar.gz
```

Apply patches

```
cd polybar-3.6.3
cp ../polybar-openbsd-patches/patch-* .
patch -i patch-cmake_cmake_cxx
patch -i patch-include_modules_cpu_hpp
patch -i patch-src_modules_cpu_cpp
patch -i patch-src_modules_temperature_cpp
```
_Note: Only `patch-cmake_cmake_cxx` is actually required to build Polybar. You can skip the other three if you're not planning to use the built-in cpu and temperature modules_

Build and install

```
mkdir build
cd build
cmake ..
make -j$(sysctl hw.ncpu | cut -d "=" -f2)
doas make install
```

From here refer to the Polybar wiki for customization https://github.com/polybar/polybar/wiki

Please email teamseabass@proton.me with any questions or issues. Thanks!
