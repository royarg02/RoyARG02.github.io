---
title: 'The one thing most tutorials for building Arch Linux packages miss'
date: 2022-05-28T00:00:00+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - linux
  - packages
aliases:
  - /blogs/build-from-chroot
  - /blogs/build-from-chroot.html
---
I've recently begun [packaging my custom builds of known software][1] I use on
my Linux setup.

Creating packages for Arch-based distros requires a description file called
[PKGBUILD][2], which lists several metadata. Two of the important ones are
`depends` and `makedepends`.

- **`depends`**: Packages or libraries the package depends on to _run_. In other
words, the runtime dependencies.
- **`makedepends`**: Packages or libraries the package depends on to _build_. In
other words, the compile-time dependencies.

These two are often overlooked in part by PKGBUILD writers. Because, "hey, it
built and ran fine on _my_ machine!"

Don't do that.

The dependencies might be already present on your system, either explicitly
installed or brought in as a dependency by another package. As such, you
wouldn't know what exactly will be required by other setups.

A way to determine the dependencies would be to test the PKGBUILDs on a fresh
install. But using a separate system with only `base` packages and then adding
the dependencies one at a time can be a pain.

An alternative is to make them in [chroot][3]. Think of it as containerised Arch
Linux system with no access to the files(including packages) on your host
system. The [ArchWiki has a guide on this][4], so go over there for the
tutorial. Let me show you what it would look like in practice.

Take the example of the [slock-royarg-git][5] package.  Below are its
[dependencies][6]:

```sh
depends=('libxcrypt' 'libxrandr' 'libxinerama' 'libxft')
makedepends=('git')
```

If we remove all dependencies(except `git`, as that is required to fetch the
source in the first place), and try to build, we will hit an error:

```sh
slock.c:20:10: fatal error: X11/extensions/Xrandr.h: No such file or directory
20 | #include <X11/extensions/Xrandr.h>
   |          ^~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
make: *** [Makefile:19: slock.o] Error 1
```

So we can figure out the compile-time dependencies, but what about the runtime
dependencies? We run:

```sh
makechrootpkg -c -n -r $CHROOT
```

The **`-n`** flag additionally runs [`namcap`][7], which checks for packaging
issues. If we miss a runtime dependency, `namcap` will produce the following
messages:

```sh
slock-royarg-git E: Dependency libxft detected and not included (libraries ['usr/lib/libXft.so.2'] needed in files ['usr/bin/slock'])
slock-royarg-git E: Dependency libxcrypt detected and not included (libraries ['usr/lib/libcrypt.so.2'] needed in files ['usr/bin/slock'])
slock-royarg-git E: Dependency libxrandr detected and not included (libraries ['usr/lib/libXrandr.so.2'] needed in files ['usr/bin/slock'])
slock-royarg-git E: Dependency libxinerama detected and not included (libraries ['usr/lib/libXinerama.so.1'] needed in files ['usr/bin/slock'])
```

So you've now figured out the package dependencies and made sure that it will
build and run as expected on other systems.

[1]: {{< ref "packages.md" >}}
[2]: https://wiki.archlinux.org/title/PKGBUILD
[3]: https://wiki.archlinux.org/title/Chroot
[4]: https://wiki.archlinux.org/title/DeveloperWiki:Building_in_a_clean_chroot
[5]: {{< get-profile "GitHub" >}}/slock-royarg-git
[6]: {{< get-profile "GitHub" >}}/slock-royarg-git/blob/8c22c966396c0d3f756890c149a777bc50a65802/PKGBUILD#L10-L11
[7]: https://wiki.archlinux.org/title/Namcap
