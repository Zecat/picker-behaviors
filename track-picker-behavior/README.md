## Polymer.TrackPickerBehavior

A Polymer behavior to detect the track / tap on pickable element and notify the active target.

### Demo & doc

See [component page](http://zecat.github.io/picker-behaviors/components/picker-behaviors/#Polymer.TrackPickerBehavior)

### How does it work

The picking begins when the user click or touch an element that contains the class defined by `_pickableClass` attribute. It is stored in `_lastActiveTarget` and updated as the cursor moves from an element to another. The picking stops when the user releases the cursor.

- `touchPickDelay` - in ms, default to 500 - allows the picking to start after the user stay a while on the same target. 
- `touchPickMaxYTremor` prevents begin picking when the track starts vertically.

### Import

```html
<link rel="import" href="/bower_components/picker-behaviors/track-picker-behaviors/track-picker-behavior.html">
```
