---
title: 'Removing lf-royarg-bin from royarg-repo'
linktitle: 'Removing `lf-royarg-bin` from `royarg-repo`'
date: 2024-02-21T16:04:46+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
categories:
tags:
  - package-maintaining
  - royarg-repo
  - updates
---

The package [`lf-royarg-bin`][1] was brought in [`royarg-repo`][2] due to
official packages lacking a proper `.desktop` file, even though it was available
[upstream][3]. That file is required to use `lf` as a default file manager.

Since then the `.desktop` file is being included in official packages, making
this unofficial package useless.

Following [deprecation][4] which rendered this into an empty package, it has now
been dropped from `royarg-repo`.

To continue using `lf` install the package from [official ArchLinux repositories][5].

[1]: {{< get-profile "GitHub" >}}/royarg-repo-pkgbuilds/tree/master/lf-royarg-bin
[2]: https://royarg02.github.io/royarg-repo
[3]: https://github.com/gokcehan/lf/blob/master/lf.desktop
[4]: {{< get-profile "GitHub" >}}/royarg-repo-pkgbuilds/commit/b25b5e38132099bcced098993e4ea3dd04da8046
[5]: https://archlinux.org/packages/extra/x86_64/lf/
