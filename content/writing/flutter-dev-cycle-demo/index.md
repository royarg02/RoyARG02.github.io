---
title: 'The development cycle of Flutter: the demonstration'
linkTitle: 'The development cycle of Flutter: _the_ demonstration'
date: 2020-04-08T00:01:00+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - beginner
  - dart
  - development
  - flutter
  - guide
---
>> This article was previously published on [Medium][1]. It has been updated
with changes introduced to Flutter for the last 3 years.

To demonstrate the development cycle of Flutter, this "guide" shows the steps
required to build a dice roller app.

## What are we building?

Our app, after completion, would somewhat look like the following:

{{< video
  src="app-complete.mp4"
  type="video/mp4"
  width="100%"
  class="figure-smaller"
>}}

As demonstrated, it consists of the following controls:

- A control to roll the dice(display a random image of a dice face). The image
of the dice face would depend upon a random number between 1 and 6.
- A control to “reset” the dice(display nothing/a white space image). It would
display only if the random number is reset to zero.
- For the sake of completion, a control to simply count up the faces of the
dice, up to 6. The image of the dice would change accordingly.

Let's get started.

## Building the dice roller

### Step 0: Making sense of the UI

Taking a look at the UI, we see the following elements to be included:

- An AppBar
- An Image
- 3 Buttons

Additionally, a SnackBar has to be invoked when we press the “Roll” button.

```goat {class=figure-smaller}
─AppBar
─Image
─Button
─Button
─Button
─SnackBar
```

Grouping the elements which display in the “background” of the app as the “body”
of the app, we get the following arrangement:

```goat {class=figure-smaller}
─AppBar
─Body
  ├──Image
  ├──Button
  ├──Button
  ├──Button
  └──SnackBar
```

Now, an element in Flutter that takes in an AppBar and a Body is a Scaffold,
thus we can continue the arrangement of the elements as:

```goat {class=figure-smaller}
─Scaffold
  ├──AppBar
  └──Body
      ├──Image
      ├──Button
      ├──Button
      ├──Button
      └──SnackBar
```

Since we are building an application that would contain these elements, wrap
these elements under a **MaterialApp**…

```goat {class=figure-smaller}
─MaterialApp
  └──Scaffold
      ├──AppBar
      └──Body
          ├──Image
          ├──Button
          ├──Button
          ├──Button
          └──SnackBar
```

…and we have the widget tree of the application.

Since Flutter doesn’t have a separate language to define the layout,
thinking instead about the widget tree is the first thing a Flutter developer
would probably do when building an app. Experienced developers would typically
break layouts into boxes and go straight to coding once getting a hang of this.

Now as we have the widget tree figured out, let’s get to actual coding.

### Step 1: Implementing the widget tree

Here’s (more or less) the widget tree above translated into Flutter (along with
some more code):

```dart {lineNos=inline,tabWidth=2}
import 'package:flutter/material.dart';

void main() {
  runApp(const App());
}

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dice Roller',
      home: Scaffold(
        appBar: AppBar(),
        body: const Body(),
      ),
    );
  }
}
```

The import statement includes all the elements(said as _widgets_ from here) to
be used in the app as the `material.dart` package, which contains the widgets
implementing the [**Material Design**][2] system. To build an app following
Apple's [**Human Interface**][3] guidelines, especially for iOS and macOS,
import `cupertino.dart`.

Next up, it uses the `main()` function, which is the entry point of execution of
any Dart program. This function calls another function [`runApp()`][4] which, as
the name suggests, runs an “app”, which is the argument passed to the function;
the `App()` object.

The `const` keyword denotes that the `App()` object passed to `runApp()` is a
**compile-time constant**. Specifying widgets as `const` is a recommended
convention that helps with performance of the app.

`App()` is nothing but an object of a simple Dart class, which behaves as a
widget by inheriting from a special class [`StatelessWidget`][5]. These widgets
**do not have a state**, which would imply that they _never_ change their
configuration in the app, be it through appearance or functionality.

