---
title: 'This website is now more than just HTML tags'
linkTitle: 'This website is now more than just `HTML` tags'
date: 2023-12-31T11:26:56+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - hugo
  - updates
aliases:
  - /writing/website-update
---
I took the better part of this year to tinker around with this website. Before
that, this website used to be made out of manually written `HTML` tags, which
included [entire blogs][1]. Although it was manageable for small blogs, writing
[long blogs][2] would have been a nightmare. I [planned][3] to update and move
my previous articles from Medium over here, so it was inevitable.

So I migrated the entire site to [Hugo][4]. It is a static site generator
written in Go. I didn't had any experience with other site generators at all so
there is no rhyme or reason why I decided to go with this one, but I very much
liked the experience I got while writing articles in plain markdown. I also
liked that it:

- is a single binary to install, with no other dependencies.
- comes out of the box with tagging, RSS, and redirects with minimal setup.
- has scripting-like capabilities to keep the templates DRY.
- supports minification and fingerprinting.

Most importantly, building the site itself is _very fast_.

Another thing I did after the migration was split the general layout of the
website along with the color scheme into a [separate theme][5], in its own repo.
That way I can use the same visual look across other sites I might port to Hugo.

[1]: https://github.com/royarg02/RoyARG02.github.io/blob/4294b27f90c5a46c26ae9e87246101df6cac6044/blogs/packages.html
[2]: {{< ref "flutter-dev-cycle-demo.md" >}}
[3]: {{< ref "actual-blogging.md" >}}
[4]: https://gohugo.io
[5]: {{< get-profile "GitHub" >}}/rugo/
