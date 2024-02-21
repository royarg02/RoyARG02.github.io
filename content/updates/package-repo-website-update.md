---
title: 'royarg-repo has been migrated to Hugo'
linktitle: '`royarg-repo` has been migrated to Hugo'
date: 2024-01-22T21:38:03+05:30
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - hugo
  - royarg-repo
  - updates
aliases:
  - /writing/package-repo-website-update
---

After [migrating my portfolio website][1] to Hugo, the logical next candidate
for the migration was [the website of my unofficial package repository][2],
which at the time was a simple **GitHub Pages** website, only displaying the
`README.md` file.

The migration had another motivation as well. I wanted the site to have a list
of the packages it offers, typical of an [_actual_ package repository][3]:

{{< figure
  src="/images/package-repo-website-update/package-repo.jpg"
  alt="A package repository typically lists the package files similar to a file server"
  caption="This is what shows up when you visit https://arch.kyberorg.fi/core/os/x86_64/ on a web browser"
  width="100%"
  loading="lazy"
>}}

Although that is already "available" at [the GitHub repository][4] where the
packages are stored, I figured that is worth having for better UX, keeping users
at the site rather than sifting through GitHub.

Migration of that website took way less effort than migrating my portfolio
website because of the [theme][5] I had created to share the look across other
websites. Most effort was spent writing the [list template][6], to display the
package files stored in the `static` directory, and [a bit of `CSS`][7], to make
the file list behave responsibly across screen sizes.

This migration ends at the browser; regular `pacman` users of this repository
should not notice anything out of the ordinary.

[1]: {{< ref "website-update.md" >}}
[2]: https://royarg02.github.io/royarg-repo
[3]: https://arch.kyberorg.fi/
[4]: {{< get-profile "GitHub" >}}/royarg-repo
[5]: {{< get-profile "GitHub" >}}/rugo
[6]: https://github.com/royarg02/royarg-repo/blob/4c75de8446e93eb044126a3dcd02c0611a95ea88/layouts/_default/list.html
[7]: https://github.com/royarg02/royarg-repo/blob/4c75de8446e93eb044126a3dcd02c0611a95ea88/assets/css/table.css