The constructor of `App` defines an **optional named parameter** named `key`
which it passes to its superclass. Defining this parameter is also recommended,
even if we don't use it in this case.

The `App` class overrides `build()`, which returns the widgets which are _under_
the current widget in the widget tree(in other words, these widgets define the
composition of the `App()` widget), to be displayed on the screen. The
[`BuildContext`][6] object it takes denotes its location in the widget tree.
Here, the function returns a [`MaterialApp`][7], whose argument to the `home`
parameter is the widget we wish to see upon launch.

The [`Scaffold`][8] widget provides a template for building any screen of an app
following the Material Guidelines. It provides various placeholders, of which,
we use two, `appBar` with an [`AppBar`][9] and `body`.

Trying to run the app now would result in an error, as flutter does not have a
widget named `Body`.

### Step 2: Defining the `Body` widget

```dart {lineNos=inline,tabWidth=2}
class Body extends StatefulWidget {
  const Body({super.key});

  @override
  State<Body> createState() {
    return _BodyState();
  }
}

class _BodyState extends State<Body> {
  @override
  Widget build(BuildContext context) {
    //TODO: Define the body
  }
}
```

`Body` is defined as a subclass of [`StatefulWidget`][10], which implies that
this widget **has a state**, upon which the appearance or behaviour of the
widget depends. The **state may change** on any point of time during the
lifecycle of the app, for instance due to a user interaction or the completion
of a task. The state of the widget is defined as another class; here as
`_BodyState`, whose structure looks very similar to the definition of any
StatelessWidget. An object of `_BodyState` is returned in the `createState()`
function of the `Body` class.

One more thing to note here. Putting an underscore "`_`" in front of any
identifier in Dart makes it **private** for the file (or library), which means
that **it is inaccessible outside the file**. As the state of any
`StatefulWidget` is only useful for the widget itself, making it private is
generally a good idea.

### Step 3: Building the body of the app

Referring back to Step 0, we need to implement an Image, and 3 buttons for the
body, _arranged vertically_.

In Flutter, a widget that vertically lays widgets is [`Column`][11]. It takes a
list of widgets to be placed vertically one under the other as its children and
aligns them according to some arguments.

In our case, Column could be used as:

```dart {lineNos=inline,tabWidth=2,hl_lines="4-7"}
class _BodyState extends State<Body>{
  @override
  Widget build(BuildContext context){
    return const Column(
      children: <Widget>[
        // TODO: Define children
      ],
    );
  }
}
```

Let us try running the app now.

{{< video
  src="step-3.mp4"
  type="video/mp4"
  width="100%"
>}}

We wouldn’t have to wait this long next time for applying changes.

### Step 4: Defining the functions to be invoked on button press

To use buttons, we need to specify what we want the app to do when the buttons
are pressed.

The three buttons of our app are supposed to do these three things:

- Provide a random number between 1 and 6.
- Reset the current number to 0.
- Increment the current number up to 6.

Since this "current number", which is the face value of the dice, is subject to
change upon user interaction, we can consider it to represent the "state" of the
app. Since a `StatefulWidget` is already being used here, we can define a state
variable "`_dice`" as a member of the state class to hold the current number,
and provide its initial value as "`0`" signifying that the dice hasn't been
rolled yet.

```dart {lineNos=inline,tabWidth=2,hl_lines=2}
class _BodyState extends State<Body>{
  int _dice = 0;
  @override
  Widget build(BuildContext context){
  ...
```

Now import the following library:

```dart {lineNos=inline,tabWidth=2}
import 'dart:math' as math;
```

This [library][12] has the usual mathematical functions and constants, along
with a class to create a **random number generator**. The `math` alias is used
here to simplify some code to be written later.

Let us define the random number generator:

```dart {lineNos=inline,tabWidth=2}
var r = math.Random(42);
```

The `math.Random()` function takes an integer as the seed to generate numbers.

Now we define the required functions for the controls as members of
`_BodyState`:

