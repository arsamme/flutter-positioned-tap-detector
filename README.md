# positioned_tap_detector_2
[![Fork](https://img.shields.io/github/forks/arsamme/flutter-positioned-tap-detector?style=social)](https://github.com/arsamme/flutter-positioned-tap-detector/fork)&nbsp; [![Star](https://img.shields.io/github/stars/arsamme/flutter-positioned-tap-detector?style=social)](https://github.com/arsamme/flutter-positioned-tap-detector)&nbsp; [![Watches](https://img.shields.io/github/watchers/arsamme/flutter-positioned-tap-detector?style=social)](https://github.com/arsamme/flutter-positioned-tap-detector/)&nbsp; [![pub package](https://img.shields.io/pub/v/positioned_tap_detector_2.svg)](https://pub.dartlang.org/packages/positioned_tap_detector_2)&nbsp; [![Example](https://img.shields.io/badge/Example-Ex-success)](https://pub.dev/packages/positioned_tap_detector_2/example)


Flutter widget allowing to receive tap callbacks, together with their position on the screen. It supports `onTap`, `onDoubleTap` and `onLongPress` gestures. Each callback function is invoked with `TapPosition` object that provides `global` and `relative` touch position. To adjust maximum time allowed between double-tap gesture consecutive taps, specify an additional `doubleTapDelay` parameter:

```Dart
PositionedTapDetector(
  onTap: (position) => _printTap('Single tap', position),
  onDoubleTap: (position) => _printTap('Double tap', position),
  onLongPress: (position) => _printTap('Long press', position),
  doubleTapDelay: Duration(milliseconds: 500),
  child: ...,
)

void _printTap(String gesture, TapPosition position) => 
    print('$gesture: ${position.global}, ${position.relative ?? '-'}');
```

![PositionedTapDetector demo](https://thumbs.gfycat.com/SameTautHammerheadbird-small.gif)

### Controller

In case you need to wrap another `GestureDetector` below in the widget tree, pass an additional `PositionedTapController` parameter and invoke its callback methods whenever relevant gesture takes place:

```Dart
final _controller = PositionedTapController();

Widget build(BuildContext context) {
  // ...
    child: PositionedTapDetector2(
      // ...
      controller: _controller,
      child: GestureDetector(
        onTap: _handleTap,
        onTapDown: _controller.onTapDown,
        onLongPress: _controller.onLongPress,
        behavior: ...,
        child: ...,
      ),
    ),
}

void _handleTap() {
  // ...
  _controller.onTap();
}
```

### Drawbacks

`PositionedTapDetector2` will not invoke *onDoubleTap* callback in case there's `GestureDetector` underneath it in the widget tree **that also specifies *onDoubleTap* parameter**. Since Flutter framework doesn't invoke its `onTapDown` callback when detector is double-tapped, *positioned detector* is unable to receive tap positions and therefore detect double-tap gestures.
