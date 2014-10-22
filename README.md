# Mobify Pinny

A mobile-first content fly-in UI plugin.

![Pinny in action](https://raw.githubusercontent.com/mobify/pinny/master/examples/assets/i/pinny.gif "Pinny in action")

## Requirements

* [Zepto](http://zeptojs.com/)
* [Mobify's fork of Velocity.js](http://github.com/mobify/velocity)

### Velocity

If you are using Zepto, you need to load `bower_components/mobify-velocity/velocity.js` (this file comes with a jQuery shim bundled directly in it). If you are using jQuery, you need to load `bower_components/velocity/jquery.velocity.js`.

### jQuery Support

Pinny supports jQuery but is not actively developed for it. You should be able to use Pinny directly with jQuery 2.0. While we don't actively support jQuery for Pinny, we welcome any and all issues and PRs to help us make it work.


## Installation

Pinny can be installed using bower:

```
bower install https://github.com/mobify/pinny.git#pinny-1.0
```

## Usage with Require.js

To use with require.js, after installing through bower you merely have to reference Pinny, Pinny's effect modules, and Pinny's dependencies (velocity, shade, and bouncefix) inside your require config file:

```config.js

{
    'paths': {
    	'plugin': 'bower_components/plugin/dist/plugin.min',
        'pinny': 'bower_components/pinny/dist/pinny.min',
        'modal-center': 'bower_components/pinny/dist/effect/modal-center',
        'sheet-bottom': 'bower_components/pinny/dist/effect/sheet-bottom',
        'sheet-left': 'bower_components/pinny/dist/effect/sheet-left',
        'sheet-right': 'bower_components/pinny/dist/effect/sheet-right',
        'sheet-top': 'bower_components/pinny/dist/effect/sheet-top',
        'shade': 'bower_components/shade/dist/shade.min',
        'velocity': 'bower_components/mobify-velocity/velocity',
        'bouncefix': 'bower_components/bouncefix.js/dist/bouncefix.min'
    }
}

```

And then require pinny in as needed:

```
define([
    'zepto',
    'modal-center',
    'pinny'
    ],
    function($, modalCenter) {
        $('.pinny').pinny({
        	effect: modalCenter
        });
    }
);
```


## Usage

Pinny requires very minimal markup. All Pinny needs is a div with your content and it will automatically transform into what we need.

> To avoid any unwanted FOUT, decorate the content you will be passing to Pinny with the `hidden` attribute. We will remove that attribute when Pinny is initialized.

For accessibility and functional purposes, Pinny will wrap all of your body content in a wrapping container. This could conflict with other plugins that alter your page's markup. If you're seeing issues, try initializing Pinny after your other plugins. If you want to specify your own wrapping container, add a class of `pinny__body-wrapper` to the element. This element should be a root level element to be effective.

```html
<!-- Include the CSS -->
<link rel="stylesheet" href="pinny.min.css">

<!-- Optionally include the Theme file -->
<link rel="stylesheet" href="pinny-style.min.css">

<!-- Optionally include a wrapping container -->
<div id="bodyContent" class="pinny__body-wrapper">
    Your specified body content
</div>

<!-- Include the markup -->
<div id="yourPinny" hidden>
	Your pinny content
</div>

<!-- Include dependencies -->
<script src="zepto.min.js"></script>
<script src="velocity.min.js"></script>

<!-- Include the effect module you want to use -->
<script src="effect/modal-center.js"></script>
<!-- Include pinny.js -->
<script src="pinny.min.js"></script>

<!-- Construct Pinny -->
<script>$('#yourPinny').pinny()</script>
```

## Initializing the plugin

### pinny()

Initializes the pinny.

```js
$('#myPinny').pinny({
	effect: modalCenter
});
```

You can also initialize the Pinny through the use of a data attribute. The attribute takes a value equal to the effect you want to use.

```html
<div id="myPinny" data-pinny="sheet-bottom">
```

_You *must* pass Pinny an effect for it to work._

### pinny(options)

Initialize with options.

```js
$('#myPinny').pinny({
    effect: sheetBottom,
    container: '#container',
    structure: {
        header: 'My Pinny Title',
        footer: false
    },
    zIndex: 2,
    cssClass: 'my-pinny-class',
    coverage: '100%',
    easing: 'swing',
    duration: 200,
   	shade: {
        color: '#404040'
    },
    open: noop,
    opened: noop,
    close: noop,
    closed: noop
});
```

#### Options

##### effect

default: `{
        open: noop,
        close: noop
    },`

Specifies which `effect` module Pinny should use when opening. `Effect` modules allow you to load specific functionality that tell Pinny how to open and close. Available `effect` modules can be found in the `dist/effect` folder. Current `effect` modules include:

- Modal Center - opens Pinny in the center of the screen
- Sheet Top - slides down from the top of the screen
- Sheet Bottom - slides up from the bottom of the screen
- Sheet Left - slides in from the left of the screen
- Sheet Right - slides in from the right of the screen

```js
$('#myPinny').pinny({
    effect: sheetLeft
});
```

#### container

default: `body`

Specify the container the pinny will be created within

```js
$('#myPinny').pinny({
    container: $('#mainForm') // or container: '#mainForm'
});
```

##### structure

default: `body`

Specify the container the pinny will be created within

```js
$('#myPinny').pinny({
    container: $('#mainForm') // or container: '#mainForm'
});
```


default: `{
            header: '',
            footer: false
        }`

Defines the structure to use for pinny. Specifically, pinny tries to build its own HTML structure if passed the default options. Setting the `structure` option to false will allow the developer to supply their own HTML structure.

Please see below for available sub-options for `header` and `footer`.

###### header

default: `''`

Sets the header that pinny should use in its header bar. Valid values are:

- `boolean` - specifies no default header generated. If chosen, the user is required to specify the header markup themselves, including the appropriate class, `pinny__header`. It will be expected that this will be a part of the element that is used to invoke pinny.
- `string` - specifies the title text used in the header. The header structure will be generated automatically.
- `html|element` - specifies the HTML to be used for the header.

```js
// generates no header
$('#myPinny').pinny({
	header: false
});
```
or

```js
// generates a default header with the title "My Pinny"
$('#myPinny').pinny({
	header: 'My Pinny'
});
```

or

```js
$('#myPinny').pinny({
	header: '<header class="pinny__header">My Pinny<button class="pinny__close">Close</button></header>'
});
```

###### footer

default: `false`

Sets the footer that pinny should use in its footer. Valid values are:

- `boolean` - specifies no default footer generated. If chosen, the user is required to specify the footer markup themselves, including the appropriate class, `pinny__footer`.
- `string` - specifies the title text used in the footer. The footer structure will be generated automatically.
- `html|element` - specifies the HTML to be used for the footer.

```js
// generates no footer
$('#myPinny').pinny({
	footer: false
});
```
or

```js
// generates a default footer with the contents "My Footer"
$('#myPinny').pinny({
	footer: 'My Footer'
});
```

or

```js
$('#myPinny').pinny({
	footer: '<footer class="pinny__footer">My Footer</footer>'
});
```

##### zIndex

default: `2`

Sets the z-index value for Pinny. Use this value if you need to specify a specific stacking order.

```js
$('#myPinny').pinny({
    zIndex: 10
});
```

##### cssClass

default: `''`

Sets a class to apply to the main Pinny parent element for ease of styling.

```js
#('#myPinny').pinny({
    cssClass: 'my-pinny-class'
});
```

##### coverage

default: `80%`

Sets the coverage value. This will allow you to specify that the pinny covers only a portion of the screen.

```js
$('#myPinny').pinny({
    coverage: '80%'
});
```

##### duration

default: `200`

Sets the duration for the animation.

```js
$('#myPinny').pinny({
    duration: 600
});
```

##### shade

default: `{}`

Specifies whether pinny should use the shade overlay. You can pass options through to [Shade](http://github.com/mobify/shade) using this property. For more information on available options, see the [Shade documentation](http://github.com/mobify/shade).

> **Warning:** We currently force Shade's duration to match the one provided to Pinny. This is to limit DOM thrashing as much as possible during Pinny's animation. Pinny hitches a little if we don't do this.

```js
$('#myPinny').pinny({
    shade: {
        color: '#333333'
    }
});
```

##### easing

default: `swing`

Sets the easing for the animation. Pinny takes all of the same easing properties that [Velocity.js](http://julian.com/research/velocity) accepts.

> * [jQuery UI's easings](http://easings.net/) and CSS3's easings ("ease", "ease-in", "ease-out", and "ease-in-out"), which are pre-packaged into Velocity. A bonus "spring" easing (sampled in the CSS Support pane) is also included.
* CSS3's bezier curves: Pass in a four-item array of bezier points. (Refer to [Cubic-Bezier.com](http://cubic-bezier.com/) for crafing custom bezier curves.)
* Spring physics: Pass a two-item array in the form of [ tension, friction ]. A higher tension (default: 600) increases total speed and bounciness. A lower friction (default: 20) increases ending vibration speed.
* Step easing: Pass a one-item array in the form of [ steps ]. The animation will jump toward its end values using the specified number of steps.

For more information, check out [Velocity's docs on easing](http://julian.com/research/velocity/#easing).

```js
$('#myPinny').pinny({
    easing: 'ease-in-out'
});
```

##### open

default: `function(e, ui) {}`

Triggered every time the selected pinny item is starting to open.

**Parameters**

| Parameter name | Description |
|----------------|-------------|
| **e** | An Event object passed to the callback |
| **ui** | An object containing any associated data for use inside the callback |

```js
$('#myPinny').pinny({
    open: function(e, ui) {
        // ui.item contains the item opening
    }
});
```

##### opened

default: `function(e, ui) {}`

Triggered every time the selected pinny item has finished opening.

**Parameters**

| Parameter name | Description |
|----------------|-------------|
| **e** | An Event object passed to the callback |
| **ui** | An object containing any associated data for use inside the callback |

```js
$('#myPinny').pinny({
    opened: function(e, ui) {
        // ui.item contains the item that opened
    }
});
```

##### close

default: `function(e, ui) {}`

Triggered every time an pinny item is starting to close.

| Parameter name | Description |
|----------------|-------------|
| **e** | An Event object passed to the callback |
| **ui** | An object containing any associated data for use inside the callback |

```js
$('#myPinny').pinny({
    close: function(e, ui) {
        // ui.item contains the item closing
    }
});
```

##### closed

default: `function(e, ui) {}`

Triggered every time an pinny item is finished closing.

| Parameter name | Description |
|----------------|-------------|
| **e** | An Event object passed to the callback |
| **ui** | An object containing any associated data for use inside the callback |

```js
$('#myPinny').pinny({
    closed: function(e, ui) {
        // ui.item contains the item that closed
    }
});
```

## Methods

### Open

Open the selected pinny item by element reference

```js
$pinny.pinny('open');
```

### Close

Close the selected pinny item by element reference

```js
$pinny.pinny('close');
```

> Pinny will automatically trigger `close` on elements decorated with the class name `pinny__close`.

```html
<button class="pinny__close">Close</button>
```

## Browser Compatibility

| Browser           | Version | Support                      |
|-------------------|---------|------------------------------|
| Mobile Safari     | 5.0+    | Degraded. Form inputs cause scroll issues while typing. |
| Mobile Safari     | 6.0+    | Supported.                   |
| Chrome (Android)  | 1.0+    | Supported.                   |
| Android Browser   | 4.0+    | Degraded. No scroll locking. |
| Android Browser   | 2.3.x   | Degraded. No transitions.    |
| IE for Win Phone  | 8.0+    | Degraded. No scroll locking. |
| Firefox (Android) | 23.0+   | Supported. (Support may exist for earlier versions but has not been tested) |

## Known Issues

Currently, form inputs and selects inside of Pinny have issues on iOS7 and under. This is due to not being able to lock scrolling without causing rendering issues as well as iOS attempting to scroll the page when the keyboard opens. Forms work but will cause some visual jumping.

## Building a distribution

### Requirements
* [node.js 0.10.x/npm](http://nodejs.org/download/)
* [Grunt](http://gruntjs.com/)
    * Install with `npm install -g grunt-cli`
* [Bower](http://bower.io/)
    * Install with `npm install -g bower`

### Steps
1. `npm install`
1. `bower install`
1. `grunt build-dist`

The `dist` directory will be populated with minified versions of the css and javascript files for distribution and use with whatever build system you might use. The `src` directory has our raw unminified Sass and Javascript files if you prefer to work with those.

## License

_MIT License. Pinny is Copyright © 2014 Mobify. It is free software and may be redistributed under the terms specified in the LICENSE file._