```dart {lineNos=inline,tabWidth=2}
void _rollDice(){
  setState((){
    _dice = r.nextInt(6) + 1;
  });
}

void _countUp(){
  setState((){
    _dice = _dice < 6 ? _dice + 1 : _dice;
  });
}

void _resetDice(){
  setState((){
    _dice = 0;
  });
}
```

The [`nextInt()`][13] method of the Random object gives a number from 0 up to
the argument (exclusive). So to get a number between 1 and 6, we get a number
from 0 and 5 and add 1.

Each of these functions call the [`setState()`][14] function, which "notifies
the framework of a change in the state". The change is made in a function
provided as an argument. The widget tree is then rebuilt reflecting the changes.
Changing the state itself without calling `setState()` will not make the
framework rebuild the tree.

### Step 5: Defining the buttons

For buttons, we use the [`ElevatedButton`][15] widget, which among many, takes
two parameters: `child` and `onPressed`. The first parameter is typically used
to display the label, using a [`Text`][16] widget. The latter takes the function
which would be invoked every time the button is pressed.

```dart {lineNos=inline,tabWidth=2,hl_lines="7-18"}
class _BodyState extends State<Body>{
  int _dice = 0;
  @override
  Widget build(BuildContext context){
    return Column(
      children: <Widget>[
        ElevatedButton(
          onPressed: _rollDice,
          child: const Text('Roll'),
        ),
        ElevatedButton(
          onPressed: _countUp,
          child: const Text('Count up'),
        ),
        ElevatedButton(
          onPressed: _resetDice,
          child: const Text('Reset'),
        ),
      ],
    );
  }
}
```

Since functions are treated as _first-class objects_ in Dart, you can pass
functions to the `onPressed` parameter of each `ElevatedButton` just like any
other object. But they are not constants, so the `Column` can't be declared as
`const` anymore, but each of the labels of the buttons can.

{{< video
  src="step-5.mp4"
  type="video/mp4"
  caption="To hot reload, hit `r` in the current terminal session of `flutter run`"
  width="100%"
>}}

### Step 6: Importing offline assets

We have the following images to display the dice faces:

{{< figure
  src="dice-images.jpg"
  alt="The dice images to be used in the app"
  caption="The dice images to be used in the app"
  width="100%"
  loading="lazy"
>}}

To add them the app, let’s create a folder to put them in:

{{< figure
  src="assets-location.jpg"
  alt="The assets are put in a newly created \"assets\" folder in the root of the project"
  caption="The assets are put in a newly created \"assets\" folder in the root of the project"
  width="100%"
  class="figure-smaller"
  loading="lazy"
>}}

To use these images in the app, they need to be “imported”, but not like any
library file. Enter…

#### pubspec.yaml

This file provides information about various dependencies required to build the
app. This includes any offline assets.

By default, there are commented statements as guides to use the assets:

{{< figure
  src="asset-comments.jpg"
  alt="The commented lines guiding to specify asset dependencies"
  width="100%"
  loading="lazy"
>}}

Likewise, with proper indentation, add to `pubspec.yaml`:

```yaml {lineNos=inline,tabWidth=2}
assets:
 - assets/dice_1.png
 - assets/dice_2.png
 - assets/dice_3.png
 - assets/dice_4.png
 - assets/dice_5.png
 - assets/dice_6.png
 - assets/empty_dice.png
```

That’s it! After saving, these assets would be available through their location
in the project folder.

### Step 7: Using offline assets

To use images, Flutter provides the [`Image`][17] widget. As in this case the
images are obtained from the bundled assets, we use the [`Image.asset()`][18]
constructor. You can explicitly set the dimensions of the image using the
`height` and `width` parameter.

```dart {lineNos=inline,tabWidth=2,hl_lines="7-11"}
class _BodyState extends State<Body>{
  int _dice = 0;
  @override
  Widget build(BuildContext context){
    return Column(
      children: <Widget>[
        Image.asset(
          'assets/dice_1.png',
          height: 300,
          width: 300,
        ),
        ElevatedButton(
          onPressed: _rollDice,
          child: const Text('Roll'),
        ),
        ElevatedButton(
          onPressed: _countUp,
          child: const Text('Count Up'),
        ),
        ElevatedButton(
          onPressed: _resetDice,
          child: const Text('Reset'),
        ),
      ],
    );
  }
}
```

