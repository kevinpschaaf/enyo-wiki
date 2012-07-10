# Gestures

Enyo supports a set of cross-platform gesture events that work similarly on	all
supported platforms. These events are provided so that users can write a single
set of event handlers for applications that run on both mobile and desktop
platforms.  They are needed because desktop and mobile platforms handle basic
gestures differently.  For example, desktop platforms provide mouse events,
while mobile platforms support touch events and a limited set of mouse events
for backward compatibility.

## Gesture Events

Enyo provides the following gesture events, which are synthesized from the
available DOM events:

* `down` is generated when the pointer is pressed down.

* `up` is generated when the pointer is released up.

* `tap` is generated when the pointer is pressed down and released up.  The
	target is the lowest DOM element that received both	the related `down` and
	`up` events.

* `move` is generated when the pointer moves.

* `enter` is generated when the pointer enters a DOM node.

* `leave` is generated when the pointer leaves a DOM node.

Please note that Enyo's gesture events are generated on Enyo controls, not DOM
elements.

## Gesture Event Properties

Gesture events have the following common properties, when available:

* `target`
* `relatedTarget`
* `clientX`
* `clientY`
* `pageX`
* `pageY`
* `screenX`
* `screenY`
* `altKey`
* `ctrlKey`
* `metaKey`
* `shiftKey`
* `detail`
* `identifier`
