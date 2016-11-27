## Polymer.SamplesPickerBehavior

A Polymer behavior that helps you create a picker element where the user can select samples by tracking or tapping on elements.

It listens users mouse and touch actions on a theoretically infinit list of elements and tells you to set them as `selected` or `selecting`.

This behavior implements [Polymer.TrackPickerBehavior](/track-picker-behavior).

### Demo & doc

See [component page](http://zecat.github.io/picker-behaviors/components/picker-behaviors/#Polymer.SamplesPickerBehavior)

### Check out the icon-picker demo

In this example, when you track on elements, the blue icons are in `selecting` state, the grey background icons are in `selected` state.

Blue icons represent an interval from the first picked element to the current active element. When the user is picking and an element gain or lose the `selecting` state, his `selected` state is toggled. When the cursor is released, items loose the `selecting` state but keep there current `selected` state.

### How to use it

Create a Polymer element and implement this behavior. A convenient way to save the selected and selecting state of your items is to store them in an array.
Let say, for example, you have a property `contacts` of type Array with value :
```json
[ { "name": "John" }, { "name": "Doe" }, { "name": "Paul" }, { "name": "Peter" } ]
```

#### 1) Generate pickable elements and fetch there selection state
Iterate on your array to generate a list of elements with the class corresponding to `_pickableClass` attribute - default to `pickable`.
Later we will tell the behavior to update `selected` and `selecting` properties (Boolean) of our items when the user pick items. Let's give them the corresponding class so we can style them.

Using dom-repeat :
```html
    <template is="dom-repeat" items="[[contacts]]">
        <div class="pickable {{getSelectedClass(item.selected)}} {{getSelectingClass(item.selecting)}}">[[item.name]]</div>
    </template>
```
```js
    getSelectedClass: function(selected) {
        if (selected) { return 'selected'; }
    },
    getSelectingClass: function(selecting) {
        if (selecting) { return 'selecting'; }
    },
```
```css
    .pickable {
        width: 100%;
        height: 40px;
    }
    .selected {
        color: green;
        background: lightgrey;
    }
    .selecting {
        color: blue
    }
```

*\*Ensure user can point them with the cursor. In some cases you will have to play with `pointer-event` or `z-index` on certain nodes to access your pickable element. Also if you are using images, set `draggable=false`.*

#### 2) Index your elements
To work, this behavior needs to know if an element is before or after another one according to your logic. You can for exemple set a property or an attribute on your items when you generate them. If you are using dom-repeat, items are already indexed.

#### 3) Get the index of an element from its target
In your implementation, you have to override `_getIndexFromTarget(target)` so the behavior know the index of the element picked by the user.
Using dom repeat:
```js
    _getIndexFromTarget(target) {
        return this.$.domRepeat.indexForElement(target);
    }
```
Or if you store the index as an attribute, let say "data-index" :
```js
    _getIndexFromTarget(target) {
        return target.getAttribute('data-index');
    }
```

#### 4) Tells the behavior how to modify the selecting and selected states of your elements
To do so, you need to override `_toggleSelectingAndSelectedStateForIndex(index)` and `_removeSelectingStateForIndex(index)` with your own logic. Theses methods will be call automatically as the user pick items.


• In `_toggleSelectingAndSelectedStateForIndex(index)`, **toggle** the **selected** and **selecting** state of the element at the given index.

In our example :

```js
_toggleSelectingAndSelectedStateForIndex(index) {
    this.set('contacts.'+index+'.selected', !this.contacts.selected);
    this.set('contacts.'+index+'.selecting', !this.contacts.selecting);
}
```

• In `_removeSelectingStateForIndex(index)`, **remove** the **selecting** state of the element at the given index.

In our example :

```js
_removeSelectingStateForIndex(index) {
    this.set('contacts.'+index+'.selecting', false);
}
```

That's it !

### Using non-linear indexes

If your indexes are not linear (0, 1, 2, 3, 4 …) because for example, you create a date-picker and you use timestamps as indexes. Then you should override the following methods :

- `_toggleSelectingAndSelectedStateForInterval(startIndex, endIndex)`
- `_removeSelectingStateForInterval(startIndex, endIndex)`
- `_getNextIndex(index)`
- `_getPreviousIndex(index)`

*\* some methods and attributes name are '_' prefixed so they doesn't appear in your final element documentation. Click 'SHOW PRIVATE API' on the component page to see the full documentation.*


### Import

```html
<link rel="import" href="/bower_components/picker-behaviors/samples-picker-behavior/samples-picker-behavior.html">
```