{{< video
  src="step-7.mp4"
  type="video/mp4"
  width="100%"
>}}

### Step 8: Changing image on the dice roll

We define a function that returns the location of the image depending on the
value of `_dice`:

```dart {lineNos=inline,tabWidth=2}
String determineImage(int value) {
  if (value == 0) {
    return 'assets/empty_dice.png';
  }
  return 'assets/dice_$value.png';
}
```

We use the naming scheme followed by the assets to our advantage, and use the
"value" of `value` itself to derive the exact name of asset the app needs to
show by using [string interpolation][19].

Now we use the function in the `Image` widget:

```dart {lineNos=inline,tabWidth=2,hl_lines=2}
Image.asset(
  determineImage(_dice),
  height: 300,
  width: 300,
),
```

{{< video
  src="step-8.mp4"
  type="video/mp4"
  width="100%"
>}}

Since the widget tree is already being rebuilt using `setState()` in the
`onPressed` handler of the buttons, any changes will automatically take effect.

### Step 9: Implementing the SnackBar

The snackbar in the app is shown when we press the **Roll** button. To do that
we modify the `_rollDice()` function:

```dart {lineNos=inline,tabWidth=2,hl_lines="6-11"}
void _rollDice(){
  setState((){
    _dice = r.nextInt(6) + 1;
  });

  ScaffoldMessenger.of(context).showSnackBar(
    const SnackBar(
      content: Text('Rolling...'),
      duration: Duration(milliseconds: 300),
    )
  );
}
```

The new statement in the function uses the [`ScaffoldMessenger`][20] to
display a [`SnackBar`][21] with the provided label as `content` and the amount
of time the `SnackBar` should stay visible on the screen as the `duration`
parameter.

{{< video
  src="step-9.mp4"
  type="video/mp4"
  width="100%"
>}}

Now, even if the app generates the same random number, you can be _sure_ that
you rolled the dice.

### Step 10: Finishing touches

Our app is mostly done, except

- aligning the image and the buttons, and provide some space between them.
- adding a title to be displayed on the appbar.
- change the default colors!

Going back to the Column widget for vertical alignment:

```dart {lineNos=inline,tabWidth=2,hl_lines=2}
return Column(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: <Widget>[
```

Passing this argument  aligns the children of the `Column` with the remaining
space evenly divided between them along the **main axis**(i.e., vertically).

