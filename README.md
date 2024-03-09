# Downloads

Linux prebuilt binaries are available on the Releases page!

If you download them please star the repo so more people can find it!

Windows version was obtained trought Archive.org

# Building for Linux

An unofficial PKGBUILD of Citra is available for Arch Linux on the AUR.

# Dependencies:

You’ll need to download and install the following to build Citra:

## SDL2

        Deb: sudo apt install libsdl2-dev
        Arch: pacman -S sdl2
        Fedora: sudo dnf install SDL2-devel
        OpenSUSE: zypper in libSDL2-devel

## OpenSSL (optional)

        Deb: sudo apt install libssl-dev
        Arch: pacman -S openssl-1.0
        Fedora: sudo dnf install openssl-devel
        OpenSUSE: zypper in openssl-devel

## Qt 6.2+

Only 6.2+ versions are tested. Lower version might or might not work. See the section Install new Qt version below if your distro does not provide a sufficient version of Qt
Deb: sudo apt install qt6-base-dev qt6-base-private-dev qt6-multimedia-dev
You may also need apt install qt6-l10n-tools qt6-tools-dev qt6-tools-dev-tools to build with translation support
You may also need apt install libgl-dev if you run into WrapOpenGL issues while configuring with CMake.
Arch: pacman -S qt6-base qt6-multimedia qt6-multimedia-ffmpeg
You will also need to install a multimedia backend, either qt6-multimedia-ffmpeg or qt6-multimedia-gstreamer.
Fedora: sudo dnf install qt6-qtbase-devel qt6-qtbase-private-devel qt6-qtmultimedia-devel
OpenSUSE: zypper in qt6-base qt6-multimedia

    PORTAUDIO
        Deb: sudo apt install libasound-dev
        Fedora: sudo dnf install portaudio-devel
        OpenSUSE Leap 15: zypper in portaudio-devel
        OpenSUSE Tumbleweed: zypper in portaudio-devel

    XORG
        Deb: sudo apt install xorg-dev libx11-dev libxext-dev
        Fedora: sudo dnf install xorg-x11-server-devel libX11-devel libXext-devel
        OpenSUSE Leap 15: zypper in xorg-x11-util-devel libX11-devel libXext-devel
        OpenSUSE Tumbleweed: zypper in xorg-x11-util-devel libX11-devel libXext-devel

    JACK Audio Connection Kit
        Deb: sudo apt install jackd
        Fedora: sudo dnf install jack-audio-connection-kit-devel
        OpenSUSE Leap 15: zypper in libjack-devel
        OpenSUSE Tumbleweed: zypper in libjack-devel

    PipeWire
        Deb: sudo apt install libpipewire-0.3-dev
        Fedora: sudo dnf install pipewire-devel
        OpenSUSE Leap 15: zypper in pipewire-devel
        OpenSUSE Tumbleweed: zypper in pipewire-devel

    Vulkan SDK
        Ubuntu 22.04 (Jammy Jellyfish)

        wget -qO- https://packages.lunarg.com/lunarg-signing-key-pub.asc | sudo tee /etc/apt/trusted.gpg.d/lunarg.asc
        sudo wget -qO /etc/apt/sources.list.d/lunarg-vulkan-jammy.list http://packages.lunarg.com/vulkan/lunarg-vulkan-jammy.list
        sudo apt update
        sudo apt install vulkan-sdk

        Users on other distros can follow the official guide

    Optional dependencies needed for HLE AAC Decoding on Linux
        FFMPEG 4.0+
            Deb: sudo apt install ffmpeg libswscale-dev libavformat-dev libavcodec-dev libavdevice-dev
            Fedora: dnf install ffmpeg-devel compat-ffmpeg4
            OpenSUSE Leap 15: zypper in ffmpeg-3 ffmpeg-3-libavcodec-devel
            OpenSUSE Tumbleweed: zypper in ffmpeg-4 ffmpeg-4-libavcodec-devel

    Optional dependencies

    sndio
        Deb: sudo apt install libsndio-dev
        Fedora: sudo dnf -y copr enable andykimpe/shadow && sudo dnf -y install sndio
        OpenSUSE Leap 15: zypper in sndio-devel
        OpenSUSE Tumbleweed: zypper in sndio-devel

    Optional dependencies

    Gnome esound
        Deb: echo "esound require build use source code https://download.gnome.org/sources/esound/"
        Fedora: sudo dnf install esound-devel
        OpenSUSE Leap 15: zypper in libesd0-devel
        OpenSUSE Tumbleweed: zypper in libesd0-devel

    Compiler: GCC or Clang. You only need one of these two:
        GCC 11.0+.
            Deb: apt install build-essential
            Arch: pacman -S base-devel
            Fedora: dnf install gcc-c++
            OpenSUSE: zypper in gcc-c++
        Clang 15.0+
            Deb: apt install clang clang-format libc++-dev
                Note for Ubuntu users: Clang 15 is available only from 22.10 onward. For earlier distro versions, see: https://apt.llvm.org/
            Arch: pacman -S clang, libc++ is in the AUR. Use pacaur or yaourt to install it.
            Fedora: dnf install clang libcxx-devel
            OpenSUSE: zypper in clang

    CMake 3.20+
        Deb: apt install cmake
        Arch: pacman -S cmake
        Fedora: dnf install cmake
        OpenSUSE: zypper in cmake extra-cmake-modules

    Note on Boost library: you don’t need to install Boost library on your system, because citra provides a bundled trimmed Boost library. However, if you already have Boost library installed on your system, please make sure its version is at least 1.66 (which contains a bug fix for GCC 7), otherwise compilation would fail.

Cloning Citra in Git:

```
git clone --recursive https://github.com/citra-emu/citra
cd citra
```

The --recursive option automatically clones the required Git submodules too.
Building Citra in Debug Mode (Slow):

Using gcc:

```
mkdir build
cd build
cmake ../
cmake --build . -- -j"$(nproc)"
sudo make install (optional)
```

Optionally, you can use “cmake -i ..” to adjust various options (e.g. disable the Qt GUI).

Using clang:

Note: It is important you use libc++, otherwise your build will likely fail:

```
mkdir build
cd build
cmake .. -DCMAKE_CXX_COMPILER=clang++-5.0 \
	-DCMAKE_C_COMPILER=clang-5.0 \
	-DCMAKE_CXX_FLAGS="-O2 -g -stdlib=libc++"
cmake --build . -- -j"$(nproc)"
sudo make install (optional)
```

If you get a weird compile error related to std::span conversions, make sure you are using clang and libc++ 15 or up. This is an issue with libc++ 14.
Building Citra in Release Mode (Optimized):

```
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . -- -j"$(nproc)"
sudo make install (optional)
```

Building with debug symbols:

```
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build . -- -j"$(nproc)"
sudo make install (optional)
```

Running without installing:

After building, the binaries citra, citra-qt and citra-room (depending on your build options) will end up in build/bin/.

# SDL

cd build/bin/
./citra

# Qt

cd build/bin/
./citra-qt

# Dedicated room

cd build/bin/
./citra-room

Debugging:

cd data
gdb ../build/bin/citra-qt
(gdb) run

(gdb) bt

Install new Qt version

If your distribution’s version of Qt is too old, there are a few places you may be able to find newer versions.

    This Ubuntu PPA contains backports of Qt 6 to various older versions: https://launchpad.net/~savoury1/+archive/ubuntu/qt-6-2

    This unofficial CLI installer allows downloading and installing the latest first-party builds of Qt to your system (whether it works against your distribution may vary): https://github.com/miurahr/aqtinstall
