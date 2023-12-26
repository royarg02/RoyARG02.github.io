---
title: Flutter BLoC, from the perspective of a beginner
date: 2020-05-11T00:00:40+05:30
author: 'Anurag Roy'
license:
  name: 'CC BY‑ND 4.0'
  url: https://creativecommons.org/licenses/by-nd/4.0/legalcode
tags:
  - beginner
  - flutter
  - guide
---
>> This is an adaptation of an article previously published on [Medium][1].

When developement of applications is considered, we can think of every action
the app is supposed to perform as an _asynchronous event_ - whether it is
fetching data from the outside world, interacting with the underlying system, or
handling of user input. Keeping this in mind we can also think of the updates in
the UI due to changes in the state as an asynchronous event. This concept is
used by one of the state management patterns used in Flutter known as
"**B**usiness **Lo**gic **C**omponents" aka **BLoC**.

Fortunately, Dart, the language in which Flutter apps are written, has built-in
support for aynchronous programming, with out-of-the-box support for
[Futures][2] and [Streams][3]. But the most common way to implement this pattern
is through the [bloc][4] and [flutter_bloc][5] packages, which, although takes
care of all the boilerplate needed, made the underlying concept of the pattern
very difficult to understand for me when I was getting started.

This article is an attempt to explain this pattern in the most simplest way
possible in all its boilerplate glory.

## The basics

In a typical "reactive" application, the UI can be thought as a function of
state. The state, in turn, depends on the model. The data of the model can be
either internal or can be something brought in from the outside world.

This is not a one way street though, as UI can also make changes to the
model(usually through a controller layer), which causes the state to change and
this whole cycle continues.

```goat
 .-----------. action  .-----------. update  .-----------.
 |           +-------->|CONTROLLER +-------->|           |
 |    UI     |         .-----------.         |   MODEL   |
 |           |<--------+   STATE   |<--------+           |
 '-----------'  update '-----------' change  '-----------'
```

For updates to the UI to be managed through asynchronous events, we would need
two sets of events: one from the UI layer to the controller layer and another to
the UI layer with the updated state.

Since the events are asynchronous, they can appear at any time or not at all,
and we cannot use them directly. As before, Dart provides us with Futures and
Streams to handle asynchronous data. Since the events go in a sequence and can
be more than one during the lifecycle of the app, we can use **Streams**.

## Thinking in Streams

A stream can be thought of being a pipe. Data can be pushed to its input known
as its **sink**, and they can be received from its output(also called listening)
as its **stream**(not to be confused with the pipe "stream" itself).

```goat
          .----. .----------------------------. .----.
event in |      |                              |      | event out
-------->| sink |           STREAM             |stream+-------->
         |      |                              |      |
          '----' '----------------------------' '----'
```

To easily create a stream, this article uses a [StreamController][6], which
doesn't require any know how of streams other than above.

Going back to managing the sets of asynchronous events, we would need two
streams. In the context of BLoC, we construct a "BLoC" class, which receives
events from the UI through a stream, uses the received data to perform the
required _business logic_ with the help of the model, and then sends the
result(state) back to the UI through another stream.

```goat
 .----------------------.------------------.---------------------.
 |           send event +----- STREAM ---->|        BLoC         |
 |                      '------------------'                     |
 |          UI          |                  | state=              |
 |                      .------------------.   logic(model,event)|
 |        receive state |<---- STREAM -----+                     |
 '----------------------'------------------'---------------------'
```

## Implementing the BLoC

The "events" are typically managed using [Enum][7]s...

```dart {lineNos=inline}
enum Event {
  eventOne,
  eventTwo,
  eventThree,
}
```

...although they can also be subclasses of a [sealed][8] class:

```dart {lineNos=inline}
sealed class EventClass {}
final class EventOne extends EventClass {}
final class EventTwo extends EventClass {}
final class EventThree extends EventClass {}
```

In a BLoC, we create two streams each typed as per the type of the data passing
through. We expose the _sink_ of one for the UI to send and the _stream_ of
another for the UI to receive the data back:

```dart {lineNos=inline}
class Bloc {
  late final Model _model;
  final StreamController<Event> _eventStreamController = StreamController();
  final StreamController<String> _stateStreamController = StreamController();

  StreamSink<Event> get eventSink => _eventStreamController.sink;
  Stream<String> get stateStream => _stateStreamController.stream;
}
```

Here the code is considering the state to be of `String` type(for instance, we
need to display the formatted checkout amount of a shopping store), hence
`_stateStreamController` is a Stream typed to `String`.

Once we receive any event from `_eventStreamController`, we use the business
logic defined in the BLoC to determine the state to send back to the UI through
the _sink_ of `_stateStreamController`.

```dart {lineNos=inline,hl_lines="9-19"}
class Bloc {
  late final Model _model;
  final StreamController<Event> _eventStreamController = StreamController();
  final StreamController<String> _stateStreamController = StreamController();

  StreamSink<Event> get eventSink => _eventStreamController.sink;
  Stream<int> get stateStream => _stateStreamController.stream;

  void performLogic(Event event) {
    switch(event) {
      case Event.eventOne:
        onEventOne(_model);
      case Event.eventTwo:
        onEventTwo(_model);
      case Event.eventThree:
        onEventThree(_model);
    }
    _stateStreamController.sink.add(_model.data);
  }
}
```

Now we hook the business logic to the _stream_ of `_eventStreamController`, and
put the initial data of `Model` to the _sink_ of `_stateStreamController`. We
also close all streams in the `dispose` method.