For centering the column horizontally, wrap it with the [`Center`][22] widget.
To make the IDE work for you, hit `Ctrl + .` in **Visual Studio Code** or `Alt +
Return` in **Intellij IDEA**/**Android Studio** while placing the cursor over
`Column` and bring up a handy menu to do this(and many more) job with a single
button press!

```dart {lineNos=inline,tabWidth=2,hl_lines=1}
return Center(
  child: Column(
```

For the second point, we provide the `AppBar` a `title`:

```dart {lineNos=inline,tabWidth=2,hl_lines=2}
appBar: AppBar(
  title: const Text('Dice Roller'),
),
```

To change the default colors, we use the newly supported [**Material 3**][23]
(also known as "Material You") design system to generate a complete
color scheme for our app with a single seed color:

```dart {lineNos=inline,tabWidth=2,hl_lines="9-12"}
return MaterialApp(
  title: 'Dice Roller',
  home: Scaffold(
    appBar: AppBar(
      title: const Text('Dice Roller'),
    ),
    body: const Body(),
  ),
  theme: ThemeData(
    useMaterial3: true,
    colorSchemeSeed: Colors.red,
  ),
);
```

{{< video
  src="step-10.mp4"
  type="video/mp4"
  width="100%"
>}}

## What did we built

{{< details "Click to reveal the complete source code." >}}

```dart {lineNos=inline,tabWidth=2}
import 'dart:math' as math;
import 'package:flutter/material.dart';

var r = math.Random(42);

void main() {
  runApp(const App());
}

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dice Roller',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Dice Roller'),
        ),
        body: const Body(),
      ),
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.red,
      ),
    );
  }
}

class Body extends StatefulWidget {
  const Body({super.key});

  @override
  State<Body> createState() {
    return _BodyState();
  }
}

class _BodyState extends State<Body> {
  int _dice = 0;
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: <Widget>[
          Image.asset(
            determineImage(_dice),
            height: 300,
            width: 300,
          ),
          ElevatedButton(
            onPressed: _rollDice,
            child: const Text('Roll'),
          ),
          ElevatedButton(
            onPressed: _countUp,
            child: const Text('Count up'),
          ),
          ElevatedButton(
            onPressed: _resetDice,
            child: const Text('Reset'),
          ),
        ],
      ),
    );
  }

  void _rollDice() {
    setState(() {
      _dice = r.nextInt(6) + 1;
    });

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Rolling...'),
        duration: Duration(milliseconds: 300),
      )
    );
  }

  void _countUp() {
    setState(() {
      _dice = _dice < 6 ? _dice + 1 : _dice;
    });
  }

  void _resetDice() {
    setState(() {
      _dice = 0;
    });
  }

  String determineImage(int value) {
    if (value == 0) {
      return 'assets/empty_dice.png';
    }
    return 'assets/dice_$value.png';
  }
}
```

{{< /details >}}

In a matter of simple 10 steps, we created:

- A beautiful application, courtesy of Material You design language.
- An application guaranteed to run in 60 fps natively.
- An application which runs on Android, iOS, Linux, Windows, macOS, and the web.
- Likewise, an application ready to be deployed on any environment of your
choice, if you are able to port the flutter engine there first.

Also, we've gone through mostly everything the development of a Flutter app
involves, including widget refactoring, using custom widgets, bringing in
assets, a bit of state management, some theming, and not to mention, using Hot
Reload.

All from a single codebase.

What I showcased was a sliver of what Flutter could provide you in app
development. If you are interested, [visit the website][24] and give it a spin,
[read the docs][25], go through the [framework source code][26], read some more
articles and watch tutorial videos. If this article didn’t convince you, those
would definitely will.

Best of luck for your future Flutter endeavours!

[1]: https://royarg.medium.com/the-development-cycle-of-flutter-a-demonstration-a108f8064114
[2]: https://material.io
[3]: https://developer.apple.com/design/human-interface-guidelines/guidelines/overview
[4]: https://api.flutter.dev/flutter/widgets/runApp.html
[5]: https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html
[6]: https://api.flutter.dev/flutter/widgets/BuildContext-class.html
[7]: https://api.flutter.dev/flutter/material/MaterialApp-class.html
[8]: https://api.flutter.dev/flutter/material/Scaffold-class.html
[9]: https://api.flutter.dev/flutter/material/AppBar-class.html
[10]: https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html
[11]: https://api.flutter.dev/flutter/widgets/Column-class.html
[12]: https://api.dart.dev/stable/dart-math/dart-math-library.html
[13]: https://api.dart.dev/stable/dart-math/Random/nextInt.html
[14]: https://api.flutter.dev/flutter/widgets/State/setState.html
[15]: https://api.flutter.dev/flutter/material/ElevatedButton-class.html
[16]: https://api.flutter.dev/flutter/widgets/Text-class.html
[17]: https://api.flutter.dev/flutter/widgets/Image-class.html
[18]: https://api.flutter.dev/flutter/widgets/Image/Image.asset.html
[19]: https://dart.dev/language/built-in-types#strings
[20]: https://api.flutter.dev/flutter/material/ScaffoldMessenger-class.html
[21]: https://api.flutter.dev/flutter/material/SnackBar-class.html
[22]: https://api.flutter.dev/flutter/widgets/Center-class.html
[23]: https://docs.flutter.dev/development/ui/material
[24]: https://flutter.dev/
[25]: https://api.flutter.dev/
[26]: https://github.com/flutter/flutter
