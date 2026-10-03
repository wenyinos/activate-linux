# activate-linux
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

The "Activate Windows" watermark ported to Linux with cairo in C

"Science isn't about WHY. It's about WHY NOT. Why is so much of our science dangerous? Why not marry safe science if you love it so much. In fact, why not invent a special safety door that won't hit you on the butt on the way out, because you are fired." — Cave Johnson (Portal 2)

> Read [ARGS.md](ARGS.md) for information about command line arguments.

## New Features

- Draggable overlay on X11 with `-M` / `--x11-draggable`.
- Draggable overlay on Wayland (layer-shell based) with `-Y` / `--wayland-draggable`.
- Overlay position persistence after drag release.
  - Auto-saved to `~/.config/activate-linux.cfg`.
  - Custom config path is supported with `-C` / `--config-file`.
- Dynamic distro label for Linux and BSD presets.
  - Linux now shows current distro name/version (for example `Fedora 43`).
  - BSD preset now shows runtime system/version (for example `FreeBSD 14.1`, `OpenBSD 7.6`).
- Expanded build-from-source guides for Linux, FreeBSD, OpenBSD, Fedora/RHEL/CentOS, Windows (MSYS2), and more.

## Prebuilt Packages

Every [release](https://github.com/wenyinos/activate-linux/releases) ships artifacts for `x86_64` and `aarch64`:

- `activate-linux-x86_64.AppImage` / `activate-linux-aarch64.AppImage` — self-contained, just `chmod +x` and run. Requires FUSE (`libfuse2`); if FUSE is unavailable, run with `--appimage-extract-and-run`.
- `activate-linux-<version>-linux-x86_64.tar.gz` / `activate-linux-<version>-linux-aarch64.tar.gz` — portable tarball (binary, man page, desktop file, icon, docs). Unpack and run `./activate-linux` (add `-Y` or `-M` to enable dragging).
- `activate-linux-<version>-freebsd-amd64.pkg` / `activate-linux-<version>-freebsd-aarch64.pkg` — FreeBSD 14+ packages. Install with `pkg install ./activate-linux-<version>-freebsd-amd64.pkg`; `pkg` pulls in the required cairo/pango/wayland/X11/libconfig dependencies automatically.
- `activate-linux-<version>-openbsd-amd64.tgz` / `activate-linux-<version>-openbsd-arm64.tgz` — OpenBSD 7.7+ packages. The package is unsigned, so install with `pkg_add -D unsigned ./activate-linux-<version>-openbsd-amd64.tgz`; `pkg_add` resolves the cairo/pango/wayland/libconfig dependencies automatically.

### Tarball minimum system requirements

Tarballs are built on a RHEL 8 baseline (glibc 2.28) and run on any newer distribution (RHEL 8+, Ubuntu 20.04+, Fedora, Debian 11+, ...):

- glibc ≥ 2.28
- cairo (`libcairo.so.2`) ≥ 1.15
- pango + pangocairo ≥ 1.42 (pulls in glib/fontconfig/freetype/harfbuzz)
- Wayland sessions: `libwayland-client.so.0` (≥ 1.17) with a wlr-layer-shell capable compositor
- X11 sessions: `libX11` ≥ 1.6.8, `libXext` / `libXfixes` / `libXinerama` / `libXrandr`
- `libconfig.so.9` (RHEL/CentOS: from EPEL; Ubuntu/Debian: `libconfig9`)

## Building
Note that the executable's name depends on the target platform.
You can use `make install` to install and `make uninstall` to remove it.

### Xmake
I've been experimenting with using [Xmake](https://xmake.io/#/) for building instead of make, you can test this by installing it and running
```console
xmake
```
Please leave feedback to improve this if you want

### *nix:
```console
make
```

### Windows (using [MSYS2](https://msys2.org)):
Replace `gcc` with `clang`, if you want. But then don't forget to start proper shortcut `MSYS2 CLANG64`.
```console
pacman -S --noconfirm git make pkgconf pactoys
pacboy -S --noconfirm gcc:p
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
export backends=gdi
make
```

## Linux

### Dependencies
This project depends on:
- [`libcairo2-dev`](https://cairographics.org)
- [`libpango1.0-dev`](https://www.gtk.org/docs/architecture/pango)
- [`libxi-dev`](https://gitlab.freedesktop.org/xorg/lib/libxi)
- [`libx11-dev`](https://gitlab.freedesktop.org/xorg/lib/libx11)
- `x11proto-core-dev`
- [`x11proto-dev`](https://gitlab.freedesktop.org/xorg/proto/x11proto)
- [`libxt-dev`](https://gitlab.freedesktop.org/xorg/lib/libxt)
- [`libxext-dev`](https://gitlab.freedesktop.org/xorg/lib/libxext)
- [`libxfixes-dev`](https://gitlab.freedesktop.org/xorg/lib/libxfixes)
- [`libxinerama-dev`](https://gitlab.freedesktop.org/xorg/lib/libxinerama)
- [`libxrandr-dev`](https://gitlab.freedesktop.org/xorg/lib/libxrandr)
- [`libwayland-dev`](https://gitlab.freedesktop.org/wayland/wayland)
- [`wayland-protocols`](https://gitlab.freedesktop.org/wayland/wayland-protocols)

Optional dependencies:
- [`libconfig-dev`](https://hyperrealm.github.io/libconfig)


## FreeBSD

### Dependencies

Use `pkg` to install build tools and runtime dependencies:

```console
sudo pkg install gmake pkgconf cairo pango wayland wayland-protocols \
libX11 libXext libXfixes libXinerama libXrandr libXi libXt libconfig
```

### Minimal dependencies (X11-only build)

If you only need the X11 backend, install:

```console
sudo pkg install gmake pkgconf cairo pango \
libX11 libXext libXfixes libXinerama libXrandr libXi libXt libconfig
```

Build with X11 only:

```console
gmake backends=x11
```

Build with GNU Make:

```console
gmake
```

## OpenBSD

### Dependencies

Use `pkg_add` to install build tools and runtime dependencies:

```console
doas pkg_add gmake pkgconf cairo pango wayland wayland-protocols libconfig
```

### Minimal dependencies (X11-only build)

If you only need the X11 backend, install:

```console
doas pkg_add gmake pkgconf cairo pango libconfig
```

Build with X11 only:

```console
gmake backends=x11
```

Build with GNU Make:

```console
gmake
```


### Source Installation

#### Ubuntu / Debian
```console
sudo apt update
sudo apt install git make pkg-config libconfig-dev libcairo2-dev libpango1.0-dev \
libxi-dev libx11-dev x11proto-core-dev x11proto-dev libxt-dev libxext-dev \
libxfixes-dev libxinerama-dev libxrandr-dev libwayland-dev wayland-protocols
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
make
sudo make install
```

#### Fedora / RHEL / CentOS
Use the same package names on all three distributions:
```console
sudo dnf install -y git make gcc pkgconf-pkg-config cairo-devel pango-devel \
libX11-devel libXext-devel libXfixes-devel libXinerama-devel libXrandr-devel \
libXi-devel libXt-devel wayland-devel wayland-protocols-devel libconfig-devel
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
make
sudo make install
```

#### Arch Linux
```console
sudo pacman -S --needed git make pkgconf cairo pango libxi libx11 libxt libxext \
libxfixes libxinerama libxrandr wayland wayland-protocols libconfig
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
make
sudo make install
```

#### NixOS / Nix
Use the flake development shell to compile from source:
```console
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
nix develop -c make
sudo nix develop -c make install
```

#### Gentoo
Install required development libraries, then build directly from source:
```console
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
make
sudo make install
```

## Windows

### Source Installation (MSYS2)

```console
pacman -S --noconfirm git make pkgconf pactoys
pacboy -S --noconfirm gcc:p
git clone https://github.com/wenyinos/activate-linux
cd activate-linux
export backends=gdi
make
```

The resulting executable is generated in the repository root (for example `activate-windows64.exe`).


## MacOS (Horrific)

### Dependencies

Use MacPorts to install the following, then build normally.

- `xorg-server`
- `cairo`
- `xorg-libXinerama`

Alternatively, you can use [this](https://github.com/Lakr233/ActivateMac) project instead because it actually works properly.