```dart {lineNos=inline,hl_lines="21-30"}
class Bloc {
  late final Model _model;
  final StreamController<Event> _eventStreamController = StreamController();
  final StreamController<String> _stateStreamController = StreamController();

  StreamSink<Event> get eventSink => _eventStreamController.sink;
  Stream<String> get stateStream => _stateStreamController.stream;

  void performLogic(Event event) {
    switch(event) {
      case Event.eventOne:
        onEventOne(_model);
      case Event.eventTwo:
        onEventTwo(_model);
      case Event.eventThree:
        onEventThree(_model);
    }
    _stateStreamController.sink.add(_model.data);
  }

  Bloc({required Model model}) {
    _model = model;
    _stateStreamController.sink.add(_model.data);
    _eventStreamController.stream.listen(listener);
  }

  void dispose() {
    _eventStreamController.close();
    _stateStreamController.close();
  }
}
```

```goat
 .----------------------.-------------------------.---------------------.
 |             eventOne |_eventStreamController ->|        BLoC         |
 |                      '-------------------------'                     |
 |          UI          |                         | onEventOne(_model)  |
 |                      .-------------------------.                     |
 |           model.data |<- _stateStreamController|                     |
 '----------------------'-------------------------'---------------------'
```

## Hooking the UI

Flutter provides a neat widget [StreamBuilder][9] that builds UI depending on
the data received from a stream, so using that in this context is a no brainer.
But how do we access the `Bloc` class in the first place?

### "Providing" the BLoC

Making data available for widgets that need it can be done by defining a
`BlocProvider` class which extends [InheritedWidget][10], which makes it
available to any widget down in the widget tree if need be.

```dart {lineNos=inline}
class BlocProvider extends InheritedWidget {
  final Bloc bloc;
  const BlocProvider({
    super.key,
    required this.bloc,
    required super.child,
  });

  @override
  bool updateShouldNotify(BlocProvider oldWidget) => true;

  static BlocProvider? of(BuildContext context) =>
      context.dependOnInheritedWidgetOfExactType<BlocProvider>();
}
```

When we add this widget at the top of the widget tree, every widget under it
gets access:

```dart {lineNos=inline,hl_lines="8-11"}
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'BLoC Demo',
      home: BlocProvider(
        bloc: Bloc(model: Model(0)),
        child: const MyHomePage(title: 'BLoC Demo Home Page'),
      ),
    );
  }
}
```

### Using the BLoC

Widgets that need the BLoC to send events and receive states need to be called
as [StatefulWidget][11]. The lifecycle methods available to those widgets help
in proper initialization and disposal of streams. In the following snippet the
`Text` widget is built depending on the data received from the BLoC and the
`TextButton` sends events to the BLoC when it is pressed.

```dart {lineNos=inline,hl_lines="12-15 23-29 33 42-45"}
class BlocConsumer extends StatefulWidget {
  const BlocConsumer({super.key});

  @override
  State<BlocConsumer> createState() => _BlocConsumerState();
}

class _BlocConsumerState extends State<BlocConsumer> {
  late final Bloc? _bloc;

  @override
  void didChangeDependencies() {
    _bloc = BlocProvider.of(context)?.bloc;
    super.didChangeDependencies();
  }

  @override
  Widget build(BuildContext context) {
    if (_bloc == null) return Container();
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        StreamBuilder(
          stream: _bloc.stateStream,
          builder: (context, snapshot) => Text(
            '${snapshot.data}',
            style: Theme.of(context).textTheme.headlineMedium,
          ),
        ),
        Container(
          margin: const EdgeInsets.only(top: 16),
          child: TextButton(
            onPressed: () => _bloc.eventSink.add(Event.eventOne),
            child: const Text('Send Event'),
          ),
        ),
      ],
    );
  }

  @override
  void dispose() {
    _bloc?.dispose();
    super.dispose();
  }
}
```

We mainly use two lifecycle methods:

- [`didChangeDependencies`][12]: Here we access the bloc from `BlocProvider`.
Defining here would make the widget itself rebuild when `BlocProvider` is
changed.
- [`dispose`][13]: Here we call the `Bloc.dispose` method to dispose the defined
streams whenever the state of the widget itself is disposed.

There we have it, BLoC implemented using built in Dart libraries. Although not
recommended to be used in production, this could be an useful exercise to
understand BLoC implementation wise. There are packages that do the same thing
but with [less][14] and [even lesser][15] code with the ability to minutely
track events from the UI event leading to the change of state, from old to the
new. The best resource to get started to know more about is at the [bloc library's official website][16].

[1]: https://royarg.medium.com/flutter-bloc-from-the-perspective-of-a-beginner-d249d907eeff
[2]: https://api.dart.dev/stable/dart-async/Future-class.html
[3]: https://api.dart.dev/stable/dart-async/Stream-class.html
[4]: https://pub.dev/packages/bloc
[5]: https://pub.dev/packages/flutter_bloc
[6]: https://api.dart.dev/stable/dart-async/StreamController-class.html
[7]: https://api.dart.dev/stable/dart-core/Enum-class.html
[8]: https://dart.dev/language/class-modifiers#sealed
[9]: https://api.flutter.dev/flutter/widgets/StreamBuilder-class.html
[10]: https://api.flutter.dev/flutter/widgets/InheritedWidget-class.html
[11]: https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html
[12]: https://api.flutter.dev/flutter/widgets/State/didChangeDependencies.html
[13]: https://api.flutter.dev/flutter/widgets/State/dispose.html
[14]: https://bloclibrary.dev/#/coreconcepts?id=bloc
[15]: https://bloclibrary.dev/#/coreconcepts?id=cubit
[16]: https://bloclibrary.dev
