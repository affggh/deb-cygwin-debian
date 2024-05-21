Deb Cygwin
===========

> [!NOTE]
>
> This cross-compiler suite is now also available on MSYS2 (unofficial) via [msys2-cygwin][msys2-cygwin],
> feel free to check it out if you don't have a Linux install.

This is a [makedeb][makedeb] port of [arch-cygwin][arch-cygwin] for using the Cygwin cross toolchain on Ubuntu / Debian.

Currently, this repository contains some experimental PKGBUILDs to build a Cygwin cross compiler.

And possibly more packages (or some build tool wrappers) can be added in the future.

## Install

> [!NOTE]
>
> Currently only the latest x86_64 LTS release of Ubuntu and Debian is supported, if you want to build on other distros and architectures,
> which is untested, you can try to follow the [Bootstrapping](#bootstrapping) instructions to build the cross compilers yourself.
>
> Please feel free to open an [Issue](../../issues) if you run into any problem.

All packages are precompiled and available for downloading at [GitHub Releases](../../releases),

to use them, add the following lines to the end of your `/etc/apt/sources.lsit` (note pkgs are currently not signed) and sync the database using `apt update`:

```text
deb [trusted=yes] https://github.com/affggh/deb-cygwin-debian/releases/download/snapshot-deb ./
```

Then you should be able to install packages like this: `apt install cygwin-gcc`

### Building

> [!IMPORTANT]
>
> Parallel builds might break things sometimes, use a single job to be safe.
>
> You can do this by editing your `/etc/makepkg.conf` and remove `-jXX` from `MAKEFLAGS` (if any).

First, clone this repository using Git and make sure you have installed `build-essential`, `texinfo`, `automake` and `bison` before proceeding.

Get the [makedeb][makedeb] tool, and then simply run `makedeb -si` in a subdirectory in order to build and install that package.

#### Bootstrapping

When bootstrapping the cross compilers, build and install the following packages one by one, and in this order:

  * cygwin-binutils
  * cygwin-default-manifest-bin
  * cygwin-w32api-headers
  * cygwin-w32api-runtime-bin
  * cygwin-bin
  * cygwin-gcc
  * cygwin-default-manifest (replaces the `-bin` one)
  * cygwin-w32api-runtime (replaces the `-bin` one)
  * cygwin (replaces the `-bin` one; requires `cocom` to build)
  * cygwin-gcc (rebuild to link against our just-built libraries)

After that other packages can be built and installed normally ;)

##### See also

[Cygwin documentation: Building a cross-compiler](https://x.cygwin.com/docs/cg/cross.html)

[Cygwin cross-compilers on MSYS2 (unofficial)](https://github.com/ookiineko-cygport/msys2-cygwin.git)

## Contributing

Please refer to [ArchWiki: MinGW package guidelines](https://wiki.archlinux.org/title/MinGW_package_guidelines).

## Credits

Special thanks to the following external sources where arch-cygwin took references or borrowed code from (unsorted):

  * [cygwin-packages](https://cygwin.com/cgit/cygwin-packages) (For Cygwin quirks and patches!)

  * [MinGW-w64 packages on AUR](https://aur.archlinux.org/packages?K=mingw-w64) (For various kinds of wrappers, and packaging rules!)

  * [msys2/MINGW-packages](https://github.com/msys2/MINGW-packages) (For LLVM build quirks)

  * [Fedora Cygwin](https://copr.fedorainfracloud.org/coprs/yselkowitz/cygwin/) (For Cygwin quirks when cross compiling)

  * [archlinux packaging](https://gitlab.archlinux.org/archlinux/packaging/packages)

  * [yselkowitz's Fedora GIT](https://fedorapeople.org/cgit/yselkowitz/)

<!-- References: -->
[makedeb]: https://makedeb.org
[arch-cygwin]: https://github.com/ookiineko/arch-cygwin.git
[msys2-cygwin]: https://github.com/ookiineko-cygport/msys2-cygwin.git
