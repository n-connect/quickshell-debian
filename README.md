# quickshell-debian
Complete compiled Quickshell for Debian from Bookwork release

# Compilation steps

## Compilation dependencies
Warmup: the package compiled in a docker already in use for Rust/Go/C++ compilations, so it is sure from zero it needs more packages. The Docker is an official rust docker "FROM rust:1.81-slim-bookworm" since updated to 1.89 internally

So at least these needed, cmake, c++, ninja, meson, bunch of hyprland and wayland libs/dev packages were already there:
`apt install libqt6shadertools6 qt6-shadertools-dev libcli11-dev libjemalloc-dev qt6-base-private-dev qt6-declarative-private-dev`

```
git clone https://github.com/quickshell-mirror/quickshell /tmp/quickshell; cd /tmp/quickshell
git checkout v0.2.0                             
cmake -GNinja -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCRASH_REPORTER=OFF -DI3=OFF -DI3_IPC=OFF -DINSTALL_QML_PREFIX=/usr/lib/x86_64-linux-gnu/qt6/qml -DINSTALL_QMLDIR=/usr/lib/x86_64-linux-gnu/qt6/qml/org/quickshell -DDISTRIBUTOR="Debian bookworm"
cmake --build build
cmake --install build
tar -cvf /tmp/quickshell_v0.2.0_nolocal_2nd.tar.gz -T build/install_manifest.txt
```
# Copy over create tar.gz to to target system via:
`sudo docker cp <docker-id>:/path/to/quickshell_v0.2.0_nolocal_2nd.tar.gz ~/your/path/`

## Target system requirements:
- have the same version of QT6, as the compiling Docker
- have various QML6 modules, depending the actual dotfiles/config set
- a running, working Hyprland
- free space under your `$XDG_RUNTIME_DIR` mounted as 'tmpfs' usually under '/run/user/1000' if your are the a non-root first created user on your Linux
- patience for the first crash/seggfault -less run

## Target system dependencies:
Again a full Hyprland runs already with QT6 elements, like `hyprpolkitagent`, so QT6 and QT6-Wayland / QT6-wlroot packages were already there.
Additionaly, you need to install these at least:
# Quickshell asks for these only: "apt install qt6-image-formats-plugins qml6-module-qtmultimedia qml6-module-qt5compat-graphicaleffects"

`apt install qml6-module-qt-labs-platform qml6-module-qt-labs-folderlistmodel qml6-module-qt-labs-settings qml6-module-qtcore qml6-module-qt5compat-graphicaleffects qml6-module-qtquick-dialogs qml6-module-qtquick-effects fonts-roboto qt6-image-formats-plugins qml6-module-qtmultimedia`

You likely to need this special font set for sure:
```
curl -L "https://github.com/google/material-design-icons/raw/master/variablefont/MaterialSymbolsRounded%5BFILL%2CGRAD%2Copsz%2Cwght%5D.ttf" -o ~/.local/share/fonts/MaterialSymbolsRounded.ttf
fc-cache -f
```

Extract the uploaded .tar.gz file as `sudo tar -xvf /path/to/quickshell_v0.2.0_nolocal_2nd.tar.gz -C /`
Make symlink, if you don't have such one `sudo /usr/local/bin/quickshell /usr/local/bin/qs` 

## Test you quickshell like this:
`quickshell -c ~/path/to/matt/reborn/quickshell/`
If you like what you've got, copy all the content from the directory recursively, or like use `mc` for it...

Already good complete setups, the first two has its own config panels to:
- https://github.com/AvengeMedia/DankMaterialShell
- https://github.com/noctalia-dev/noctalia-shell # no autohide in main bar yet
- https://github.com/ryzendew/Reborn-Quickshell
