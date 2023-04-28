---
title: 'Important update for users of dwmblocks-royarg-git'
date: 2022-07-07T00:00:00+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - packages
  - updates
---
[Due to a versioning mishap][1], an update to **[dwmblocks-royarg-git][2]** may
display the following message from `pacman`:

```sh
warning: dwmblocks-royarg-git: local (1.0.8f56741-1) is newer than royarg-repo (1.0.r75.8f56741c-1)
```

It is recommended to upgrade anyway by reinstalling the package, regardless of
`pacman` complaining about a downgrade:

```sh
$ sudo pacman -S dwmblocks-royarg-git
warning: downgrading package dwmblocks-royarg-git (1.0.8f56741-1 => 1.0.r75.8f56741c-1)
resolving dependencies...
looking for conflicting packages...

Package (1)                       Old Version    New Version         Net Change  Download Size

royarg-repo/dwmblocks-royarg-git  1.0.8f56741-1  1.0.r75.8f56741c-1    0.00 MiB       0.01 MiB

Total Download Size:   0.01 MiB
Total Installed Size:  0.02 MiB
Net Upgrade Size:      0.00 MiB

:: Proceed with installation? [Y/n]
```

[1]: {{< get-profile "GitHub" >}}/dwmblocks-royarg-git/commit/c22f884e4e483d5be882fae97b3925209135545e
[2]: {{< get-profile "GitHub" >}}/dwmblocks-royarg-git
