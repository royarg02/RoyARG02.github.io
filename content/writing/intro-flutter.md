---
title: 'The development cycle of Flutter: An introduction'
date: 2020-04-08T00:00:00+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - dart
  - development
  - flutter
---
>> This article was previously published on [Medium][1]. It has been updated
with changes introduced to Flutter for the last 3 years.

Having experience in programming for about 7 years in various languages limited
to consoles, I always wanted to explore software development beyond the black
screen. One area that particularly interested me, was Android and its app
ecosystem. I had been meaning to start mobile development for quite a few years,
but the development processes then were a bit overwhelming to me.

And then I came across [Flutter][2], not even a year ago. Honestly, its
not-so-difficult learning curve, ease of building custom designs and quick
development caught the attention of the designer in me and intrigued the
programmer in me, and I was instantly hooked. If you had asked me about Flutter
about a year ago(from the time of writing this article), I wouldn’t have even
had the _slightest_ idea.

## Differences from native frameworks

What makes Flutter different from native frameworks? Here are some points to
consider:

- Flutter uses a [declarative approach to defining UI][3], inspired by React.
This is in contrast to the imperative approach used by native frameworks. This
effectively takes the burden of managing transitions of moving from one state of
UI to another from the developers to the framework.
- Flutter provides ["hot-reload"][4] functionality, making iteration during
development less of a chore. This is in contrast to native where (generally) a
full rebuild of the application is required to view changes.
- The entire framework down to the [engine API][5] is written in a [single
language][6], requiring _zero_ context switching in [most cases][7] between
different languages, and provides an easier pathway to implement different
[design][8] [languages][9]. This is in contrast to native using multiple
formats/languages for business logic and UI.
- Flutter is [open source][10], which includes excellent documentation following
[strict guidelines][11], enabling one to dig deeper into the actual guts of the
framework. A viable alternative to offline documentation.

{{< tweet user="remi_rousselet" id="1247692103808925697" >}}

## The elephant in the room: Hot Reload

{{< figure
  src="/images/intro-flutter/low-effort-hot-reload.webp"
  alt="Demonstrating hot-reload though a meme"
  width="100%"
  loading="lazy"
>}}

So what is the uniqueness of hot reload that makes it worthy of having a
separate section? It might be just the thing you need to escape compiling take
the greater share of development and gain _ultra-fast development_. Hot Reload
makes changes take effect **moments after you press a button** and continue. By
default, it also preserves the current state of the app.

{{< video
  src="/videos/intro-flutter/hot-reload.webm"
  type="video/webm"
  width="100%"
  caption="Hot reloading makes the changes take effect immediately without losing state"
>}}

This “sub-secondness” doesn’t end with adding or removing only a few elements
and changing some properties; entire screens can be added, removed, or swapped
with another.

This is one of the consequences of Flutter using [Dart][6], a language made by
Google that includes a Dart Virtual Machine with Just in Time (JIT) compiler
which supports incremental compilation. An abstract explanation of the hot
reload process follows:

{{< figure
  src="/images/intro-flutter/hot-reload-process.webp"
  alt="When the user requests a hot-reload, the VM reloads the changed files and forces a widget rebuild"
  caption="A crude representation of the hot reload process in Flutter"
  width="100%"
  class="figure-smaller"
  loading="lazy"
>}}

- Upon making changes and hot reloading, the Dart VM reloads (or injects) the
libraries(files) that have been changed into the heap memory, replacing them
with the older versions to be taken care of by the garbage collector.
- Upon reloading, the VM rebuilds incorporating any added/removed elements of
the app(this may consist of a single element or multiple elements).
- After that, the VM forces the rendering engine to produce a new frame having
the changes displayed at the output.

This functionality is not limited to Flutter itself, as the following tweet shows:
{{< tweet user="passy" id="1223583561049047040">}}

Enough talk, head on over to this [article demonstrating the dev cycle in practice][12].

[1]: https://royarg.medium.com/the-development-cycle-of-flutter-a-demonstration-a108f8064114
[2]: https://flutter.dev
[3]: https://docs.flutter.dev/get-started/flutter-for/declarative
[4]: https://docs.flutter.dev/development/tools/hot-reload
[5]: https://github.com/flutter/engine/blob/main/lib/ui/ui.dart
[6]: https://dart.dev
[7]: https://docs.flutter.dev/development/platform-integration/platform-channels
[8]: https://github.com/macosui/macos_ui
[9]: https://github.com/bdlukaa/fluent_ui
[10]: https://github.com/flutter/flutter
[11]: https://github.com/flutter/flutter/wiki/Style-guide-for-Flutter-repo#documentation-dartdocs-javadocs-etc
[12]: {{< ref "flutter-dev-cycle-demo.md" >}}
