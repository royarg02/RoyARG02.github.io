---
title: 'I took compiling from source to the next level'
date: 2022-05-27
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - development
  - linux
  - packages
aliases:
  - /blogs/packages
---
{{< figure
  src="https://xkcd.com/comics/automation.png"
  alt="Automation comic by xkcd"
  caption="[Automation](https://xkcd.com/1319) by [xkcd](https://xkcd.com) / [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/legalcode)"
  width="100%"
  class="figure-float-right"
>}}

I use various suckless(or suckless like) software, including the [window
manager][1], the ["run" launcher][2], the [status bar][3] and the [terminal][4]
on my Linux machine.

One of the main features of these "suckless-like" software is that they have to
be built from the source since that is the only way for configuration. The
typical installation comprises these steps:

- You get the source code itself(for instance, through `git clone`),
- You modify the source code or apply patches to add features not available in
the vanilla build,
- You run `make`, and if compilation is successful, `make install` to install
the built binary.

However, these are not managed by the package manager, creating issues with
dependency management.  My build requires additional dependencies(and thus not
documented upstream), and removing them might leave their dependencies around.

Also, if I migrate my setup, I can have most of my packages installed using a
single command, thanks to a generated list from the package manager. But it
would require the "clone-make-make install" ritual for the source builds.

It led me to consider making binary packages of my custom suckless software,
requiring me to write a package description file(also known as [PKGBUILD][5] in
Arch-based distros)...

"... and posting them to the AUR." would be the first guess for any Arch user.
However, it is possible to have these packages removed from the AUR as these are
merely user-specific builds of another software. Thus I would have to make _my_
package repository.

## Making the package repository(and automating it)

A package repository contains some package files(among others) having the
extension `pkg.tar.zst`. Navigating to a specific address based on the directory
listing of these files would provide you with the actual package file.

I figured **GitHub pages** would be best suited for it. I could have a
repository having a `os/x86_64` folder and dump all the built packages there.

So I went looking for guides for writing PKGBUILDs and creating user
repositories. But most of them rely on _manually_ building the packages,
_manually_ copying packages to the database, _manually_ building the database
repo, and _manually_ pushing the changes along with the updated packages to the
remote. Too much _manual_ interference is something I would rather not have in
my setup, and thus led me to use **GitHub Actions** for automation.

Having no experience, using it was easy due to its `YAML` syntax. I chained
together multiple actions to suit my purpose.

My source repositories can have multiple commits a day, and I wouldn't want to
trigger the action on every push.  They are instead triggered by a change to the
PKGBUILDs. Here is how it is wired, as of writing this blog:

- I push the updated source code to the source repo,
- I push the updated PKGBUILD to the package repo, triggering a build of the new
package,
- The package is then [committed][6] to the database repo,
- The database then [builds][7] itself, incorporating the new packages.

## Packages available in the repo

Currently, the following packages are available in the repository:

- **[dmenu-royarg-git][8]**
- **[dwm-royarg-git][9]**
- **[dwmblocks-royarg-git][10]**
- **[lf-royarg-bin][11]**
- **[slock-royarg-git][12]**
- **[st-royarg-git][13]**

The built package files are located [here][14].  Instructions to have these
packages are in the [README][15] or can be found directly at
<https://royarg02.github.io/royarg-repo>.

With that said, if you want to, go ahead and try those packages!

Until next time.

[1]: {{< get-profile "GitHub" >}}/dwm
[2]: {{< get-profile "GitHub" >}}/dmenu
[3]: {{< get-profile "GitHub" >}}/dwmblocks
[4]: {{< get-profile "GitHub" >}}/st
[5]: https://wiki.archlinux.org/title/PKGBUILD
[6]: {{< get-profile "GitHub" >}}/st-royarg-git/blob/master/.github/workflows/package-upload.yaml
[7]: {{< get-profile "GitHub" >}}/royarg-repo/blob/master/.github/workflows/build-database.yaml
[8]: {{< get-profile "GitHub" >}}/dmenu-royarg-git
[9]: {{< get-profile "GitHub" >}}/dwm-royarg-git
[10]: {{< get-profile "GitHub" >}}/dwmblocks-royarg-git
[11]: {{< get-profile "GitHub" >}}/lf-royarg-bin
[12]: {{< get-profile "GitHub" >}}/slock-royarg-git
[13]: {{< get-profile "GitHub" >}}/st-royarg-git
[14]: {{< get-profile "GitHub" >}}/royarg-repo
[15]: {{< get-profile "GitHub" >}}/royarg-repo/blob/master/README.md
